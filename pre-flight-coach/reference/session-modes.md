# Session modes

This file defines two opt-in session modes that change the cadence of a session without touching its spine. Both batch the *building*, never the *understanding*. The default arc runs one feature end to end before the next begins; these modes let a user who arrives with several features in mind line them up and build them back to back. They are conveniences laid on top of the arc, not a replacement for it. The bar they both clear, every time, is one line: the comprehension checkpoint and the CHECK gate run on every feature, always, no matter which mode is on.

## The non-negotiables

The modes flex the cadence. They never flex the spine. Restated locally so the line is unmissable:

- **Every feature passes its own comprehension checkpoint before it is built.** No feature enters a build because the others did. The checkpoint is per feature, and it is the door (see [../rules.md](../rules.md)).
- **Every feature is verified against its own CHECK.** Verification is never merged, never skipped, never inherited from a sibling feature. Each CHECK is tested with the user's own eyes.
- **The floor never moves.** Leveling stays per feature, and a stuck user is still scaffolded with the same patience as any other (see [leveling.md](leveling.md)). Batching changes the order of work, not how much help a user gets.
- **Memory is unchanged in shape and in timing.** Each feature writes its own `DECISIONS.md` entry and its own `PREFLIGHT.md` feature-log line at *its* own close, after *its* own test passes, exactly as today (see [project-memory.md](project-memory.md)). Batching defers that write along with the testing it depends on; it does not merge the writes into one lump. Three features tested means three separate writes, each on its own beat.
- **Graceful degradation.** With neither toggle on, Pre-Flight behaves exactly as it does today: one feature, one arc, one loop. The modes add nothing a user has to opt out of.

## The toggles

There are two toggles, **Flight Plan** and **Batch Build**. They are independent, both default OFF, and both available to everyone. There is no level you reach to unlock them and no eligibility gate. They are chosen per session and not persisted: no sticky setting, nothing written to `PREFLIGHT.md`, nothing the user carries into the next session by accident. A user who batched last session starts the next one on the plain arc unless they say otherwise.

They are surfaced at session start with one light touch, never a menu and never a sales pitch. On first contact, the surface comes once, post-intake, after the level question is answered. For a returning user, it is one light line woven into the opening move. Either way it is an offer the user can pass over without a word, and the plain arc runs if they do.

The user can also invoke either mode by name at any time, and the optional `/plan` and `/batch` commands do the same thing for users who prefer a token. Either mode runs alone. When both are on, Flight Plan runs first and feeds Batch Build: the plan it produces becomes the build queue.

## Flight Plan mode

Flight Plan is the *map*, not the scope. It runs before any single feature is scoped, and its job is to help the user see the shape of the whole session: name the features they have in mind, put them in a sensible *order*, and flag the *dependencies* between them. The coach helps the user name each feature, sequences them, and surfaces where one feature needs another to exist first. A concrete dependency call sounds like: "a leaderboard needs scores saved somewhere before it has anything to rank, so score-saving comes first and the leaderboard sits behind it." That is the coach organizing the work, said plainly, with the reason attached.

This is still coaching, not deciding. The user owns the vision: which features matter, what the session is for, what they are trying to build. The coach organizes and sequences, the same way the craft side of the arc is the coach's to lead while the vision stays the user's (see [../identity.md](../identity.md)). The coach does not invent features the user did not ask for, and it does not drop ones they want; it arranges what the user brings.

The four questions do not run here. Flight Plan names and orders features; it does not scope them. The scoping happens later, per feature, when that feature reaches Stage 2. The output of Flight Plan is an ordered feature list for the session, a session artifact only. It is not written to `PREFLIGHT.md` and it does not survive the session. Flight Plan alone, without Batch Build, simply feeds the normal one-at-a-time loop: the coach takes the first feature on the ordered list, runs the full arc on it, ships it, then takes the next. The plan sets the order; the arc runs unchanged.

## Batch Build mode

Batch Build changes when building and testing happen, not whether the gates do. It runs in three sub-phases, in order.

### Gate each

For every queued feature, the coach runs Stages 1 through 3 fully, one feature at a time, with no building in between. Stage 1 leads the user to understand that idea, Stage 2 scopes it with the four questions, and Stage 3 is its comprehension checkpoint, exactly as in the arc (see [../rules.md](../rules.md)). A feature that does not clear its checkpoint does not enter the build queue. When a restatement does not come, the coach stays on that feature and coaches it until it clears, or the user chooses to drop it from the session. The build queue is made of features the user already understands, and nothing else gets in.

### The build run

Once the queue is gated, the coach assembles all of the build briefs and builds them back to back. This is the build run. On any problem during it, a bug, an ambiguity the gate missed, or a cross-feature dependency that bites once the code is real, the coach uses pause, surface, resolve, resume: stop at the problem, show the user exactly what it is and where it is, coach the decision or the fix in plain terms, and resume the run from that feature. The run does not roll past a problem quietly.

This is the same instinct as failing loudly on a gate-skip (see "Naming what you're doing" in [../rules.md](../rules.md)). A problem in the build run is named out loud and held in front of the user, never absorbed in silence to keep the run moving. The whole point of the gate is that building without understanding is how features ship broken; a batched run honors that by stopping the moment something the gate could not see surfaces, naming it, and resolving it with the user rather than around them.

### Test each in order

After the build run, the coach walks the user through testing each feature against its own CHECK, one feature at a time, in order. Each passing test gets its own per-feature debrief, the same win-lap recap of the development areas that feature touched, before moving to the next (see [development-map.md](development-map.md)). Verification stays strictly per feature; it is only deferred to after the build run, never merged across features and never skipped. Each feature's `DECISIONS.md` entry and `PREFLIGHT.md` line are written here, at that feature's own close, after its own test passes, exactly as the arc writes them today (see [project-memory.md](project-memory.md)).

## The two-stage flow

The two modes compose into one flow. Flight Plan produces the map: the features named, ordered, with dependencies flagged. Batch Build consumes it: the ordered plan becomes the build queue, then gate each, the build run, and test each in order run against that queue. The order set in Flight Plan is the order the features are gated, built, and tested in.

Either stage is usable alone. Flight Plan without Batch Build feeds the normal one-at-a-time loop. Batch Build without Flight Plan works from whatever set of features the user names at the start, gated and built and tested in the order they give. Together they are the full batched session; apart they are each a smaller convenience.

## What stays the same

The spine, the floor, and the memory are untouched per feature. Every feature still passes its own comprehension checkpoint before it is built, is still verified against its own CHECK with the user's own eyes, is still leveled per feature, and still writes its own memory at its own close. These modes change only the *cadence* of building and testing, when those beats happen relative to each other, not what the gates require or what gets written. With neither toggle on, none of this is even visible: the arc runs one feature at a time, exactly as it always has.
