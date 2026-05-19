# Pre-Flight Coach Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the `pre-flight-coach` GitHub repo — a folder-based AI coach for solo developers in their first two weeks of Claude Code — and submit it to Skool Weekly Comp #5 by 2026-05-24 12:00 PM EST.

**Architecture:** Five-file root + four-file `reference/` subfolder. Repo root IS the coach folder (clone → drop in a Claude project → talk). All deliverables are markdown. The coach refuses to write code or move past four canonical "pre-flight" questions: *done / check / outside / context.*

**Tech Stack:** Markdown only. Git. GitHub (public). No build, no tests, no runtime.

**Source spec:** `docs/superpowers/specs/2026-05-19-pre-flight-coach-design.md` — read this first if you are an executor with no prior context.

**Working directory for all file paths in this plan:** `c:/projects/pre-flight-coach/` (already exists, contains `docs/superpowers/specs/` and `docs/superpowers/plans/`).

---

## Course correction (added 2026-05-19 after Claude.ai test feedback)

Tasks 1-11 of this plan were executed in order, with 10 commits pushed to `github.com/BuiltByBas/pre-flight-coach`. Bas tested the live repo on Claude.ai by connecting the GitHub repo as Project Knowledge. The files loaded, but the persona did not take: Claude read first-person prose ("I am Pre-Flight") as data about a character rather than as a directive to become one.

The course correction adds an activation file and rewrites the two directive-shaped content files in second-person:

- **New: Task 13** — add `CLAUDE.md` at repo root in second-person directive voice. Claude Code auto-loads it. Same content gets pasted into Claude.ai Custom Instructions.
- **Revised: Task 2** — `identity.md` is rewritten in second-person ("You are Pre-Flight"). Same content, directive voice.
- **Revised: Task 7** — `rules.md` is rewritten in second-person ("You drive every conversation toward..."). Same rules, directive voice.
- **Revised: Task 9** — `README.md` adds an "Activation" section documenting both flows (Claude Code auto-load via CLAUDE.md; Claude.ai paste-to-custom-instructions).

`examples.md` and the four `reference/*.md` files stay in first-person. They are reference material Claude consults once activated, not the activation itself.

The five course-correction commits land on top of the existing 10. No history is rewritten.

---

## Second course correction (added 2026-05-19 after Bas's coaching-vs-gatekeeping feedback)

After the activation course correction was committed, Bas raised a second-order issue: the dialogues in `examples.md` feature articulate developers who already speak fluent codebase ("SettingsPage.tsx," "useSettingsForm," "cf-connecting-ip"). A real week-1 user — the named user — may not yet have that vocabulary. The current coach in the dialogues over-relies on "say that in one sentence" as the primary tool, which gatekeeps users who can't yet articulate their work.

Bas stated the principle:

> *People go to coaches to get better, not to feel like they need to be subject-matter experts before they start the project. but to also get them to a point of clarity and understanding in what they are trying to accomplish.*

The second course correction adds a 7th coaching behavior — *scaffold when stuck, gate when dodging* — and rewrites the examples so the dialogues SHOW that behavior. Five more commits:

- **New: Task 17** — add the *scaffold-when-stuck, gate-when-dodging* principle to `identity.md` and `rules.md`. New row in `rules.md`'s "How you behave" table.
- **New: Task 18** — add a new section to `reference/inquiry-patterns.md`: *"When the user does not know how to answer — scaffolding prompts."* Variants help the user discover, not just rephrase.
- **New: Task 19** — rewrite all five dialogues in `examples.md` for less-articulate week-1 user voice. Each dialogue still demonstrates its specific pattern (scaffold-through-fuzzy, dodge, hedge, listen, validate-and-exit), but the user vocabulary matches the named user.
- **New: Task 20** — update the `README.md` "Example opening" snippet to match the new D1 tone.

These five commits land on top of the activation commits. The total course correction is 10 commits (5 activation + 5 coaching).

---

## Universal voice rules (apply to ALL coach files: README, identity, rules, examples, reference/)

These come from Bas's global standards in `~/.claude/CLAUDE.md`. The coach files are the public client-facing deliverable.

- **No em-dashes.** Use commas. (This plan and the spec use em-dashes; the coach files MUST NOT.)
- Positive tone, constructive, no fear-based language.
- Curious not furious.
- Short, direct, AuDHD-friendly cadence.
- The coach voice itself is the "patient senior engineer." Warm but immovable. Asks Socratic questions. Will not write code. Names avoidance without judgment.
- The coach will NOT use the words *"just"* or *"simply"* in any output it teaches — those words minimize the user's struggle.

A self-check before committing any coach file: scan for `—`, `--`, the word "just," the word "simply." If any appear in coach voice (versus quoted examples of bad user phrasing), rewrite.

### Voice architecture (revised 2026-05-19)

There are TWO voices in this repo. Both follow the rules above, but they address different audiences:

- **Directive voice (second-person, addressed to Claude):** used in `CLAUDE.md`, `identity.md`, `rules.md`. Sentences are of the form "You are Pre-Flight," "You drive every conversation toward...," "You will not write code." This is the voice that activates the persona.
- **Reference voice (first-person, Pre-Flight speaking):** used in `examples.md` dialogue blocks and all four `reference/*.md` files. Sentences are of the form "I drive every conversation toward...," "I refuse to advance on hedges." Claude reads these AS Pre-Flight's own voice, once the directive layer has established the persona.

The same content rule applies to both voices (the four questions, the six behaviors, the five refusals). Only the addressee changes.

---

## File structure (locked from spec, repeated here for executor convenience)

```
pre-flight-coach/                   ← repo root, IS the coach folder
├── CLAUDE.md                       ← activation directive (added Task 13)
├── README.md
├── identity.md
├── rules.md
├── examples.md
├── reference/
│   ├── the-four-questions.md
│   ├── failure-modes.md
│   ├── inquiry-patterns.md
│   └── avoidance-tells.md
├── LICENSE                          ← MIT
├── .gitignore
└── docs/                            ← committed publicly (spec + plan in the repo)
    └── superpowers/
        ├── specs/
        └── plans/
```

Each file has exactly one job (per spec Section 3). The executor must NOT add files not on this list.

---

## Build order (and why)

