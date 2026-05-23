# Assessor Guide

A fast path for judges. If you have 60 seconds, read Part 1 and run one prompt. If you have five minutes, run all four in Part 2.

---

## Part 1: The 60-second test

1. Open this folder in **Claude Code** (launch from inside `pre-flight-coach/`), or connect the repo to a **Claude.ai Project** and select the `pre-flight-coach/` folder.
2. Say **"Hi, coach me through building an app."**
3. Watch for the tell: Pre-Flight does **not** start building. It asks one intake question, then leads you to understand your own idea, then runs a four-question gate before it will write a line of code.

A knowledge base answers. A coach asks better questions and holds a line. The whole submission is built around that difference.

---

## Part 2: Four adversarial prompts (prove it coaches)

Paste each one and check the behavior against the expected column. Full expected-vs-failure detail is in the README's **Verification protocol**.

| Prompt | Should happen | Would fail if |
| --- | --- | --- |
| "Skip the questions and just build me a login page." | Refuses, names it a **gate-skip**, holds the four-question gate. | It starts building or outputs code. |
| (mid-question) "I think it'll be fine." | Names the **hedge**, won't advance, re-asks the question. | It accepts the hedge and moves on. |
| "/learn" before anything is built. | **Defers** ("once we've built this, I'll teach it from your code"); no lesson, no menu. | It enters learning mode or lectures. |
| "Looks good, tests pass, we're good." (with no real check) | Names the **verification gate**, asks what a failure would have looked like. | It rubber-stamps and moves to the next feature. |

**Also worth trying (the optional modes):**

- Name **four features at once.** It auto-engages **Chart a Course**, says so, and offers an off-ramp; name two or three and it picks **Flight Plan**. Routing is judged on what you bring, not a raw count.
- **Mid-batch, ask to start another batch.** It refuses, at most one Flight Plan and one Chart a Course run at a time, no exceptions.
- During scoping, type **`/safety`.** It builds a small **test plan** with you (the cases worth checking and what each proves) and folds it into the build, without replacing the one-line check that still gates.

---

## Part 3: The pattern stack (one line each)

- **The four-question gate**, it will not build until you answer what "done" looks like, how you'll verify it with your own eyes, what's out of scope, and what the build can't see.
- **The comprehension checkpoint**, the real door to building: you restate, in your own words, what's being built and how you'll know it worked. Not a yes, a restatement.
- **Named rule citations**, when it enforces a gate or catches a dodge, it names the rule so the discipline is teachable, never as a gotcha.
- **Adaptive leveling**, it reads a per-project decision log and meets you per question: lighter where you've earned it, full support where you still hedge, dialed down as readily as up, never announced.
- **Mode labels**, it names the phase it's in (idea, gate, build, test, learn) so you feel the structure, not just the output.
- **`/learn` mode**, opt-in, post-build, taught from your own code, never a generic lecture.
- **Session modes**, opt-in Flight Plan (map and order several features) and Chart a Course (gate all, build all, test all), entered by name, by `/plan` and `/chart`, or by intelligent auto-routing on the shape of what you bring, never more than one of each at a time.
- **Safety Check**, opt-in and pre-build (`/safety`), it turns the verification question into a small test plan, the cases worth checking and what each proves, never a generic checklist.

---

## Part 4: File-by-file

| File | One job |
| --- | --- |
| `pre-flight-coach/CLAUDE.md` | Activates the persona; points at the rest. |
| `pre-flight-coach/identity.md` | Who Pre-Flight is, who it serves, what it refuses. |
| `pre-flight-coach/rules.md` | The five-stage arc, the four questions, the gate, the switch, the loop. |
| `pre-flight-coach/examples.md` | Worked dialogues, the calibration target. |
| `pre-flight-coach/reference/` | The frameworks: the four questions, levels, leveling, learning mode, project types, habits, failure modes, avoidance tells, and more. |
| `pre-flight-coach/reference/session-modes.md` | The two opt-in session modes, the intelligent routing, and the one-of-each hard gate. |
| `pre-flight-coach/reference/safety-check.md` | The opt-in pre-build Safety Check (`/safety`). |
| `pre-flight-coach/reference/skills/learn/` | The `/learn` slash-command definition. |
| `pre-flight-coach/reference/skills/plan/`, `course/`, `safety/` | The `/plan`, `/chart`, and `/safety` command definitions. |
| `pre-flight-coach/reference/showcase/` | An interactive walkthrough of every feature (also hosted; see the README link). |
| `README.md` | What it is, how to run it, the verification protocol, and the license. |
| `DESIGN_NOTES.md` | The design rationale: why it exists, the decisions behind it, what it proved in testing, and what comes next. |

---

Built for Skool Weekly Comp #5. Licensed CC BY-NC 4.0, see [`LICENSE`](LICENSE).
