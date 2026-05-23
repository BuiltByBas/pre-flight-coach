# Security Blind Spots, the Safety Check catalog

This is the catalog the **Pre-Flight Safety Check** sweeps against (see [safety-check.md](safety-check.md)). It is a codebase-agnostic list of the security failures that AI code review and AI-assisted building most consistently miss, adapted from the *AI Security Blind Spots: Pen-Test Reference* by **BuiltByBas LLC**.

You do not run all of it at once, and you do not dump it on the user. It loads only when a `/safety` sweep runs.

## How you use it

- **One category per pass.** Run the codebase through one section at a time; combining them in a single sweep degrades attention. This is the same instinct as the rest of the coach: one thing at a time.
- **Select intelligently for the stack.** A beginner's static page does not get the JWT, GraphQL, or container passes. Read what the user actually built, pick the categories that can apply, and skip the rest, the same judgment the mode routing uses.
- **Detection signals are what you look for**, the concrete things to grep, lint, or read. **Prevention is what you coach**, the fix, taught at the user's level.
- **Adversarial framing.** "Is this secure?" gets "looks fine." Ask instead, of each category, "how would someone break this?" and enumerate the instances in the real code.
- **Verify before you assert.** Every finding is something you saw in their code, not a guess. If you cannot point to the line, you do not claim the bug. This is the BuiltByBas standard.
- **Coach, never dump.** For each real finding: name it, say why it matters in plain terms, show the fix. Never hand over the raw catalog as a checklist.

---

## I. Input handling and injection

- **SQL injection** — untrusted input concatenated into a query. *Detect:* string-built queries, ORM `.raw()`/`$queryRawUnsafe`, interpolated table/column names (parameterization does NOT cover identifiers). *Prevent:* parameterized queries only; allow-list dynamic identifiers; least-privilege DB user.
- **NoSQL injection** — operator injection (`{$ne:null}`, `$where`) via untyped JSON. *Detect:* `req.body` fields used directly in filters; no schema validation before the DB call. *Prevent:* strict schema validation (Zod/Joi/Pydantic); coerce scalars; disable server-side JS.
- **Command / OS injection** — user input reaches a shell. *Detect:* `exec`/`spawn`/`system` with concatenated strings, `shell=True`, `child_process.exec` vs `execFile`. *Prevent:* argument-array forms (`execFile`, `shell=False`); never build a shell string; command allow-list.
- **LDAP / XPath / XQuery injection** — metacharacters alter directory/XML query semantics. *Detect:* string-built LDAP filters or XPath from request data. *Prevent:* parameterized filters; precompiled XPath with bindings.
- **XXE (XML external entity)** — parser resolves external DTDs/entities (SSRF, file read, billion-laughs). *Detect:* XML parsers without explicit feature flags; SOAP/SAML/SVG/DOCX endpoints. *Prevent:* disable DTDs and external entities; use a hardened parser (defusedxml); prefer JSON.
- **Server-side template injection (SSTI)** — user input rendered AS a template reaches engine eval. *Detect:* `render_template_string`, `Template(user_input)`, user-controlled email/notification templates. *Prevent:* render data INTO a fixed template, never render user input as one.
- **XSS (reflected/stored/DOM)** — untrusted data in HTML/JS/attribute/URL/CSS context without context-correct encoding. *Detect:* `innerHTML`, `dangerouslySetInnerHTML`, `v-html`, `href`/`src` from input (javascript: URIs), JSON inlined in `<script>`. *Prevent:* context-aware encoding; strict CSP (no unsafe-inline/eval); DOMPurify for genuine raw HTML; URL scheme allow-list.
- **Prototype pollution (JS)** — attacker JSON merged into `Object.prototype`. *Detect:* `_.merge`/`Object.assign`/recursive copy of user input; user-controlled keys reaching assignment. *Prevent:* reject `__proto__`/`constructor`/`prototype` keys; `Object.create(null)`; `Map` for arbitrary keys.
- **Mass assignment / overposting** — request JSON bound straight to a model sets `isAdmin`/`role`/`ownerId`. *Detect:* `Model.create(req.body)`, no DTO/allow-list, `fillable: '*'`. *Prevent:* explicit DTOs/allow-lists; re-derive sensitive fields server-side (`ownerId = req.user.id`).
- **Insecure deserialization** — `pickle`/`yaml.load`/Java `readObject`/`unserialize` running gadget chains. *Detect:* native deserializers on untrusted data, serialized objects in cookies. *Prevent:* JSON only on trust boundaries; `yaml.safe_load`; signed envelopes if unavoidable.
- **HTTP parameter pollution, header/CRLF injection, open redirect** — duplicate params parsed inconsistently; CR/LF in headers; redirect target from query. *Detect:* `res.redirect(req.query.next)`, user data in headers, array-vs-scalar confusion. *Prevent:* strip CR/LF; redirect allow-list or same-origin; canonicalize params at the edge.
- **ReDoS** — catastrophic regex backtracking on attacker input. *Detect:* nested quantifiers `(a+)+`, regex on long/user strings, user-supplied regex. *Prevent:* linear-time engines (RE2); input length caps + regex timeouts.
- **Path traversal / zip slip** — `../` or absolute paths escape the intended directory; archive entries overwrite files. *Detect:* `path.join(base, userInput)` without a resolved-path check, archive extraction without entry validation. *Prevent:* resolve to absolute then verify `startsWith(base + sep)`; reject `..`/absolute/symlink entries; Unicode-normalize first.
- **File upload abuse** — Content-Type/extension trust, executable upload to web root, image bombs. *Detect:* trust of client MIME, files under web root, no resource limits. *Prevent:* magic-byte sniff + re-encode; store outside web root; random filenames; sandboxed processing; AV scan.
- **Unicode / encoding confusion** — homoglyphs, NFC vs NFKC, RTL override, double-decoding across layers. *Detect:* identity comparisons without normalization, allow-list pre-decode but sink post-decode. *Prevent:* one normalization form at the boundary; reject control/bidi chars in identifiers; confusables detection.