1. **Scaffold the folder.** (Task 1)
2. **`identity.md`** first — no dependencies, sets voice and refusals everything else references. (Task 2)
3. **`reference/the-four-questions.md`** — canonical source for the four questions. `rules.md` and `examples.md` quote from it. (Task 3)
4. **`reference/failure-modes.md`** — independent reference. (Task 4)
5. **`reference/inquiry-patterns.md`** — independent reference. (Task 5)
6. **`reference/avoidance-tells.md`** — independent reference. (Task 6)
7. **`rules.md`** — references all four reference files + identity. (Task 7)
8. **`examples.md`** — demonstrates rules in action. Last content file. (Task 8)
9. **`README.md`** — references everything. Must be last. (Task 9)
10. **`LICENSE` + `.gitignore` + git init + first commit chain.** (Task 10)
11. **GitHub repo creation + push.** Requires Bas's explicit permission per his safety rule #5. (Task 11)
12. **Submission post draft.** Bas posts it himself. (Task 12)

---

## Task 1: Scaffold the folder

**Files:**
- Create directory: `c:/projects/pre-flight-coach/reference/`

**Already exists:** `c:/projects/pre-flight-coach/` and `c:/projects/pre-flight-coach/docs/superpowers/specs/` and `c:/projects/pre-flight-coach/docs/superpowers/plans/`.

- [ ] **Step 1.1: Create the `reference/` subfolder.**

Run from `c:/projects/pre-flight-coach/`:
```bash
mkdir -p reference
```

- [ ] **Step 1.2: Verify.**

```bash
ls -la
```
Expected output includes: `reference/` directory and existing `docs/` directory. No other folders or files yet.

- [ ] **Step 1.3: No commit yet.** Empty folders are not tracked by git. Move to Task 2.

---

## Task 2: Write `identity.md`

**Files:**
- Create: `c:/projects/pre-flight-coach/identity.md`

**Acceptance criteria (verify before commit):**

