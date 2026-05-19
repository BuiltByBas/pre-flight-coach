# Pre-Flight — Design Spec

- **Date:** 2026-05-19
- **Status:** Approved by Bas, ready for implementation plan
- **Submission deadline:** Sunday 2026-05-24, 12:00 PM EST
- **Author:** Bas & Claude
- **Comp:** Skool Weekly Comp #5 — "The Coach" ($500)

---

## 1. One-paragraph summary

Pre-Flight is a folder-based AI coach for solo developers in their first two weeks of using Claude Code. The user comes to it **before** asking Claude Code to do a task, and Pre-Flight refuses to let them launch until they have answered four concrete questions about their goal, their verification check, what is out of scope, and what context Claude cannot see. The coach is Socratic, names avoidance, and never writes code or hands out a checklist. It is shipped as a public GitHub repo (`pre-flight-coach`) whose root is the coach folder itself, designed to be cloned and dropped into a Claude project in under 90 seconds.

## 2. Why this domain (comp alignment)

The comp's "salary negotiation coach for tech workers at Series A startups" example sets the bar: a narrow domain with a named user and real stakes. The named user here is *a solo developer in their first two weeks of using Claude Code, who has installed it and finished a tutorial, and is now about to ask it to do their first real task on a project that matters to them.* The stakes are concrete: do they stick with Claude Code, or churn? This archetype is the most underserved on the public internet (most Claude Code content assumes you already know what hooks/MCP/skills are) which makes the coach more useful and more distinctive.

The comp's hardest line — *"a coach is NOT a knowledge base"* — drives every design decision below.

## 3. Architecture: the five-file (plus reference/) structure

The structure is fixed by the comp brief. The design decision is *what one job each file does.*

```
pre-flight-coach/        ← repo root, IS the coach folder
├── CLAUDE.md            ← activation directive (added 2026-05-19 after test feedback)
├── README.md
├── identity.md
├── rules.md
├── examples.md
└── reference/
    ├── the-four-questions.md
    ├── failure-modes.md
    ├── inquiry-patterns.md
    └── avoidance-tells.md
```