## II. Authentication

- **Weak/missing MFA enforcement** — MFA on login but not on token-refresh, recovery, or sensitive changes. *Detect:* MFA-gated login but unguarded API/recovery paths. *Prevent:* server-side policy enforcing MFA for elevation, recovery, and high-value changes; phishing-resistant factors for admins.
- **Password storage misuse** — MD5/SHA1, unsalted, fast hashes, low bcrypt cost. *Detect:* bcrypt cost < 12, shared salt, `==` hash comparison. *Prevent:* Argon2id or bcrypt cost ≥ 12; per-user salts; constant-time compare.
- **Credential stuffing / brute force** — no rate limit on login, reset, or MFA-verify. *Detect:* no per-account/per-IP limit, unrate-limited MFA verify, username enumeration via differential responses. *Prevent:* rate limit per username AND IP; breached-password check; identical responses/timing for valid/invalid users.
- **Session fixation** — session ID not rotated on login/MFA/privilege change. *Detect:* login returns the existing session ID. *Prevent:* regenerate session ID on every auth state change; invalidate the old one.
- **JWT misuse** — `alg=none`, RS256/HS256 confusion, no `exp`/`aud`/`iss` validation, secret in client, `kid` path traversal. *Detect:* `verify` without an algorithms allow-list, long-lived tokens, `kid` as a file path. *Prevent:* pin the algorithm; validate all claims every time; short access-token lifetime; treat `kid` as an opaque registry key; re-check authority server-side.
- **OAuth/OIDC flaws** — missing state/PKCE, loose `redirect_uri`, token in URL. *Detect:* state not session-bound, prefix/wildcard redirect match, implicit flow. *Prevent:* Authorization Code + PKCE; exact-match `redirect_uri`; validate `id_token` signature/iss/aud/nonce.
- **Account recovery weakness** — email-only reset, guessable security questions, recovery resets MFA without proofing. *Detect:* MFA reset via support without verification, tokenless reset codes. *Prevent:* rate-limited recovery codes; step-up proof for MFA reset; notify on all attempts.
- **Default / hardcoded credentials** — `admin/admin`, sample keys committed, build-time secrets in images. *Detect:* known initial passwords, secrets in Dockerfile ENV. *Prevent:* force password set on first run; secrets at runtime only, never in image layers.

