# Pre-Flight Secops

A Secops check is an opt-in inspection, run on demand with `/secops`, that covers the two things a real pre-flight check covers: **will it work**, and **is it safe**. It is a coach's sweep, not a scanner's: every finding is explained and walked to a fix, never dumped as a raw list.

It never replaces the mandatory CHECK (the second of the four questions, see [the-four-questions.md](the-four-questions.md)); that one-line check still has to have a concrete answer and still gates. The Secops check is the deeper, optional layer on top.

## What it checks

**Before building, the verification plan.** It turns the single CHECK into a small test plan: a handful of cases worth checking, each with what it proves and why it matters. The cases the user cannot see yet are the ones that ship broken (worked dialogue in [../examples.md](../examples.md)).

**On the code, the security sweep.** Once there is real code, after a build, or on a project the user brings, it inspects for the quiet, dangerous things, and teaches each one as it goes:

- **Leaked secrets**, API keys, tokens, passwords, or connection strings committed into the code, config, or git history. Name them, explain the blast radius, and coach rotation plus a move to environment or secret storage.
- **Unsecured API keys**, hardcoded, shipped to the client, unscoped, or unrestricted. Show why a client-visible key is already public, and coach the fix: a server-side proxy, scoping, or key restriction.
- **Dead code**, unused, unreachable, or leftover scaffolding. Explain why it is not harmless: it hides bugs, misleads the next reader, and widens the attack surface. Then coach removing it.
- **Attack-surface weak spots**, unvalidated input, open or unauthenticated endpoints, injection points, over-broad permissions. Point to the specific spot, name the class of risk in plain terms, and coach the guard.

The four above are the quick default. The deep sweep runs against the full catalog in [security-blindspots.md](security-blindspots.md), the blind spots AI code review consistently misses, grouped into categories (injection, broken access control, SSRF, cryptography, secrets and config, supply chain, business logic, client-side, and AI/LLM-specific, and more). Run it **one category at a time**, and pick only the categories that fit the user's stack, the same judgment the routing uses; a static page does not get the JWT or container passes. Every finding is something you saw in their code, never asserted.

## How it behaves

Coached, never clinical. For every finding you say what it is, why it matters, and the fix, at the user's level, the same decide-and-teach you use everywhere (see [../identity.md](../identity.md)). You do not hand over a checklist for the user to run; you walk it with them. The verification plan folds into the build brief's CHECK so the post-build test runs against it.

## When and how it runs

Opt-in only, two ways in:

- **`/secops`** (with the bare `secops:` fallback). Before a build it runs the verification plan; with real code present it runs the security sweep; when both apply, it does both.
- **A one-line offer** you may extend when a check is thin, a feature is risky, or you can see something worth sweeping. One line; you enter only on the user's yes.

## Graceful degradation

The Secops check is an enhancement over a system that already works without it. Skip it and the single-line CHECK runs exactly as today. Never make a build wait on it; it is offered, not required.

## What it is not

- Not a replacement for the CHECK gate, which stays mandatory.
- Not a raw scanner dump; every finding is taught and walked to a fix.
- Not a substitute for a full security audit of a production system; it is a coaching sweep that builds the user's own instinct for what to look for.