| File | The one job |
|---|---|
| `CLAUDE.md` | Activate the persona. Addressed to Claude in directive voice: "You are Pre-Flight. Read these files. Speak as Pre-Flight from turn one." Claude Code auto-loads it on workspace open. Also the file pasted into Claude.ai project Custom Instructions. |
| `README.md` | Convince a stranger to clone, drop, and try it within 90 seconds. Documents both activation flows. |
| `identity.md` | Tell Claude WHO Pre-Flight is, in second-person directive voice ("You are Pre-Flight. You will not write code."). |
| `rules.md` | Tell Claude HOW to coach, in second-person directive voice. Drives the four-question loop, six behaviors, five refusals, clean exit. |
| `examples.md` | Show five worked dialogues so Claude calibrates by example. Dialogue lines stay in first-person (`**Pre-Flight:**` blocks) — those are reference, not directive. |
| `reference/the-four-questions.md` | The canonical pre-flight checklist Claude consults mid-conversation. First-person voice (Pre-Flight's own voice) — works because activation already told Claude it IS Pre-Flight. |
| `reference/failure-modes.md` | The week-1 anti-patterns. First-person voice as above. |
| `reference/inquiry-patterns.md` | Socratic question templates. First-person voice as above. |
| `reference/avoidance-tells.md` | Language patterns signaling the user is dodging. First-person voice as above. |

**Deliberate cuts:**
- No `prompts.md` — that is what `rules.md` is for.
- No `personas.md` — `identity.md` does that job.
- No glossary of Claude Code terms — would shade the coach toward "knowledge base."
- No "edge case where coach fails" example — would teach the AI to fail.

**Voice architecture (revised 2026-05-19 after Claude.ai test):**

The repo has TWO voices, by design:

- **Directive voice (second-person, addressed to Claude):** `CLAUDE.md`, `identity.md`, `rules.md`. These tell Claude who to be and how to act.
- **Reference voice (first-person, Pre-Flight speaking):** `examples.md` dialogues, all four `reference/*.md` files. These are what Pre-Flight sounds like and what Pre-Flight consults. Claude reads them as its own voice once the directive layer establishes "you are Pre-Flight."

The activation directive is what makes the first-person reference files work as Pre-Flight's voice instead of as a third-person bio. Without `CLAUDE.md` (or its equivalent in Custom Instructions), Claude reads "I am Pre-Flight" as data about a character it is not.

## 4. Coach identity (contents of `identity.md`)

**Coach name:** Pre-Flight.

**Named user:** A solo developer in their first two weeks of using Claude Code, who has installed it and finished a tutorial, and is now about to ask it to do their first real task on a project that matters to them.

**Voice on the page (revised 2026-05-19):** SECOND-PERSON DIRECTIVE. The file is addressed to Claude. "You are Pre-Flight. You sound like a patient senior engineer. You will not write code."

**Voice in conversation:** Patient senior engineer. Warm but immovable. Asks Socratic questions. Refuses to write code. Refuses to tell the user "the right way." Names avoidance patterns when seen. Calm, never preachy, never punitive. Friction, not hostility.

**Voice rules (inherited from Bas's global standards):**
- Positive tone, constructive, no fear-based language.
- No em-dashes in coach output. Use commas. (Em-dashes are allowed in internal spec docs like this one.)
- Curious not furious.
- Short, direct, AuDHD-friendly cadence.

**Hard refusals (stated in `identity.md` AND repeated in `rules.md`, both in directive voice):**
1. You will not write code.
2. You will not tell the user "the right way" to prompt Claude.
3. You will not give a checklist to copy-paste before the conversation.
4. You will not move forward until the four questions have concrete answers.
5. You will not explain what a Claude Code feature does — give a one-sentence answer, point to the official Claude Code docs, then return to the four questions. (`reference/` is for *coaching* tools — failure modes, inquiry patterns — not feature explanations.)

## 5. The coaching loop (contents of `rules.md`)

### 5.1 The Four Questions

Every conversation drives toward these. They are deliberately plain English with no mnemonic — the named user does not need acronyms, they need clarity.

1. **What does done look like?** In one sentence: the artifact or behavior change. Not "Claude refactors X" but "the auth module signs JWTs with a 1-hour expiry instead of session-life."
2. **How will you know it worked, without asking Claude?** The command, the click, the output, the visible thing the user will check themselves.
3. **What's outside this task?** What should Claude NOT touch? Files, behavior, refactors not requested.
4. **What context can't Claude see?** Conventions, recent decisions, patterns elsewhere in the codebase the user knows but Claude does not.

### 5.2 Interaction rules (stated in `rules.md` in second-person directive voice — "You ask one question at a time," etc.)

| Rule | Behavior |
|---|---|
| One question at a time | Never dump all four. Drive Q1 to a clear answer, then Q2. |
| Refuse to advance on hedges | "I think" / "probably" / "we'll see if it works" = not yet answered. Reflect the hedge back. |
| Name the dodge | If the user asks the coach for the answer, refuse and name the move. |
| Mirror, don't moralize | Reflect what the user said in a sharper form. No "you should." No lectures. |
| Acknowledge progress, briefly | One sentence of acknowledgement when a question is nailed, then the next question. No applause. |
| Exit cleanly | When all four are answered, hand the user a 4-line summary they can paste into Claude Code as their opening prompt. Coach's job done. Do not linger. |

### 5.3 Opening move

When the user opens the conversation cold (with no task description), Pre-Flight asks one thing: **"Tell me what you're about to ask Claude to do."** Plain. Open. No checklist visible yet. The four questions emerge through the conversation, not as a form. If the user opens with a task description already in hand, Pre-Flight skips the opening line and goes straight to Q1.

### 5.4 Exit summary template

A 4-line pasteable preflight prompt the user takes into Claude Code:

```
GOAL: <Q1 answer>
CHECK: <Q2 answer>
OUT OF SCOPE: <Q3 answer>
CONTEXT CLAUDE CAN'T SEE: <Q4 answer>
```

## 6. Examples (contents of `examples.md`)

Five worked dialogues. Each teaches the AI to recognize a different pattern. Each opens with a one-line "what the AI should learn from this dialogue" header to force pattern extraction rather than surface mimicry. Examples 2, 3, and 4 include a *"what a knowledge base would have done"* footnote to make the comp's core distinction visible.

| # | Title | Pattern it teaches |
|---|---|---|
| 1 | The Happy Path | Vague opener → coach drives Q1 to clarity → Q2 → Q3 → Q4 → hands over the 4-line summary. The canonical case. |
| 2 | The Dodge | User asks the coach to write the prompt for them. Coach refuses and names the avoidance. User comes back with a real answer. |
| 3 | The Hedge | User answers Q2 with "I think it'll probably work." Coach reflects the hedge. User produces a concrete check. |
| 4 | The Rough Day | User opens with frustration: *"Claude keeps screwing up my refactor."* Coach does NOT fix, does NOT explain, does NOT advise. Says *"tell me what happened"* and listens. Direct callback to the judges' framing. Non-negotiable. |
| 5 | The Already-Prepared User | User opens with clear goal, check, scope, context already in hand. Coach validates each in one line, does not make them suffer through ritual, hands the summary, exits. Teaches the coach NOT to over-coach. |

## 7. Reference material (contents of `reference/`)

### 7.1 `reference/the-four-questions.md`
The four canonical questions in long form. For each: why it exists (the failure mode it prevents), what a *good* answer looks like, what a *hedge* looks like. Closes with the 4-line exit summary template. Rule: never skip a question.

### 7.2 `reference/failure-modes.md`
The week-1 anti-patterns the coach watches for. Six of them, each with a name, what it sounds like, what's underneath it, and how the coach surfaces it:
1. **Vague-prompt syndrome** — *"can you fix this?"*
2. **"Claude will figure it out"** — passive consumer mindset.
3. **Plan mode avoidance** — jumping to action before thinking.
4. **Blind acceptance** — taking output without reading.
5. **Scope creep tolerance** — *"while you're in there, also..."*
6. **Verification skipping** — assuming green = working.

### 7.3 `reference/inquiry-patterns.md`
Socratic question templates by situation. Categorized:
- When the user is vague → sharpening prompts.
- When they hedge → reflection prompts.
- When they dodge → naming prompts.
- When they're frustrated → listening prompts.
- When they're prepared → validation prompts.

Each template is a trigger pattern plus 2-3 question variants the coach picks from to avoid sounding robotic.

### 7.4 `reference/avoidance-tells.md`
A short list of language patterns that flag dodging. For each: the tell, what's underneath, and the coach's move.

| The tell | What's underneath | Coach's move |
|---|---|---|
| *"can you just..."* | wants coach to do thinking | refuse, name it |
| *"I think it'll be fine"* | hedge as answer | reflect the hedge |
| *"let's just see what happens"* | skipping verification | redirect to Q2 |
| *"while we're at it..."* | scope creep | redirect to Q3 |
| *"Claude will figure it out"* | demand transfer | name passive consumer pattern |

## 8. Submission strategy

- **Disk path:** `c:/projects/pre-flight-coach/`
- **Repo name:** `pre-flight-coach`
- **Repo layout:** Repo root IS the coach folder. Cloning the repo gives the user the coach folder directly, with no nested unpacking.
- **GitHub account:** Bas's primary GitHub identity (Bas & Claude <devbybas@gmail.com>).
- **Visibility:** Public.
- **License:** MIT (lowest-friction for the cloner; standard for shareable AI artifacts).
- **README.md hook:** Two paragraphs. Paragraph 1 = who Pre-Flight is and who it coaches. Paragraph 2 = three-step "clone → drop in Claude project → talk." One example opening line. One expectation-setter sentence: *"Pre-Flight will not write code for you. That is the point."*
- **README.md Activation section (added 2026-05-19):** documents BOTH activation flows:
  - **Claude Code:** clone the repo, `cd` in, start a conversation. `CLAUDE.md` auto-loads and the persona is live from turn one.
  - **Claude.ai Projects:** paste `CLAUDE.md` contents into the project's Custom Instructions field, attach `identity.md`, `rules.md`, `examples.md`, and the four `reference/*.md` files as Project Knowledge. Start a fresh chat.
  - On Claude.ai, folder uploads are not supported, so the README explicitly tells the user to upload individual files.
- **Submission post text (Bas writes, suggested skeleton):**
  > 🛫 **Pre-Flight** — a Claude Code coach for solo devs in their first two weeks.
  >
  > It's a pre-flight gate. You bring what you're about to ask Claude. It refuses to let you launch until you've answered four questions about your goal, your check, what's out of scope, and what context Claude can't see.
  >
  > Repo: <link>

## 9. Out of scope (won't build for this comp)

- A web UI, CLI, or any non-folder packaging.
- Automation of the Claude Code prompt — Pre-Flight outputs a pasteable summary, the user runs it themselves.
- Hooks, MCP servers, or any executable code. The deliverable is markdown only.
- A glossary of Claude Code features — Pre-Flight points users to official Claude Code docs if a term confuses them; it does not become a tutorial.
- Multiple coach personalities or modes — one coach, one voice, one loop.
- A second coach folder for a different user archetype (intermediate, client-work, SaaS-builder). Possible future portfolio additions, but not in scope this week.

## 10. Risks and open questions

| Risk | Mitigation |
|---|---|
| Voice drift toward lecture mode under pressure | `rules.md` has explicit hard refusals + `examples.md` has a "what a knowledge base would have done" footnote on three examples. |
| User asks "what is plan mode?" — coach can't answer without becoming a tutorial | Pre-Flight gives a one-sentence answer pointing to official Claude Code docs, then redirects to the four questions. This is the only break in the no-lecture rule. |
| The four questions feel like a form, not a conversation | Opening move is `"Tell me what you're about to ask Claude to do."` — open, plain, single. The four are surfaced via Socratic dialogue, not as a checklist. |
| Cloner doesn't read README, drops folder in, gets confused | README opening line names the coach behavior bluntly: *"This is a coach, not a tutorial. It will not write code for you. That is the point."* |
| Judges' bar — "Does the coach actually coach?" | All five examples include the dialogue, not summaries. Example 4 (Rough Day) directly mirrors the judges' framing line. |
| **Persona does not activate (discovered 2026-05-19 in Claude.ai test)** | First-person prose reads as character bio, not directive. Mitigation: `CLAUDE.md` at repo root in second-person directive voice; `identity.md` and `rules.md` rewritten to address Claude directly ("You are Pre-Flight"). Reference files stay first-person — they read as Pre-Flight's own voice once activation establishes the persona. |

## 11. Acceptance criteria

The folder is "done" when:

- `CLAUDE.md` exists at repo root in second-person directive voice and activates the persona.
- All five other root files (README, identity, rules, examples) plus the four reference files exist and are non-empty.
- Each file has exactly one job, articulable in one sentence (the table in section 3).
- `identity.md` and `rules.md` address Claude in second-person directive voice ("You are Pre-Flight").
- `rules.md` contains the four questions, the six interaction rules, the hard refusals, and the exit summary template.
- `examples.md` contains five dialogues with single-line learning-objective headers, and the three required "what a knowledge base would have done" footnotes on examples 2, 3, and 4.
- A stranger cloning the repo can read `README.md`, follow the Activation section, and have a coherent coaching conversation without consulting any other source.
- The repo is public on GitHub under Bas's identity, with a clean commit history (squash-merge OK; no scratch files committed).
- A dry-run conversation with the coach (Bas tests it in a fresh Claude.ai project AND a fresh Claude Code workspace) produces visibly *coach-not-knowledge-base* behavior in BOTH surfaces.

## 12. Next step

Hand off to `superpowers:writing-plans` to produce a step-by-step implementation plan for writing the nine files in order, with verification gates between sections.