## III. Authorization and access control

- **IDOR / BOLA** — endpoint returns/modifies an object by ID without checking the caller owns it (OWASP API #1). *Detect:* `/api/orders/:id` with route-level (not resource-level) authz, tenant isolation only in UI state. *Prevent:* every read/write filters by owner/tenant IN the query; policy-as-code, deny-by-default; test every endpoint × role × cross-tenant.
- **Function-level authorization gaps** — admin endpoints reachable by guessing the path; role checked on the UI only. *Detect:* `/admin/*` gated by frontend route guard, inconsistent middleware. *Prevent:* deny-by-default; every route declares a required role/scope; route-inventory diff per release.
- **Role/scope conflation** — a scope used as a role; a role for one feature checked for another. *Detect:* `scope='read'` permitting writes, one `admin` role spanning domains. *Prevent:* separate scopes (capabilities) from roles; per-feature granularity.
- **Vertical privilege escalation** — user elevates own role via mass assignment or a weak admin path. *Detect:* `PATCH /me` accepting `role`, internal flags writable publicly. *Prevent:* role changes only via a dedicated, audited endpoint; server re-derives role each request.
- **Horizontal privilege escalation (cross-tenant)** — user A reads tenant B's data. *Detect:* query lacks `tenant_id`, caches keyed without tenant, background jobs running cross-tenant. *Prevent:* `tenant_id` in every query (ORM scope / DB row-level security); tenant-prefixed cache keys.
- **TOCTOU** — state checked, then acted on; attacker mutates in between. *Detect:* permission check then action without an atomic guard; balance check then debit without a transaction. *Prevent:* atomic ops (`SELECT ... FOR UPDATE`, CAS); idempotency keys.
- **Confused deputy** — a service uses its own elevated privilege on behalf of a caller without checking the caller's authority. *Detect:* services calling each other with system creds + a user-ID param. *Prevent:* propagate caller identity; downstream re-checks authority; zero-trust between services.
- **GraphQL authz gaps** — missing field-level authz, introspection in prod, no depth/complexity limit. *Detect:* resolvers without a per-field auth predicate. *Prevent:* authorize every field; disable prod introspection; depth + complexity + cost limits.

## IV. Business logic

- **Race conditions in money/state flows** — parallel requests bypass a single-check guard (double-spend, coupon stacking, oversell). *Detect:* read-modify-write without a DB lock; balance checks in app code not constraints. *Prevent:* DB constraints (UNIQUE, CHECK ≥ 0); locking with versioning; idempotency keys; concurrency tests.
- **Workflow step-skipping** — step N doesn't verify steps 1..N-1; attacker calls N directly. *Detect:* each step authenticates the user but not the workflow state; hidden form fields trusted as state. *Prevent:* server-side state machine; signed continuation tokens; each handler asserts prerequisites.
- **Replay attacks** — captured request replayed (payment, vote, reset, MFA token). *Detect:* mutating endpoints without a nonce/idempotency key; reusable one-time tokens. *Prevent:* idempotency keys with a replay store; mark one-time tokens used atomically; webhook timestamp window + signature.
- **Negative / boundary value abuse** — negative-quantity refunds, integer overflow on price, fractional-cent drift. *Detect:* no min/max on numerics, floating-point money. *Prevent:* domain types with invariants; integer cents/decimal for money; property-based tests; DB CHECK constraints.
- **Pricing / discount stacking** — combining promos yields a negative or zero total. *Detect:* independent promo reducers with no conflict resolution, no price floor. *Prevent:* one pricing engine with precedence; floor at zero; combination test matrix.
- **Refund / reversal abuse** — refund after fulfillment, chargeback + refund double-credit. *Detect:* refund possible after the order moves to fulfilled, no PSP reconciliation. *Prevent:* state-machine constraints on refund eligibility; reconcile PSP webhooks against the ledger.
- **Resource exhaustion via legitimate endpoints** — wildcard search, huge export, repeated expensive aggregation. *Detect:* endpoints without pagination caps, synchronous large exports. *Prevent:* cursor pagination with a hard max; async export with quotas; cost-based rate limiting; DB query timeouts.

## V. Cryptography

- **Weak/deprecated algorithms** — MD5/SHA1 for integrity, DES/RC4, ECB mode (often the silent default). *Detect:* `Cipher.getInstance("AES")` (defaults to ECB), custom "encryption". *Prevent:* AES-GCM or ChaCha20-Poly1305; Ed25519/RSA-PSS for signatures; SHA-256+ for integrity.
- **Nonce / IV misuse** — static IV, reused GCM nonce (catastrophic), predictable counter. *Detect:* hardcoded IV/nonce. *Prevent:* random 96-bit IV under rate threshold; misuse-resistant AEAD (GCM-SIV) above it.
- **Insecure randomness** — `Math.random`/`rand()` for tokens/secrets. *Detect:* token generation calling a weak PRNG, UUIDv1 as a secret. *Prevent:* `crypto.randomBytes`/`secrets`/`getrandom()`; ≥ 128 bits of entropy.
- **Non-constant-time comparison** — `==`/`memcmp` on secrets leaks via timing. *Detect:* equality check on an HMAC/token/hash. *Prevent:* `hmac.compare_digest`/`timingSafeEqual`.
- **Key management failures** — keys in code/config, no rotation, shared across environments, one key for everything. *Detect:* long-lived static keys, same key for sign and encrypt. *Prevent:* KMS/HSM with envelope encryption; distinct keys per purpose; per-environment keys; rotation.
- **Rolling your own crypto** — custom encryption/handshake, `key||message` hashed as a MAC, XOR with a static key. *Detect:* crypto primitive code in the app repo, hand-rolled MAC. *Prevent:* vetted libraries (libsodium, Tink); standard protocols; external review before any custom crypto.
- **TLS validation disabled** — `verify=False`, `InsecureSkipVerify`, accept-all hostname verifier (often in "internal" calls). *Detect:* `verify=False`, a TrustManager returning true. *Prevent:* internal CA + mTLS; forbid `verify=False` in lint/CI.
- **Signature verification mistakes** — picking the verify key from the unvalidated payload (key confusion), verifying body but not headers. *Detect:* `kid` from token selects the key, webhook handler reads sender before verifying. *Prevent:* key from a trusted registry; verify before parsing; pin the algorithm at the verifier.
- **Padding oracle / MAC-then-encrypt** — CBC + PKCS#7 with distinguishable padding errors. *Detect:* AES-CBC in new code, different errors for padding vs MAC failure. *Prevent:* AEAD only; uniform error on any decryption failure.

## VI. SSRF and network egress

- **Classic SSRF** — server fetches a user-supplied URL pointed at internal IPs, cloud metadata, `file://`. *Detect:* any user URL reaching an HTTP/file client, PDF/HTML renderers loading arbitrary URLs, auto-followed redirects, DNS rebinding. *Prevent:* destination allow-list; resolve DNS then validate the IP against a deny-list (RFC1918, `169.254.169.254`, IPv6 equivalents) and connect to that IP; egress proxy; cloud IMDSv2 + hop-limit 1.
- **Blind SSRF** — no response to the attacker but internal side effects succeed. *Detect:* fire-and-forget HTTP to a user-controlled host. *Prevent:* same egress policy; outbound logging; segmentation so the app tier can't reach the control plane.
- **DNS rebinding** — DNS returns a public IP for validation, an internal IP for the fetch. *Detect:* validate-then-fetch by hostname, client re-resolves on retry. *Prevent:* resolve once, connect to that IP with the Host header set.
- **Cloud metadata exposure** — SSRF hits `169.254.169.254` for instance credentials. *Detect:* metadata reachable from app pods, IMDSv1 enabled, hop limit > 1. *Prevent:* IMDSv2 + hop-limit 1; block metadata IPs at pod egress; workload identity.
- **Open egress** — unrestricted outbound enables exfiltration and C2. *Detect:* no egress policy. *Prevent:* default-deny egress with a per-workload allow-list; pre-cache dependencies; egress logging.

## VII. Configuration, secrets, and deployment

- **Secrets in source / images / logs** — keys in git history, baked into Docker layers, printed in logs or error pages. *Detect:* high-entropy strings in `git log -p`, `docker history` ENV, `console.log(req)`, crash reporters capturing env/bodies. *Prevent:* pre-commit secret scanning + CI gate; runtime secret manager; `.dockerignore`; log scrubber; rotate any secret ever committed.
- **Debug / dev features in production** — debug toolbar, `/debug` routes, verbose stack traces, seed routes. *Detect:* `DEBUG=True` reachable in prod, health endpoint leaking version/env. *Prevent:* build-time exclusion of dev routes; generic prod error page; negative tests confirming dev endpoints are absent.
- **CORS misconfiguration** — `ACAO: *` with credentials, reflective origin, `null` origin allowed. *Detect:* `ACAO: *` + credentials true, origin reflected without an allow-list. *Prevent:* strict per-env origin allow-list; never combine credentials with a reflective origin; reject `null`.
- **Insecure cookies** — missing `Secure`/`HttpOnly`/`SameSite`, broad `Domain`, long `Max-Age`. *Detect:* session cookies without `Secure`, `SameSite=None` without `Secure`. *Prevent:* `Secure; HttpOnly; SameSite=Lax`; `__Host-` prefix; narrow domain; short lifetime.
- **Missing / weak security headers** — no HSTS, CSP, `X-Content-Type-Options`, `Referrer-Policy`. *Prevent:* HSTS preload; strict CSP with nonces; `nosniff`; `Referrer-Policy: strict-origin-when-cross-origin`; `Permissions-Policy`.
- **Insecure defaults carried forward** — framework dev defaults: open admin, sample users, default secret keys. *Detect:* bundled sample admin, default secret keys in config templates. *Prevent:* a hardening baseline in the starter template; CI scanning for known default values.
- **Container / IaC misconfig** — running as root, broad capabilities, `hostPath`, public buckets, `IAM Action:"*"`. *Detect:* Dockerfile without `USER`, pods without `securityContext`, wildcard IAM. *Prevent:* non-root, read-only rootfs, drop ALL caps; least-privilege IAM; IaC scanning (Checkov/tfsec).
- **Network segmentation gaps** — flat network; a compromised pod pivots freely. *Detect:* no NetworkPolicy, `0.0.0.0/0` on management ports. *Prevent:* default-deny ingress/egress; mTLS + authz policies; no public SSH/RDP.

## VIII. Dependencies, supply chain, build pipeline

- **Known vulnerable dependencies** — direct/transitive deps with CVEs. *Detect:* no SCA in CI, stale lockfile. *Prevent:* SCA blocking on high/critical; Dependabot/Renovate; SBOM per build.
- **Typosquatting / dependency confusion** — lookalike names, internal names published publicly, install scripts. *Detect:* public registry as primary for internal names, unscoped internal packages. *Prevent:* scoped internal packages; private registry policy; pin by hash; `--ignore-scripts`.
- **Unsigned / untrusted artifacts** — images/binaries/charts pulled without signature verification. *Detect:* pulls by mutable tag, no cosign/Sigstore. *Prevent:* sign artifacts; admission controllers verify; pin by digest.
- **Build pipeline compromise** — stolen CI tokens, malicious third-party Actions. *Detect:* Actions pinned by tag not SHA, secrets exposed to fork PRs. *Prevent:* pin Actions by SHA; OIDC federation (no long-lived CI secrets); ephemeral isolated runners.
- **Provenance gaps / outdated base image** — can't link binary to source; OS CVEs / EOL runtimes in the base. *Detect:* `:latest` base tags, EOL runtimes. *Prevent:* SLSA provenance; distroless/minimal base with nightly rebuilds; image scanning.

## IX. APIs, web, and frontend

- **CSRF** — state-changing request authed by ambient cookies without a per-request token. *Detect:* cookie-auth `POST/PUT/DELETE` without a token, GET endpoints with side effects. *Prevent:* `SameSite=Lax/Strict` + CSRF tokens; double-submit or signed Origin check; bearer-token auth where possible.
- **Clickjacking** — site framed by an attacker; clicks hijacked. *Detect:* missing `frame-ancestors`/`X-Frame-Options`, one-click sensitive actions. *Prevent:* CSP `frame-ancestors 'none'`; confirmation/re-auth for sensitive actions.
- **Web cache poisoning/deception** — unkeyed inputs influence a cached response served to others. *Detect:* response varies on a header not in `Vary`, personalized data without `no-store`. *Prevent:* explicit `Vary`; `Cache-Control: private, no-store` for personalized responses; normalize cache keys.
- **HTTP request smuggling** — front-end and back-end disagree on request boundaries. *Detect:* multiple proxies in the chain, mixed HTTP versions. *Prevent:* HTTP/2 end to end; strict parsing; reject ambiguous CL/TE; pre-prod smuggling tests.
- **WebSocket / SSE auth & origin** — no Origin check on upgrade, auth only at connect, messages not authorized per action. *Detect:* no Origin check in the upgrade handler. *Prevent:* Origin allow-list on upgrade; authorize each message; idle/abs timeouts.
- **Third-party script risk** — vendor JS runs with full DOM access; vendor compromise = user compromise. *Detect:* many external `<script src>`, no SRI, tag managers on payment pages. *Prevent:* SRI; strict `script-src`; keep third-party scripts off auth/payment pages; self-host vetted scripts.
- **Browser storage of secrets** — tokens in `localStorage` exposed to any XSS. *Detect:* `localStorage.setItem('token', ...)`, refresh tokens client-side. *Prevent:* `HttpOnly; Secure` cookies for session creds; short-lived access tokens in memory.
- **postMessage / cross-origin** — receiver doesn't verify `event.origin`; `targetOrigin: '*'`. *Detect:* `addEventListener('message')` without an origin check, `postMessage(..., '*')`. *Prevent:* strict origin allow-list; explicit `targetOrigin`; schema-validate the payload.

## X. Server-side and runtime

- **Memory safety (C/C++/unsafe Rust/cgo)** — overflow, use-after-free, integer overflow → undersized alloc. *Detect:* `memcpy`/`strcpy`/`sprintf` without bounds, user length as alloc size. *Prevent:* memory-safe languages; compiler hardening; sanitizers + fuzzing in CI.
- **Integer overflow / truncation** — arithmetic on attacker sizes wraps; alloc/index goes wrong. *Detect:* multiplied attacker ints used as a size, narrowing casts. *Prevent:* checked arithmetic; refuse oversized inputs at the boundary.
- **Concurrency bugs** — shared mutable state without sync; a security check races with use. *Detect:* globals mutated without a lock, cache-invalidation races. *Prevent:* immutable-by-default; race detectors; message-passing for hot paths.
- **Uncontrolled resource consumption** — unbounded memory/handles/threads/connections exhaust the host. *Detect:* buffered read of the whole body without a cap, thread-per-request without a pool. *Prevent:* hard caps on body/header/URL size; bounded pools with backpressure; ulimits/cgroups.
- **Server-side polyglot parsers** — YAML/Markdown/SVG parsers that execute or include external content. *Detect:* `yaml.load` vs `safe_load`, inlined uploaded SVGs, Markdown with raw HTML enabled. *Prevent:* safe loaders; sanitize/rasterize SVG; Markdown sanitizer in allow-list mode.
- **Improper error handling / info disclosure** — stack traces, query fragments, internal hostnames returned to users; differential responses leak existence. *Detect:* file paths/SQL in error responses, different codes for valid vs invalid usernames. *Prevent:* generic errors with a correlation ID; uniform enumeration-sensitive responses.
- **SSRF via server-side PDF/HTML renderers** — renderer fetches images/CSS over user content. *Detect:* `wkhtmltopdf`/headless Chrome over user content, `file://` enabled. *Prevent:* isolated network namespace; disable `file://`/`javascript:`; strip URLs or proxy through an allow-list.
- **Unsafe reflection / dynamic code** — `eval`/`Function()`/reflection dispatch driven by request data. *Detect:* `eval`, `new Function`, method dispatch by request string, dynamic import paths. *Prevent:* static dispatch tables/allow-lists; no eval on user content; sandbox if scripting is a real feature.

## XI. Data handling, privacy, and storage

- **Sensitive data in logs/telemetry** — tokens/PII/full bodies shipped to log aggregators and error trackers. *Detect:* logging full request/response, Sentry without scrubbing. *Prevent:* field allow-list/deny-list; redaction layer; PII tagging respected by serializers.
- **PII sprawl & excess retention** — personal data copied everywhere, kept forever. *Detect:* prod data restored to dev, no retention schedule. *Prevent:* data inventory + classification; tokenize/hash in non-prod; automated retention/erasure.
- **Encryption at rest misuse** — disk encryption assumed sufficient, one key for all tenants. *Detect:* full-disk only with no field-level encryption, single tenant key. *Prevent:* field-level/envelope encryption per record; per-tenant keys where warranted; strict KMS IAM.
- **Backup security** — backups unencrypted, shared, recoverable without MFA, retaining pre-redaction snapshots. *Detect:* backups in the same account/creds as prod, no restore tests. *Prevent:* cross-account immutable backups with object lock; separate MFA-required access; restore drills.
- **Data validation asymmetry** — validation at write only; imports/migrations introduce invalid state. *Detect:* bulk imports bypassing model validation, jobs trusting stored data. *Prevent:* DB-level constraints as the last line; imports through the same validation; sanity checks on read for high-stakes fields.
- **Insecure direct storage access** — over-broad signed URLs, public-read ACLs, presigned URLs without expiry. *Detect:* long-TTL presigned URLs, presigned PUT with arbitrary content-type. *Prevent:* short TTL + strict method/content-type; account-level block-public-access; server-mediated downloads for sensitive content.

## XII. Logging, monitoring, incident response

- **Insufficient security logging** — authn/authz/admin actions not logged or not retained; breach scope unknowable. *Detect:* login/role/MFA changes not logged with actor+target+outcome, retention < typical dwell time. *Prevent:* a standard audit schema (who/what/when/where/outcome/correlation-id); centralized SIEM, ≥ 1 year for auth/admin; tamper-evident storage.
- **Log injection** — newlines/control chars forge log entries (or log4j-style sinks). *Detect:* unescaped user strings in logs, lookup features enabled. *Prevent:* structured (JSON) logging; disable lookups; encode user fields.
- **Missing alerting** — logs exist but no one watches. *Detect:* no alerts on failed-login spikes, new admin role, mass export, IAM change. *Prevent:* detection-as-code with runbooks; anomaly detection on critical actions; tabletop exercises.
- **Insufficient token telemetry** — stolen tokens used silently post-revocation. *Detect:* refresh-token reuse not detected, no issuance metadata. *Prevent:* detect refresh-token reuse → invalidate the family; alert on use from a new ASN/country.

## XIII. Mobile, desktop, and client-side

- **Client-side trust boundary** — validation/authz/business rules only in the client, bypassed by a direct API call. *Detect:* price/eligibility/role computed client-side, "hidden" endpoints assumed private. *Prevent:* all trust decisions server-side; treat every endpoint as public; assume a hostile client.
- **Insecure local storage (mobile)** — secrets in plaintext prefs / in the app binary. *Detect:* tokens/PII in `SharedPreferences`/`UserDefaults`, hardcoded keys. *Prevent:* Keystore/Keychain; encrypted storage; backend-issued short-lived tokens, no long-lived secrets in the app.
- **Insecure inter-app communication** — exported activities/intents/URL schemes accept untrusted input; deep links bypass auth. *Detect:* `android:exported="true"` without permission, unverified App/Universal Links. *Prevent:* default `exported=false`; domain-verified links; validate deep-link params server-side.
- **Certificate pinning absent/broken** — client accepts any system-trusted CA; MITM via an installed root. *Prevent:* pin to leaf/intermediate with a backup pin; stricter pinning for payments/auth.
- **WebView misuse** — JS-to-native bridge exposed to arbitrary URLs; `file://` access. *Detect:* `JavaScriptInterface` without origin restriction, `setAllowUniversalAccessFromFileURLs`. *Prevent:* origin checks before the bridge; disable file/mixed content; separate WebViews for trusted vs untrusted content.

## XIV. AI / ML-specific (when the app uses LLMs or models)

- **Prompt injection (direct & indirect)** — untrusted text overrides instructions; indirect via documents, web content, tool outputs. *Detect:* user content concatenated into the system prompt, RAG over user-submitted content, tool outputs fed back without isolation. *Prevent:* treat model output as untrusted; never auto-execute tool calls without a policy gate; delimit user channels; output validators; human-in-the-loop for high-impact actions.
- **Tool / function-calling abuse** — broad tool access; injection makes the model call destructive tools. *Detect:* tools without per-call authz, escalating tools (shell/exec) available by default. *Prevent:* authorize each tool call as if from the user; confirmation for destructive tools; per-session tool allow-list; sandboxed code execution.
- **Training data leakage / memorization** — fine-tunes leak secrets; inference logs fed back into training. *Detect:* sensitive data in fine-tune corpora, inference logs flowing into the training set. *Prevent:* PII scrubbing; strict separation of inference logs from training; extraction tests pre-release.
- **Embedding / vector store authorization** — shared vector store returns documents the caller shouldn't see. *Detect:* single namespace across tenants, no metadata filter on retrieval. *Prevent:* tenant-scoped indices / server-side metadata filters; re-check authorization on retrieved docs; treat embeddings as sensitive.
- **Model output as code / SQL** — the app executes model-generated code/SQL/commands directly. *Detect:* generated SQL run against prod, generated shell run on the host. *Prevent:* parameterized templates the model only fills; strict operation allow-list; sandbox/read-only execution before commit.

## XV. Edge cases (added for the coach's everyday users)

These sit alongside the catalog above, common in first-projects and vibecoded apps:

- **World-readable secrets/config on disk** — files written with permissive modes (`0777`, world-readable `.env`). *Detect:* `chmod 777`, files created without a mode, secrets written to a shared/public path. *Prevent:* least-privilege file modes; secrets out of the served/shared directory.
- **Committed `.env` / dotfiles** — `.env`, key files, or service-account JSON committed to the repo. *Detect:* `.env` tracked in git, no `.gitignore` entry. *Prevent:* gitignore them; use a secret manager; rotate anything committed.
- **Debug output of sensitive data left in** — `console.log`/`print` of tokens, passwords, full request objects shipped to prod. *Detect:* logging of credential-bearing objects. *Prevent:* remove before ship; structured logging with redaction.
- **Hardcoded localhost / dev URLs / test keys in prod** — `http://localhost`, test Stripe keys, dev database strings shipped live. *Detect:* localhost/test-key literals in shipped config. *Prevent:* environment config; refuse to start prod with dev values.
- **Missing rate limits on expensive AI/LLM endpoints** — an unauthenticated or unlimited endpoint that calls a paid model (cost-DoS / wallet drain). *Detect:* a model call reachable without auth or a per-user cap. *Prevent:* auth + per-user/cost rate limits + spend caps on any endpoint that calls a paid API.

---

## Standards mapping (for when a user asks "what is this in audit terms")

This catalog is organized by failure mode but cross-cuts the major frameworks: **OWASP Top 10 (2021)**, **OWASP API Security Top 10 (2023)**, **OWASP ASVS**, **CWE**, **NIST SSDF (SP 800-218)**, **MITRE ATT&CK**, **SOC 2 / ISO 27001**, **PCI DSS 4.0**. The Safety Check is a coaching sweep that builds the user's instinct for these, not a substitute for a full audit, external penetration testing, or red-team exercises.

*Adapted from the AI Security Blind Spots: Pen-Test Reference by BuiltByBas LLC. The standard holds: verify before asserting, every finding tested against the real code, never accepted as universally true.*