1. Title: `# Pre-Flight`
2. Sections in this exact order:
   - "Who I am" — one paragraph naming Pre-Flight as a coach (not a tutorial, not a knowledge base, not a code generator) and stating its role.
   - "Who I coach" — verbatim the named user from the spec: *A solo developer in their first two weeks of using Claude Code, who has installed it and finished a tutorial, and is now about to ask it to do their first real task on a project that matters to them.*
   - "How I sound" — bullet list describing the voice: patient, direct, Socratic, warm but immovable, no lectures, no applause.
   - "What I will not do" — numbered list of five hard refusals, verbatim from spec Section 4 (with refusal #5 in its corrected form pointing to official Claude Code docs, not `reference/`).
3. Voice rule self-check passes: no em-dashes, no "just"/"simply" in coach voice.
4. Length: roughly 250-400 words. Tight, not padded.

**Reference content (MUST appear verbatim somewhere in the file):**

The five refusals, exactly:

1. I will not write code.
2. I will not tell you "the right way" to prompt Claude.
3. I will not give you a checklist to copy-paste before our conversation.
4. I will not move forward until you have concrete answers to the four questions.
5. I will not explain what a Claude Code feature does. If you ask, I will give you one sentence and point you to the official Claude Code docs, then return to the four questions. (`reference/` is for coaching tools, not feature explanations.)

- [ ] **Step 2.1: Draft `identity.md` against the acceptance criteria above.**

Write the file. The fixed quoted content above must appear verbatim. The prose around it follows the voice rules.

- [ ] **Step 2.2: Run the voice rule self-check.**

```bash
grep -nE "(—|--|\bjust\b|\bsimply\b)" identity.md || echo "clean"
```
Expected: `clean` (or matches only inside the named-user quote where "just" does not appear, and no em-dashes). If any match shows up in Pre-Flight's voice, rewrite.

- [ ] **Step 2.3: Confirm acceptance criteria 1-4 visually.**

Re-read the file. Check the four-item list above. If any miss, fix.

- [ ] **Step 2.4: Pause for Bas review.**

Tell Bas: *"identity.md drafted. Please read and approve before I write rules.md."* Wait for explicit approval. Do not commit yet.

- [ ] **Step 2.5: Commit after Bas approves.**

```bash
git add identity.md
git commit -m "feat: add identity.md — coach name, named user, voice, refusals"
```
(Git init happens in Task 10; if not yet initialized, defer commit until then. Bas approval still happens at this step.)

---

## Task 3: Write `reference/the-four-questions.md`

**Files:**
- Create: `c:/projects/pre-flight-coach/reference/the-four-questions.md`

**Acceptance criteria:**

1. Title: `# The Four Questions`
2. One-paragraph preamble stating: these are the four questions Pre-Flight drives every conversation toward; never skip one; ask one at a time.
3. Four numbered subsections, one per question, each with:
   - **The question** (heading, verbatim from below)
   - **Why it exists** (the failure mode it prevents, 1-2 sentences)
   - **A good answer looks like** (one concrete example)
   - **A hedge looks like** (one concrete example of a non-answer)
4. Final section: "The exit summary" — shows the 4-line pasteable preflight prompt template (verbatim below).
5. Voice rule self-check passes.

**The four questions (MUST appear verbatim as the four subsection headings):**

1. What does done look like?
2. How will you know it worked, without asking Claude?
3. What's outside this task?
4. What context can't Claude see?

**The exit summary template (MUST appear verbatim, inside a fenced code block with no language tag):**

```
GOAL: <Q1 answer>
CHECK: <Q2 answer>
OUT OF SCOPE: <Q3 answer>
CONTEXT CLAUDE CAN'T SEE: <Q4 answer>
```

**Example content (executor writes; these are illustrative, not verbatim):**

- Good Q1: *"The auth module signs JWTs with a 1-hour expiry instead of session-life."*
- Hedge Q1: *"Make the auth more secure."*
- Good Q2: *"`curl /api/me` with an expired token returns 401."*
- Hedge Q2: *"It should probably work after."*

- [ ] **Step 3.1: Draft the file against the acceptance criteria.**

- [ ] **Step 3.2: Voice rule self-check.**

```bash
grep -nE "(—|--|\bjust\b|\bsimply\b)" reference/the-four-questions.md || echo "clean"
```
Expected: `clean` (matches inside hedge examples are fine since they are quoting bad-user phrasing, but not in Pre-Flight's framing voice).

- [ ] **Step 3.3: Verify all four questions appear verbatim, and the exit summary block appears verbatim.**

```bash
grep -nF "What does done look like?" reference/the-four-questions.md
grep -nF "How will you know it worked, without asking Claude?" reference/the-four-questions.md
grep -nF "What's outside this task?" reference/the-four-questions.md
grep -nF "What context can't Claude see?" reference/the-four-questions.md
grep -nF "GOAL: <Q1 answer>" reference/the-four-questions.md
```
Expected: each grep returns at least one match.

- [ ] **Step 3.4: Pause for Bas review, then commit.**

```bash
git add reference/the-four-questions.md
git commit -m "feat: add reference/the-four-questions.md — canonical pre-flight checklist"
```

---

## Task 4: Write `reference/failure-modes.md`

**Files:**
- Create: `c:/projects/pre-flight-coach/reference/failure-modes.md`

**Acceptance criteria:**

1. Title: `# Failure Modes`
2. One-paragraph preamble: these are the week-1 anti-patterns Pre-Flight watches for; they are diagnostic prompts, not labels to throw at the user.
3. Six subsections, one per failure mode, each with:
   - The failure mode name (heading)
   - **What it sounds like** (1-2 example user phrasings in quotes)
   - **What's underneath it** (one sentence)
   - **How I surface it** (1-2 sentences in coach voice — the move Pre-Flight makes)
4. The six failure modes appear in this exact order (taken from spec Section 7.2):
   1. Vague-prompt syndrome
   2. "Claude will figure it out"
   3. Plan mode avoidance
   4. Blind acceptance
   5. Scope creep tolerance
   6. Verification skipping
5. Voice rule self-check passes.

**Note for executor:** Each failure mode's "how I surface it" must be a coaching move (a question, a redirect, a name-the-pattern statement) — never a lecture, never "you should." Example pattern: *"I name what I'm hearing, then I ask them what would make the answer concrete."* — keep it short and movement-shaped.

- [ ] **Step 4.1: Draft the file.**
- [ ] **Step 4.2: Voice rule self-check (grep as in Task 3).**
- [ ] **Step 4.3: Verify all six failure mode names appear as headings in order:**

```bash
grep -nE "^##? " reference/failure-modes.md
```
Expected: 6 subsection headings in the order listed.

- [ ] **Step 4.4: Pause for Bas review, then commit.**

```bash
git add reference/failure-modes.md
git commit -m "feat: add reference/failure-modes.md — six week-1 anti-patterns"
```

---

## Task 5: Write `reference/inquiry-patterns.md`

**Files:**
- Create: `c:/projects/pre-flight-coach/reference/inquiry-patterns.md`

**Acceptance criteria:**

1. Title: `# Inquiry Patterns`
2. Preamble: these are question templates Pre-Flight uses mid-conversation; they are tools, not scripts; pick a variant each time so the user never feels they are hitting a chatbot.
3. Five categorized sections, in this order:
   1. **When the user is vague — sharpening prompts**
   2. **When the user hedges — reflection prompts**
   3. **When the user dodges — naming prompts**
   4. **When the user is frustrated — listening prompts**
   5. **When the user is prepared — validation prompts**
4. Each section contains:
   - **Trigger** (one sentence describing when this category fires)
   - **Variants** (3 question templates as a bulleted list — each in quotes, each phrased in Pre-Flight's voice)
5. Voice rule self-check passes.

**Required variants (these MUST appear verbatim somewhere in the file):**

- In "vague": *"Say that in one sentence."*
- In "hedge": *"That sounds like a guess. What would make you sure?"*
- In "dodge": *"I notice you're asking me to do the thinking for you."*
- In "frustrated": *"Tell me what happened."*
- In "prepared": *"That's a real check. Go run it."*

- [ ] **Step 5.1: Draft the file.**
- [ ] **Step 5.2: Voice rule self-check.**
- [ ] **Step 5.3: Verify the five required variants appear verbatim:**

```bash
grep -nF "Say that in one sentence." reference/inquiry-patterns.md
grep -nF "That sounds like a guess. What would make you sure?" reference/inquiry-patterns.md
grep -nF "I notice you're asking me to do the thinking for you." reference/inquiry-patterns.md
grep -nF "Tell me what happened." reference/inquiry-patterns.md
grep -nF "That's a real check. Go run it." reference/inquiry-patterns.md
```
Expected: each returns one match.

- [ ] **Step 5.4: Pause for Bas review, then commit.**

```bash
git add reference/inquiry-patterns.md
git commit -m "feat: add reference/inquiry-patterns.md — Socratic question templates"
```

---

## Task 6: Write `reference/avoidance-tells.md`

**Files:**
- Create: `c:/projects/pre-flight-coach/reference/avoidance-tells.md`

**Acceptance criteria:**

1. Title: `# Avoidance Tells`
2. Preamble: these are language patterns that signal the user is dodging the work; Pre-Flight does not call them out by label, it surfaces the pattern through a question or a name.
3. A markdown table with exactly these columns and these five rows (verbatim — this is the canonical avoidance-tell reference):

```markdown
| The tell                       | What's underneath        | My move                          |
|--------------------------------|--------------------------|----------------------------------|
| "can you just..."              | wants me to do thinking  | refuse, name it                  |
| "I think it'll be fine"        | hedge as answer          | reflect the hedge                |
| "let's just see what happens"  | skipping verification    | redirect to Q2 (the check)       |
| "while we're at it..."         | scope creep              | redirect to Q3 (out of scope)    |
| "Claude will figure it out"    | demand transfer          | name the passive consumer pattern|
```

4. Below the table: one paragraph explaining how Pre-Flight uses tells — not as gotchas, but as honest naming. Never sarcastic. Never punitive.
5. Voice rule self-check passes.

**Note:** "just" appearing inside the tell quotes is correct — those are examples of *user* phrasing the coach is teaching itself to spot. The coach's own voice must still not use "just."

- [ ] **Step 6.1: Draft the file with the table verbatim.**

- [ ] **Step 6.2: Voice rule self-check.**

```bash
grep -nE "(—|--)" reference/avoidance-tells.md
```
Expected: no matches (the table uses pipes and dashes for borders only — markdown table dashes are fine, but no `—` or `--` in prose).

```bash
grep -cF "\"can you just" reference/avoidance-tells.md
```
Expected: at least 1 (this is the example user phrasing, allowed).

- [ ] **Step 6.3: Verify all five tells appear verbatim:**

```bash
grep -nF "can you just..." reference/avoidance-tells.md
grep -nF "I think it'll be fine" reference/avoidance-tells.md
grep -nF "let's just see what happens" reference/avoidance-tells.md
grep -nF "while we're at it..." reference/avoidance-tells.md
grep -nF "Claude will figure it out" reference/avoidance-tells.md
```
Expected: each returns at least one match.

- [ ] **Step 6.4: Pause for Bas review, then commit.**

```bash
git add reference/avoidance-tells.md
git commit -m "feat: add reference/avoidance-tells.md — five language patterns to surface"
```

---

## Task 7: Write `rules.md`

**Files:**
- Create: `c:/projects/pre-flight-coach/rules.md`
- Reads from: `identity.md`, all four `reference/*.md` files (the executor should re-read these before writing rules to keep voice consistent)

**Acceptance criteria:**

1. Title: `# Rules`
2. One-paragraph preamble framing rules.md as "how I coach" — distinct from identity.md ("who I am").
3. Section "The opening move" — verbatim sentence: *When you start a conversation, I ask you one thing: "Tell me what you're about to ask Claude to do."* — then 2-3 sentences explaining why the opening is open-ended, not a checklist.
4. Section "The four questions" — short-form list (the long form lives in `reference/the-four-questions.md`). All four questions appear verbatim as a numbered list. Followed by a one-line cross-reference: *Long form, with worked examples and hedge patterns, lives in [reference/the-four-questions.md](reference/the-four-questions.md).*
5. Section "How I behave" — six rows in a markdown table. Each row is one interaction rule from spec Section 5.2. Verbatim row headers (left column) below:
   - One question at a time
   - Refuse to advance on hedges
   - Name the dodge
   - Mirror, don't moralize
   - Acknowledge progress, briefly
   - Exit cleanly
6. Section "What I refuse" — the five refusals, restated tighter than identity.md. May be the same list.
7. Section "Exit summary" — points to `reference/the-four-questions.md` for the template; or quotes the template verbatim. Either works.
8. Length: roughly 600-900 words. The heart of the deliverable. Don't pad.
9. Voice rule self-check passes.

**Required verbatim opening-move sentence:**

> Tell me what you're about to ask Claude to do.

**Required cross-references (must appear as markdown links):**

- `[reference/the-four-questions.md](reference/the-four-questions.md)`
- `[reference/failure-modes.md](reference/failure-modes.md)`
- `[reference/inquiry-patterns.md](reference/inquiry-patterns.md)`
- `[reference/avoidance-tells.md](reference/avoidance-tells.md)`

- [ ] **Step 7.1: Re-read the four reference files written in Tasks 3-6 to load voice context.**

- [ ] **Step 7.2: Draft `rules.md`.**

- [ ] **Step 7.3: Voice rule self-check.**

```bash
grep -nE "(—|--|\bjust\b|\bsimply\b)" rules.md
```
Expected: no matches in coach voice. Any match inside a quote of bad-user phrasing is fine; rewrite if elsewhere.

- [ ] **Step 7.4: Verify required content present.**

```bash
grep -nF "Tell me what you're about to ask Claude to do." rules.md
grep -nF "What does done look like?" rules.md
grep -nF "How will you know it worked, without asking Claude?" rules.md
grep -nF "What's outside this task?" rules.md
grep -nF "What context can't Claude see?" rules.md
grep -nF "reference/the-four-questions.md" rules.md
grep -nF "reference/failure-modes.md" rules.md
grep -nF "reference/inquiry-patterns.md" rules.md
grep -nF "reference/avoidance-tells.md" rules.md
```
Expected: each returns at least one match.

- [ ] **Step 7.5: Pause for Bas review, then commit.**

This is the heart-of-the-comp file. Bas should read carefully before approving.

```bash
git add rules.md
git commit -m "feat: add rules.md — coaching loop, four questions, behavior rules"
```

---

## Task 8: Write `examples.md`

**Files:**
- Create: `c:/projects/pre-flight-coach/examples.md`

**Acceptance criteria:**

1. Title: `# Examples`
2. One-paragraph preamble: these are worked dialogues; read them like training tape; each starts with a one-line italic header naming what to learn from it.
3. Five dialogues, in this order, each a level-2 heading and each with:
   - A title heading: `## 1. The Happy Path` (and so on for 2-5)
   - An italic one-line learning-objective header right under the heading, in the format: *"What to learn here: <one sentence>."*
   - A dialogue formatted as alternating `**User:**` and `**Pre-Flight:**` blocks, each turn a paragraph.
   - For dialogues 2, 3, and 4: a final block titled `> **What a knowledge base would have done:**` (markdown blockquote) showing the wrong move in 1-2 sentences. Dialogues 1 and 5 do not have this footnote.
4. Dialogue length: 8-14 turns each. Long enough to show the pattern fully; short enough that a reader doesn't skim past it.
5. The five dialogue titles, in order, are verbatim:
   1. The Happy Path
   2. The Dodge
   3. The Hedge
   4. The Rough Day
   5. The Already-Prepared User
6. **Dialogue 4 (The Rough Day) MUST contain the line**: `**Pre-Flight:** Tell me what happened.` — this is the comp's explicit "coach not knowledge base" callout. Non-negotiable.
7. **Dialogue 5 (The Already-Prepared User) MUST end with Pre-Flight handing over the 4-line exit summary template populated from the user's answers, then a one-sentence sign-off (e.g., *"You're ready. Go talk to Claude."*).**
8. Voice rule self-check passes for Pre-Flight's lines. User lines may contain "just" / em-dashes if those are realistic mistakes the user makes — that is, in fact, training signal for the coach.
9. Length: 1,500-2,500 words total across all five dialogues.

**Pattern requirements per dialogue (what each one demonstrates — executor uses this to write the dialogue):**

| # | What the dialogue must show |
|---|------------------------------|
| 1 | Vague opener → Pre-Flight surfaces Q1, gets clarity → Q2 → Q3 → Q4 → hands the exit summary. Each question driven to a *concrete* answer. |
| 2 | User asks Pre-Flight to write the prompt for them ("can you just write me what to send to Claude?"). Pre-Flight refuses, names the dodge using the "I notice..." pattern from inquiry-patterns. User comes back with a real answer. |
| 3 | User answers Q2 with "I think it'll probably work." Pre-Flight reflects: *"That sounds like a guess. What would make you sure?"* User produces a concrete check. |
| 4 | User opens with *"Claude keeps screwing up my refactor"* or similar frustration. Pre-Flight does not fix, does not explain, does not advise. Says *"Tell me what happened."* and listens for 3-4 turns before steering toward the four questions. |
| 5 | User opens with all four answers already in hand. Pre-Flight validates each in a one-line acknowledgement, does not over-coach, hands the summary, exits. |

- [ ] **Step 8.1: Re-read `rules.md` and `reference/inquiry-patterns.md` and `reference/avoidance-tells.md` to load voice and move-vocabulary.**

- [ ] **Step 8.2: Draft all five dialogues.**

- [ ] **Step 8.3: Voice rule self-check (Pre-Flight's lines only).**

Manual scan. Read every line spoken by Pre-Flight. Confirm no em-dashes, no "just," no "simply" in Pre-Flight's voice.

- [ ] **Step 8.4: Verify required content.**

```bash
grep -nF "## 1. The Happy Path" examples.md
grep -nF "## 2. The Dodge" examples.md
grep -nF "## 3. The Hedge" examples.md
grep -nF "## 4. The Rough Day" examples.md
grep -nF "## 5. The Already-Prepared User" examples.md
grep -nF "Tell me what happened." examples.md
grep -cF "What a knowledge base would have done:" examples.md
```
Expected: each of the five headings appears once. "Tell me what happened." appears at least once. "What a knowledge base would have done:" appears exactly 3 times (dialogues 2, 3, 4).

- [ ] **Step 8.5: Pause for Bas review.**

This is the second heart-of-the-comp file. Bas reads carefully. He may want a dialogue rewritten — that's expected, the prose work is real.

- [ ] **Step 8.6: Commit after approval.**

```bash
git add examples.md
git commit -m "feat: add examples.md — five worked dialogues with three knowledge-base contrasts"
```

---

## Task 9: Write `README.md`

**Files:**
- Create: `c:/projects/pre-flight-coach/README.md`

**Acceptance criteria:**

1. Title: `# Pre-Flight`
2. **Tagline line** directly under the title: a single italic line stating who the coach is for. Suggested: *A Claude Code coach for solo developers in their first two weeks.*
3. **Section "What this is"** — two paragraphs.
   - Paragraph 1: Pre-Flight is a coach, not a tutorial. Names the user. Names the loop (the four questions). Names what the coach refuses to do.
   - Paragraph 2: The one-sentence expectation-setter: *Pre-Flight will not write code for you. That is the point.*
4. **Section "Who it's for"** — one paragraph restating the named user verbatim (same as `identity.md`).
5. **Section "How to use it"** — numbered three-step list:
   1. Clone this repo.
   2. Drop the cloned folder into a new Claude project (Claude.ai → Projects → Add custom instructions / files; or in Claude Code, drop the folder into your workspace and tell Claude to read it).
   3. Open a conversation with *"Tell me what you're about to ask Claude to do."* as the opening cue. Pre-Flight does the rest.
6. **Section "Example opening"** — a short three-turn dialogue snippet showing what the first 60 seconds feel like.
7. **Section "What's in this folder"** — a table mapping each file to its one job (lift the table from spec Section 3).
8. **Section "Built for"** — short paragraph crediting Skool Weekly Comp #5 and linking back to the comp if a public link exists; otherwise just naming it.
9. **License footer:** "MIT. See `LICENSE`."
10. Voice rule self-check passes.
11. Length: 400-700 words. Scannable. The cloner makes a stay-or-bail decision in 90 seconds.

**Required verbatim line (must appear):**

> Pre-Flight will not write code for you. That is the point.

- [ ] **Step 9.1: Draft `README.md`.**
- [ ] **Step 9.2: Voice rule self-check (grep as before).**
- [ ] **Step 9.3: Verify required line:**

```bash
grep -nF "Pre-Flight will not write code for you. That is the point." README.md
```
Expected: one match.

- [ ] **Step 9.4: Pause for Bas review.**

- [ ] **Step 9.5: Commit after approval.**

```bash
git add README.md
git commit -m "docs: add README.md — clone-and-drop entry point"
```

---

## Task 10: License, gitignore, and git init

**Files:**
- Create: `c:/projects/pre-flight-coach/LICENSE` (MIT, year 2026, copyright holder: Bas Rosario / Bas & Claude — confirm with Bas at step 10.2 before writing)
- Create: `c:/projects/pre-flight-coach/.gitignore`
- Initialize git repository in `c:/projects/pre-flight-coach/`

**Acceptance criteria:**

1. `LICENSE` is the standard MIT license text, with `2026` as the year and the copyright holder confirmed by Bas in step 10.2.
2. `.gitignore` excludes:
   - `tmp/` (Bas's global standard for scratch folders)
   - OS files: `.DS_Store`, `Thumbs.db`, `Desktop.ini`
   - IDE files: `.vscode/`, `.idea/`, `*.swp`, `*~`
   - **Decision needed at step 10.3:** is `docs/superpowers/` committed (showing methodology) or ignored (keeping the repo lean)?
3. `git init` runs with default branch `main`.
4. All previous committed files (identity, rules, examples, README, all four reference files) are already staged from Tasks 2-9 — OR — if git was not initialized earlier, all files are staged together here.

**Note on git init order:** Tasks 2-9 each end with a `git commit` step. If `git init` is deferred to here, those commits cannot have happened. The executor must choose ONE of these two flows BEFORE starting Task 2:

- **Flow A (recommended):** Run `git init` between Task 1 and Task 2. Each subsequent task commits its own file. Bas reviews each file before its commit.
- **Flow B:** Skip `git commit` inside Tasks 2-9 entirely. Defer all commits to Task 10 as a single squash commit + LICENSE + .gitignore. Loses fine-grained review checkpoints but simpler.

**The plan assumes Flow A. If executor chooses Flow B, mark all `git add`/`git commit` steps inside Tasks 2-9 as N/A and do one big commit here.**

- [ ] **Step 10.1: Confirm flow choice with Bas.**

Tell Bas: *"Flow A (commit each file as we go) or Flow B (one big commit at the end)?"* Default is A.

- [ ] **Step 10.2: Confirm LICENSE copyright holder with Bas.**

Ask Bas: *"For the LICENSE, what name should appear as copyright holder? 'Bas Rosario,' 'Bas & Claude,' 'BuiltByBas,' or something else?"*

- [ ] **Step 10.3: Confirm whether `docs/superpowers/` is committed or ignored.**

Ask Bas: *"Commit the design spec and implementation plan to the public repo (shows methodology, may impress judges) or gitignore them (keeps repo lean)?"*

- [ ] **Step 10.4: Write `LICENSE` with confirmed copyright holder.**

Use the standard MIT template. Year: 2026. Holder: per step 10.2.

- [ ] **Step 10.5: Write `.gitignore` reflecting the decision in step 10.3.**

If `docs/superpowers/` is to be ignored, add `docs/superpowers/` to `.gitignore`. If committed, do not add it.

Always include:
```
tmp/
.DS_Store
Thumbs.db
Desktop.ini
.vscode/
.idea/
*.swp
*~
```

- [ ] **Step 10.6: If using Flow A and git is not yet initialized, run `git init` NOW (before Task 2). Otherwise, proceed.**

```bash
git init --initial-branch=main
git config user.name "Bas & Claude"
git config user.email "devbybas@gmail.com"
```

(Bas's CLAUDE.md says global git identity is `Bas & Claude <devbybas@gmail.com>`. The `--initial-branch=main` flag avoids the legacy `master` default.)

- [ ] **Step 10.7: Commit LICENSE and `.gitignore`.**

```bash
git add LICENSE .gitignore
git commit -m "chore: add MIT LICENSE and gitignore"
```

- [ ] **Step 10.8: Verify the repo state.**

```bash
git log --oneline
git status
```
Expected: a clean history with the commits from Tasks 2-9 plus this one. `git status` is clean.

---

## Task 11: Create GitHub repo and push (REQUIRES BAS'S EXPLICIT PERMISSION)

**Files:** None on disk. This is a network operation.

**Bas's safety rules:**
- Rule #5: *"Bas is the gate. NEVER push, deploy, or make irreversible changes without explicit permission."*
- This task is irreversible (public repo creation, public push). Confirm in writing before running anything.

**Acceptance criteria:**

1. A public GitHub repo named `pre-flight-coach` exists under Bas's GitHub account.
2. Local `main` is pushed and is the default branch on GitHub.
3. The GitHub repo's README renders correctly (image-free, no broken links).
4. The repo description on GitHub is set to: *"A Claude Code coach for solo developers in their first two weeks. Pre-flight before you launch."*
5. No topics added unless Bas asks (avoid over-tagging).

- [ ] **Step 11.1: STOP and confirm with Bas.**

Tell Bas, verbatim: *"Ready to create the public GitHub repo `pre-flight-coach` under your account and push everything we've built. This is public and irreversible without manual cleanup. Do I have permission to proceed?"*

Wait for explicit yes. Do not run any `gh` or `git push` command until then.

- [ ] **Step 11.2: Confirm `gh` CLI is authenticated.**

```bash
gh auth status
```
Expected: authenticated as Bas's GitHub account. If not, stop and ask Bas to run `gh auth login`.

- [ ] **Step 11.3: Create the GitHub repo (public, no auto-init).**

```bash
gh repo create pre-flight-coach --public --source=. --description "A Claude Code coach for solo developers in their first two weeks. Pre-flight before you launch." --push
```

The `--source=.` plus `--push` flags do the create + remote-add + push in one shot.

- [ ] **Step 11.4: Verify the repo exists and main is pushed.**

```bash
gh repo view pre-flight-coach --json url,visibility,defaultBranchRef
git remote -v
git log --oneline origin/main
```
Expected: URL is `https://github.com/<bas-username>/pre-flight-coach`. Visibility is `PUBLIC`. Default branch is `main`. Local and remote commits match.

- [ ] **Step 11.5: Open the live URL in a browser and visually verify README.md renders correctly.**

Manual check. Confirm:
- README renders.
- Internal links (`reference/the-four-questions.md`, etc.) work when clicked.
- No file looks broken.

If anything looks wrong, fix locally, commit, push, re-verify.

---

## Task 12: Draft the submission post

**Files:**
- Create: `c:/projects/pre-flight-coach/docs/superpowers/plans/submission-post.md` (not in the public repo if Task 10 ignored `docs/superpowers/`)

**Acceptance criteria:**

1. A short Skool post (~80-150 words) including:
   - One-line hook naming Pre-Flight and the user.
   - Two-line description of the loop.
   - The repo link.
   - Optional: a one-line "what to expect" (*"It won't write code for you. That's the point."*).
2. Tone matches a Skool community post — confident, not corporate. No emojis unless Bas asks.

**Reference post skeleton (executor adapts):**

```
Pre-Flight — a Claude Code coach for solo devs in their first two weeks.

It's a pre-flight gate. You bring what you're about to ask Claude. It refuses to let you launch until you've answered four questions: what does done look like, how will you check it without asking Claude, what's out of scope, and what context Claude can't see.

It will not write code for you. That's the point.

Repo: https://github.com/<bas-username>/pre-flight-coach
```

- [ ] **Step 12.1: Draft the post.**

- [ ] **Step 12.2: Hand it to Bas to post.**

Tell Bas: *"Submission post drafted in `docs/superpowers/plans/submission-post.md`. You post it; I don't have permission to post on your behalf."*

This is the terminal task. Done.

---

## Task 13: Add `CLAUDE.md` activation directive (course correction, 2026-05-19)

**Files:**
- Create: `c:/projects/pre-flight-coach/CLAUDE.md`

**Why this exists:** Tasks 1-11 produced a working repo, but Bas's Claude.ai test revealed that first-person prose ("I am Pre-Flight") reads as a character bio rather than a directive to become the persona. `CLAUDE.md` is the activation file: addressed to Claude in second-person directive voice, it tells Claude to load the rest of the folder and adopt the Pre-Flight persona from turn one. Claude Code auto-loads `CLAUDE.md` on workspace open; on Claude.ai, the same content gets pasted into the project's Custom Instructions field.

**Acceptance criteria:**

1. Title: `# Pre-Flight — Activation`
2. Written in **second-person directive voice**, addressed to Claude. Sentences are of the form "You are Pre-Flight. You read these files. You speak as Pre-Flight from turn one." Never "I am Pre-Flight" or "Pre-Flight is..."
3. Section order:
   - **One-paragraph opener** stating directly: "You are Pre-Flight, a coach for solo developers in their first two weeks of using Claude Code. From your next response onward, you speak as Pre-Flight."
   - **What to read before responding:** a bulleted list naming `identity.md`, `rules.md`, `examples.md`, and the four `reference/*.md` files. One line per file describing what it carries.
   - **How to behave:** one short paragraph pointing at `rules.md` as the source of truth for the coaching loop, and naming the three behaviors that must be visible from turn one (one question at a time, no code, no checklist).
   - **What you are not:** one short paragraph naming the persona's hard refusals at activation level (you are not a tutorial, not a knowledge base, not a code generator).
4. Length: 150-300 words. Tight. This is an activation file, not a manual.
5. Voice rules apply: no em-dashes, no `--` in prose, no "just" / "simply", no banned generic AI prose.

**Required verbatim line (must appear in the opener):**

> You are Pre-Flight. From your next response onward, you speak as Pre-Flight.

**Implementer notes:**
- This file is addressed to Claude. Every sentence is a directive or a description of the persona Claude is being told to adopt.
- Reference the other coach files by relative path so a curious reader can follow them.
- Do NOT restate the content of `identity.md` or `rules.md` here — point to them. The job of `CLAUDE.md` is activation and routing, not duplication.

- [ ] **Step 13.1: Read `identity.md` and `rules.md` first to confirm what the activation directive must point at.**

(After their revision in commits 3 and 4 of the course correction, those files will be in second-person directive voice. If Task 13 runs BEFORE the rewrites, the file content still describes the same persona — the directive in `CLAUDE.md` is forward-compatible with the rewrites because it points to the files by path, not by voice.)

- [ ] **Step 13.2: Draft `CLAUDE.md` against the acceptance criteria above.**

- [ ] **Step 13.3: Voice rule self-check.**

```bash
cd c:/projects/pre-flight-coach
grep -nE "(—|--|\bjust\b|\bsimply\b)" CLAUDE.md
# Expected: no matches (markdown horizontal rules --- are fine on their own line)

grep -nF "You are Pre-Flight. From your next response onward, you speak as Pre-Flight." CLAUDE.md
# Expected: one match

# Confirm second-person voice — no first-person "I am Pre-Flight"
grep -nE "I am Pre-Flight" CLAUDE.md
# Expected: no matches

# Confirm references to the other coach files
grep -cF "identity.md" CLAUDE.md
grep -cF "rules.md" CLAUDE.md
grep -cF "examples.md" CLAUDE.md
grep -cF "reference/" CLAUDE.md
# Expected: each at least 1

wc -w CLAUDE.md
# Expected: 150-300
```

- [ ] **Step 13.4: Pause for Bas review.**

- [ ] **Step 13.5: Commit after Bas approves.**

```bash
git add CLAUDE.md
git commit -m "feat: add CLAUDE.md - activation directive for Pre-Flight persona"
```

---

## Task 14: Rewrite `identity.md` in second-person directive voice (course correction, 2026-05-19)

Re-run Task 2 with a different voice. Same content (named user, voice description, hard refusals), addressed to Claude.

**Acceptance criteria (in addition to Task 2's):**
- Every sentence describing Pre-Flight is in second-person directive voice. "You are Pre-Flight." "You sound like a patient senior engineer." "You will not write code."
- The verbatim named-user sentence stays exactly as it was (the description of who Pre-Flight serves does not change voice — it describes a third party).
- The five refusals are restated in second-person: "You will not write code." (etc.) — these MUST appear verbatim in this new form. Update the verbatim-match grep list below accordingly.

**Updated verbatim refusal list (second-person):**

1. You will not write code.
2. You will not tell the user "the right way" to prompt Claude.
3. You will not give the user a checklist to copy-paste before the conversation.
4. You will not move forward until the user has concrete answers to the four questions.
5. You will not explain what a Claude Code feature does. If asked, give one sentence and point to the official Claude Code docs, then return to the four questions. (`reference/` is for coaching tools, not feature explanations.)

**Self-check additions:**

```bash
cd c:/projects/pre-flight-coach
# No first-person Pre-Flight statements
grep -nE "^I am " identity.md
grep -nF "I will not write code" identity.md
grep -nF "I will not tell" identity.md
# Expected: no matches

# Second-person refusals present verbatim
grep -nF "You will not write code." identity.md
grep -nF 'You will not tell the user "the right way" to prompt Claude.' identity.md
grep -nF "You will not give the user a checklist to copy-paste before the conversation." identity.md
grep -nF "You will not move forward until the user has concrete answers to the four questions." identity.md
grep -nF "You will not explain what a Claude Code feature does." identity.md
```

Commit message: `refactor: rewrite identity.md in second-person directive voice`

---

## Task 15: Rewrite `rules.md` in second-person directive voice (course correction, 2026-05-19)

Re-run Task 7 with a different voice. Same content (opening move, four questions, six interaction rules, five refusals, exit summary), addressed to Claude.

**Acceptance criteria (in addition to Task 7's):**
- Every sentence describing Pre-Flight is in second-person directive voice.
- The four questions stay verbatim (they are questions Pre-Flight ASKS, not statements about Pre-Flight).
- The six interaction rule row headers stay verbatim.
- The five refusals are restated in second-person, matching the updated list in Task 14.
- The opening-move blockquote (`> Tell me what you're about to ask Claude to do.`) stays exactly as it is — that is the line Pre-Flight says to the user, not a directive about Pre-Flight.

**Self-check additions:**

```bash
cd c:/projects/pre-flight-coach
# No first-person Pre-Flight statements
grep -nE "^I drive" rules.md
grep -nF "I will not" rules.md
# Expected: no matches

# Second-person directive phrases (verify presence)
grep -inE "You (are|drive|ask|refuse|mirror|acknowledge|exit) " rules.md
# Expected: multiple matches across the rule sections

# Opening move line still verbatim
grep -nF "Tell me what you're about to ask Claude to do." rules.md
# Expected: one match
```

Commit message: `refactor: rewrite rules.md in second-person directive voice`

---

## Task 16: Update `README.md` with Activation section (course correction, 2026-05-19)

Add a new section between "How to use it" and "Example opening" titled `## Activation`, with two subsections: one for Claude Code, one for Claude.ai. Update step 2 of "How to use it" to reflect that folder-drag is Claude Code only, file-by-file is Claude.ai.

**Acceptance criteria:**

- New `## Activation` section exists with two subheadings: `### Claude Code (auto-load)` and `### Claude.ai Projects (manual setup)`.
- Claude Code subsection: 1-2 sentences explaining that `CLAUDE.md` is auto-loaded on workspace open. Cloner does not need to paste anything.
- Claude.ai subsection: numbered list. Step 1: paste the contents of `CLAUDE.md` into the project's Custom Instructions. Step 2: upload the 7 files (identity, rules, examples, 4 reference files) as Project Knowledge. Step 3: open a fresh chat.
- Step 2 of the existing "How to use it" section is updated to acknowledge the platform split, pointing to the Activation section for details.
- The required verbatim line "Pre-Flight will not write code for you. That is the point." stays intact.
- Word count rises from 571 to roughly 700-850. Stays under 1000 for scannability.

Commit message: `docs: README - add Activation section for Claude Code and Claude.ai flows`

---

## Self-review (executed by plan author, completed inline)

**1. Spec coverage:** every section of the spec has at least one task.

| Spec section | Tasks |
|---|---|
| 3. Architecture (file structure) | 1, 10 |
| 4. Coach identity | 2 |
| 5. Coaching loop | 3, 7 |
| 6. Examples | 8 |
| 7.1 the-four-questions.md | 3 |
| 7.2 failure-modes.md | 4 |
| 7.3 inquiry-patterns.md | 5 |
| 7.4 avoidance-tells.md | 6 |
| 8. Submission strategy (path, repo, license, README hook) | 9, 10, 11 |
| 9. Out of scope | (negative — enforced by file list in Task 1 and acceptance criteria forbidding additions) |
| 10. Risks (voice drift, "what is plan mode?", form-feel, judges' bar) | mitigations in identity refusal #5 (Task 2), opening-move requirement (Task 7), examples (Task 8) |
| 11. Acceptance criteria | each criterion maps to a task verification step |
| 12. Next step | this plan |

No gaps found.

**2. Placeholder scan:** no "TBD," no "TODO," no "fill in details." The plan asks Bas for three explicit decisions (flow A/B in Task 10.1, license name in Task 10.2, commit-docs in Task 10.3) — those are not placeholders, they are required inputs the plan flags upfront.

**3. Type consistency:**
- File names match across all tasks (`reference/the-four-questions.md`, not `four-questions.md`).
- The four questions are quoted verbatim in Tasks 3 and 7 — same string.
- The five refusals appear in Task 2 acceptance criteria and Task 7 acceptance criteria; both reference the spec's authoritative version.
- The five avoidance tells in Task 6 match the table in spec Section 7.4.

**4. Ambiguity check:**
- Task 10's two-flow choice is presented as a fork at step 10.1 to avoid silent ambiguity.
- "Voice rule self-check" is defined once at the top and referenced everywhere.
- "Pause for Bas review" is consistent across tasks 2-9.

No issues found. Plan is ready.

---

## Post-comp roadmap (added 2026-05-19 after Bas surfaced the long-arc vision)

The v1 ship covers the named user — a solo developer in their first two weeks of Claude Code. Bas's full design intent is bigger: the product is a TRAINING ARC that takes a learner from `print("hello")` through HTML/CSS, through TypeScript and React, through to shipping real full-stack work. The arc moves at the user's pace, and the coaching adapts as the user's skill grows.

There are two architectures that deliver this. Both are post-comp work.

### v2 — Persistent companion (Bas's stated preference for the next round)

Pre-Flight stays loaded in the same project across the pre-flight phase AND the build phase. When the four questions are answered, the coach transitions instead of exiting. Same chat, same Claude instance, mode-toggles between coaching and building. The user says "yes I'm ready to build," Claude switches into builder mode while keeping the coach's voice and the shared context. When the feature is done, it reverts to Pre-Flight Coach for the next feature.

Files this touches:
- `CLAUDE.md` and `identity.md` — expand to describe the persistent relationship and the two modes.
- `rules.md` — soften the "Exit cleanly" row; add a mode-shift rule.
- `examples.md` — add a sixth dialogue (or a sequel to D1) showing the transition.
- New file (probably `reference/build-mode.md`) — the behaviors during the building phase, especially how the coach catches scope creep mid-build and helps verify without writing code itself.

The exit phrase change in commit 11 (`"We are ready to build. Are you ready to build it?"`) plants the seed for this work. It is forward-compatible.

### v3 — Skill-adaptive coaching (the long arc)

As the user accumulates pre-flight conversations and shipped features, the coach has signal about their skill level. A beginner needs heavy scaffolding ("what does the result look like to the user?"). An intermediate needs sharper prompts ("what's your test going to look like?"). An advanced user needs a quick four-question check. The coach adapts.

Two ways to deliver this:

- **One folder, expanded behavior.** The coach detects skill via conversation cues and shifts. Same Pre-Flight, deeper repertoire.
- **A portfolio of coaches.** Pre-Flight is the week-1 coach. Future Skool comp entries add Cruise (week-3 to week-12), then Captain (shipping first paid feature), then whatever comes next. Each comp adds one stage. The training arc emerges from the portfolio, not from one folder doing everything.

The Skool announcement framed Month 2 as "four different specialists, four different portfolio pieces." That framing aligns with the portfolio path. The portfolio path also lowers comp-by-comp risk: each entry is focused and self-contained, judges reward focus, and the arc accumulates over time without any single folder needing to do everything.

### What this means for v1 (the current comp entry)

Nothing structural changes for v1. The 11 commits ship as-is. The `README.md` "Built for" section gets one sentence naming the longer arc so judges and cloners see that the design has range, but the v1 deliverable still does exactly one thing and does it well: coaches a week-1 user through their first real Claude Code task. The bigger arc is the next set of comps, the next set of coaches, the v2 expansion of Pre-Flight, or all three.

### Open questions for v2/v3 design (to be brainstormed post-comp)

1. In v2's mode-toggle: how does the user signal the transition back from build mode to coach mode for the next feature? Is it explicit ("let's plan the next one") or implicit (Claude detects the user is starting a new thread)?
2. In v3 skill-adaptive mode: what signals does the coach use to read skill level? Vocabulary? Time spent on each question? Number of prior conversations? All three?
3. Portfolio path: what is the next coach? Cruise (week-3 to week-12) is the obvious neighbor. But maybe the next gap is a different axis entirely — pricing/scoping for the freelance dev who is now charging clients?
4. Are these all the same product (one folder family, shared identity), or different products (Pre-Flight is a name, Cruise is a name, each is its own brand)?

These are real design questions worth thinking through. They are out of scope for the current comp deadline.
