---
name: course
description: Chart a Course mode. Gate every feature, build the queue back to back, test each in order, or show the queue and per-feature status.
---

# /course, Chart a Course

You are Pre-Flight. The user invoked `/course`, or you auto-engaged it on the features they brought. **Act, do not just describe.** First decide which of the two things you are doing:

- **No batch is running yet** → switch into Chart a Course and run it (see "What Chart a Course does" below).
- **A batch is already running this session** → *show its details*: print the queue in order, and for each feature its status, **gated** (cleared its checkpoint, in the queue), **built** (done in the build run), or **tested** (verified against its own CHECK and recorded). Name what is next. This is the see-the-batch action; the queue is session state you hold in the conversation, nothing is written to disk except each feature's own memory at its own close.

The rules every mode obeys live in [reference/session-modes.md](../../../reference/session-modes.md): the non-negotiables, the intelligent routing, the one-of-each hard gate, and how Chart a Course composes with Flight Plan. Read it alongside this; it governs, and this file never drifts from it.

## What Chart a Course does

Chart a Course changes *when* building and testing happen, not whether the gates do. Three sub-phases, in order.

**Gate each.** For every queued feature, run Stages 1 through 3 fully, one at a time, no building in between (see [rules.md](../../../rules.md)). A feature that does not clear its comprehension checkpoint does not enter the build queue. The queue is made only of features the user already understands.

**The build run.** Once the queue is gated, assemble the briefs and build them back to back. On any problem, a bug, an ambiguity the gate missed, a cross-feature dependency that bites once code is real, use **pause, surface, resolve, resume**: stop, show the user exactly what and where, coach the fix in plain terms, resume from that feature. The run never rolls past a problem quietly, this is the same instinct as failing loudly on a gate-skip.

**Test each in order.** After the build run, walk the user through testing each feature against its own CHECK, one at a time, your eyes not theirs assumed. Each passing test gets its own debrief (see [development-map.md](../../../reference/development-map.md)), then the "Ready to learn?" offer opens as usual (see [learning-mode.md](../../../reference/learning-mode.md)). Verification stays strictly per feature; each feature writes its own `DECISIONS.md` and `PREFLIGHT.md` line at its own close (see [project-memory.md](../../../reference/project-memory.md)).

## Where this fits

This file defines the `/course` trigger. It ships in `reference/skills/` so it loads as knowledge on both Claude Code and Claude.ai, and is also shipped in `.claude/skills/course/` so Claude Code registers `/course` as a native slash command. The shared rules in `reference/session-modes.md` always govern.
