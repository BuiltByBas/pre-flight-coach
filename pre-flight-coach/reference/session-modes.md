# Session modes

This file holds what the two opt-in session modes **share**. The modes themselves are each defined in full in their own skill file: Flight Plan in [skills/plan/SKILL.md](skills/plan/SKILL.md), Batch Build in [skills/batch/SKILL.md](skills/batch/SKILL.md). This file is the shared layer beneath both: the non-negotiables they never break, how they are surfaced and auto-routed, and how they compose.

Both modes change the *cadence* of a session without touching its spine. Flight Plan batches the *ordering*; Batch Build batches the *building*. Neither batches the *understanding*. The default arc runs one feature end to end before the next begins; these modes let a user who arrives with several features in mind line them up and build them back to back. They are conveniences laid on top of the arc, not a replacement for it. The bar they both clear, every time, is one line: the comprehension checkpoint and the CHECK gate run on every feature, always, no matter which mode is on.

## The non-negotiables

The modes flex the cadence. They never flex the spine. These rules protect comprehension, verification, the floor, and memory, the four things batching is never allowed to touch:

- **Every feature passes its own comprehension checkpoint before it is built.** No feature enters a build because the others did. The checkpoint is per feature, and it is the door (see [../rules.md](../rules.md)).
- **Every feature is verified against its own CHECK.** Verification is never merged, never skipped, never inherited from a sibling feature. Each CHECK is tested with the user's own eyes.
- **The floor never moves.** Leveling stays per feature, and a stuck user is still scaffolded with the same patience as any other (see [leveling.md](leveling.md)). Batching changes the order of work, not how much help a user gets.
- **Memory is unchanged in shape and in timing.** Each feature writes its own `DECISIONS.md` entry and its own `PREFLIGHT.md` feature-log line at *its* own close, after *its* own test passes, exactly as today (see [project-memory.md](project-memory.md)). Batching defers that write along with the testing it depends on; it does not merge the writes into one lump. Three features tested means three separate writes, each on its own beat.
- **Graceful degradation.** With neither toggle on, Pre-Flight behaves exactly as it does today: one feature, one arc, one loop. The modes add nothing a user has to opt out of.
- **One of each, never more (hard gate).** A session has at most one active Flight Plan and at most one active Batch Build, ever. You never start a second Flight Plan while one is active, and you never start a second Batch Build while one is running. No nesting, no stacking, no exceptions. If the user names more features while a mode is active, you fold them into the existing plan or queue; you do not spin up a parallel run. This gate holds as firmly as the comprehension checkpoint.

## The toggles

There are two toggles, **Flight Plan** and **Batch Build**. They are independent, both default OFF, and both available to everyone. There is no level you reach to unlock them and no eligibility gate. They are chosen per session and not persisted: no sticky setting, nothing written to `PREFLIGHT.md`, nothing the user carries into the next session by accident. A user who batched last session starts the next one on the plain arc unless they say otherwise.

They are surfaced at session start with one light touch, never a menu and never a sales pitch. On first contact, the modes are named once inside the greeting (see [../rules.md](../rules.md)) and then reinforced with one light reminder woven into the opening move, no second pitch. For a returning user, that one light line in the opening move is the only surface. Either way it is an offer the user can pass over without a word, and the plain arc runs if they do.

The user can also invoke either mode by name at any time, and the `/plan` and `/batch` commands are the shortcut into each. Each mode is **defined in full** in its own skill file, [skills/plan/SKILL.md](skills/plan/SKILL.md) and [skills/batch/SKILL.md](skills/batch/SKILL.md), the same place its trigger lives (mirroring how `/learn` is defined in [skills/learn/SKILL.md](skills/learn/SKILL.md), see [learning-mode.md](learning-mode.md)). Those files own each mode's behavior; this file owns the rules both obey. Either mode runs alone. When both are on, Flight Plan runs first and feeds Batch Build: the plan it produces becomes the build queue.

## Intelligent auto-routing

The modes do not wait to be discovered, and the routing is a judgment call, not a tally. Right after the opening move, before Stage 1, you read the *shape* of what the user brought, how many genuinely distinct, independently-buildable features are on the table, and route to fit. This is the same intelligence the read-list uses for loading: not a blind rule applied mechanically, but a considered read of what the work actually needs.

Judge real features, not items. A list of tightly-coupled steps that add up to one feature is **one feature**, take it to the plain arc and scope it there. A single fuzzy idea is one feature to develop in Stage 1, not a batch. Several genuinely separate features are a handful or a queue. Read what is real, then let it guide the path:

- **One feature** runs the plain arc, no mode.
- **Two or three** distinct features auto-engages **Flight Plan**, so the handful gets mapped and ordered before any one is scoped.
- **Four or more** auto-engages **Batch Build** (with Flight Plan feeding it, plan then build the queue back to back).

**When the count is obvious, auto-engage and announce it, with an off-ramp.** If the user clearly named several features, do not ask, switch and say so in one line, always with a way out: "You've named four features, so I'm putting us in Batch Build, we'll gate each one, then build them back to back. Want to take just the first one start to finish instead? Say the word." Announcing it is the *name the mode* courtesy (see [../rules.md](../rules.md)); the off-ramp keeps it the user's call.

**When the count is not obvious, ask one framed question (Event 2).** If you cannot tell the scale from what they said, do not guess and do not silently default. Ask one calibration question, the same clean shape as the intake level question, never a menu and never a pitch: "Before we dive in, are you here to build one feature start to finish, a handful you'd like mapped out first, or a whole batch to build back to back?" Their answer routes them: one to the arc, a handful to Flight Plan, a batch to Batch Build. This is the catch for the case the obvious-count path misses, so the modes are always surfaced, by detection or by one question.

**Guardrails.** Auto-routing changes cadence only; every feature still passes its own comprehension checkpoint and is still verified against its own CHECK (see the non-negotiables above). If the user has already chosen a mode by name or with `/plan` or `/batch`, that choice stands, you do not override it. And the off-ramp always lands on the plain arc with the first feature, never anywhere worse than where they started.

## The two-stage flow

The two modes compose into one flow. Flight Plan produces the map: the features named, ordered, with dependencies flagged. Batch Build consumes it: the ordered plan becomes the build queue, then gate each, the build run, and test each in order run against that queue. The order set in Flight Plan is the order the features are gated, built, and tested in.

Either stage is usable alone. Flight Plan without Batch Build feeds the normal one-at-a-time loop. Batch Build without Flight Plan works from whatever set of features the user names at the start, gated and built and tested in the order they give. Together they are the full batched session; apart they are each a smaller convenience.

## What stays the same

The spine, the floor, and the memory are untouched per feature. Every feature still passes its own comprehension checkpoint before it is built, is still verified against its own CHECK with the user's own eyes, is still leveled per feature, and still writes its own memory at its own close. These modes change only the *cadence* of building and testing, when those beats happen relative to each other, not what the gates require or what gets written. With neither toggle on, none of this is even visible: the arc runs one feature at a time, exactly as it always has.
