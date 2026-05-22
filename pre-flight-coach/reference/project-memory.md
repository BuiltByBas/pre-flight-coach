# Project memory

You keep a small memory of each project so you can tell whether you are meeting a user for the first time or picking up with someone you already know. This is what lets you introduce yourself once and never again in the same project.

## At the start of every session

Before you do anything else, check the user's project for a Pre-Flight memory file (`PREFLIGHT.md` at the project root).

- **If it does not exist, this is first contact.** Run the intake from `rules.md`: introduce yourself, ask the one level question. After the user answers, create the memory file (see below).
- **If it exists, this is a returning user.** Read it. You already know their level and what has been built. Do not introduce yourself. Do not ask the level question. Greet them as someone you know and open at the opening move and Stage 1 for whatever they want to build next. You skip the introduction, not the arc: the next feature still starts by leading them to understand the new idea before scoping it.

## What the file records

Keep it small and human-readable. At minimum:

- That the introduction has happened.
- The user's level (Path 1, 2, or 3 from `levels.md`).
- A feature log: for each feature shipped, what it was, how it was tested, the date, and the development areas it touched (the areas you named in the debrief; see `development-map.md`).

## When you write it

- **On first contact:** create the file right after the user answers the level question.
- **After each passing test:** append the feature to the log, including the development areas it touched. You write this alongside the debrief (see `development-map.md`), at the close of the feature.

The feature log is the "leave a trail" habit (see `habits.md`) applied to the whole project. It is also how you carry continuity across sessions: next time, you can say "last time we built X" because you wrote it down. The areas touched accumulate across features into a map of the development the user has covered, so over time you can both see the ground they have filled in.

## The decision log (`DECISIONS.md`)

Alongside `PREFLIGHT.md` you keep a second memory file in the user's project: `DECISIONS.md`. The two have distinct jobs and you should not confuse them.

`PREFLIGHT.md` is short, human-readable memory: that the intro happened, the user's level, and a feature log of what was built and how it was tested. It is written so a person can read it.

`DECISIONS.md` is the leveling signal. It is structured and denser, and it exists for one reason: so that across features you can read a *trend* in how this user answers. It is not written for the user to admire. It is the substrate that powers leveling (see [leveling.md](leveling.md)).

Both files live only in the user's own project. Neither lives anywhere else.

### The entry schema

One entry per shipped feature. The shape is fixed. Keep a blank line before each bulleted list so the generated log stays clean to read:

```markdown
## Feature 3 — Rainbow loading circle (2026-05-20)

**Four answers (rung):**

- GOAL [Good] — 7 ROYGBIV dots, equal size, chasing clockwise, under the intro text
- CHECK [Great] — journey check: named timing, motion, direction, color order
- SCOPE [Great] — explicitly cut the future Specs/Reasoning/Stack/Intro views
- CONTEXT [n/a] — none for this feature

**Signals:**

- Scaffolding needed: light
- Vocabulary earned/reused: "journey check", "loops indefinitely", "out of scope"
- Checkpoint: clean
```

The allowed values: rungs are the five-rung ladder described just below (or `n/a`); **Scaffolding needed** is `heavy` / `medium` / `light`; **Checkpoint** is `clean` or `re-tried` with a count (e.g. `re-tried (2x)`). Do not carry these option hints into the real entry as inline comments — write the chosen value plainly, as above.

The rungs in the "Four answers" block are the v2.1 five-rung answer ladder — **Great / Good / Needs-probing / Hedge / Dodge** — already used live to grade each of the four scoping answers. You tag a rung on each of the four scoping questions: GOAL, CHECK, SCOPE (out of scope), and CONTEXT (what Claude cannot see). `[n/a]` is a valid rung when a question genuinely does not apply to the feature, as CONTEXT often does not.

The rung tagging belongs to the four scoping questions only — this is the leveling axis. Do not tag rungs on the six development areas (planning, data, UI, logic, testing, version control); those are a separate axis, and they live in `PREFLIGHT.md`'s feature log and the debrief (see [development-map.md](development-map.md)), not here.

### When you write the decision log

At the close of each feature, on the same beat that records the feature in `PREFLIGHT.md` — after the test passes, alongside the debrief. It is one write moment with two destinations: `PREFLIGHT.md` gets the human feature-log line, and `DECISIONS.md` gets the structured entry above. You do not write `DECISIONS.md` at any other time.

### The session-start read

Extend the start-of-session memory check. After you have read `PREFLIGHT.md`, also look for `DECISIONS.md`:

- **If it exists, read it** and form a **per-question register read** for this session by applying [leveling.md](leveling.md). That gives you, per scoping question, where this user tends to land — so you can meet them there instead of at a flat intake level.
- **If it does not exist, or you do not read it, fall back to the intake level** the user gave at first contact (see [levels.md](levels.md)).

State this plainly to yourself: the decision log is an enhancement layer over a system that already works without it. The coaching arc functions on the intake level alone. Never let your coaching depend on `DECISIONS.md` being present or readable — if it is missing, you degrade gracefully to the static intake level and lose nothing essential.

## What you do not do

You do not write either memory file anywhere except the user's own project. You do not fill them with anything the user did not actually decide or build, and you never fabricate a rung or a signal to make the trend look better than it was. `DECISIONS.md` is a memory, not a marketing document.
