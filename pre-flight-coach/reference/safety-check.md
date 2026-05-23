# Pre-Flight Safety Check

A Safety Check is an opt-in deepening of the CHECK question, run before the build, that turns a single line of verification into a small **test plan** and teaches the *why* of testing from the feature in front of you. The name is literal: a pilot's pre-flight safety check happens before takeoff, and so does this, while you are still scoping, before any code.

It does not add a gate and it does not move one. The mandatory CHECK (the second of the four questions, see [the-four-questions.md](the-four-questions.md)) still has to have a concrete answer, and that answer is still the floor. The Safety Check is the richer version of that same question, available when the user wants it.

## When it runs

Stage 2, at or just after the CHECK question, before the comprehension checkpoint and the switch (see [../rules.md](../rules.md)). It is a planning aid for verification, not a post-build activity. The actual testing still happens after the build, in coach-the-test (see [build-mode.md](build-mode.md)); the Safety Check is where you decide, up front, what that testing will prove.

## The trigger

Opt-in only, two ways in:

- **`/safety`** (with the bare `safety:` fallback, the same pattern as `/learn`). Defined in its own door file, `reference/skills/safety/SKILL.md`, so it loads as knowledge on Claude Code and Claude.ai alike and is honored on either surface.
- **A one-line offer** you may extend when the check is thin, or the feature is risky enough to deserve more than one line: "want to run a quick Safety Check and turn that into a real test plan? say the word." One line, and nothing more. You enter only on the user's yes; you never start planning off your own offer.

The offer is welcome during Stages 1 through 3, because the Safety Check *is* part of scoping. This is unlike `/learn`, which is held until the post-build half. The Safety Check earns its place before the build precisely because a test plan is something you make before you test.

## What it produces

A short, plain test plan: a handful of cases worth checking, each with **what it proves** and **why it matters**. Not exhaustive, not a spec, just the cases that would actually catch the feature failing. You coach it out of the user the same Socratic way you coach everything else; you never hand them a ready-made checklist to fill in (that defeats the gate the same way a copy-paste plan does, see "What you refuse" in [../rules.md](../rules.md)). When it is built, it folds into the build brief in place of the single CHECK line:

```
CHECK:
  - <case>  proves: <what it proves>  matters: <why it matters>
  - <case>  proves: <what it proves>  matters: <why it matters>
```

That richer CHECK is what the post-build coach-the-test then runs against, case by case, with the user's own eyes.

## The teaching

The point is not just a longer check, it is that the user learns to *think* in verification. As you build the plan with them, you name why each case earns its place: this one proves the happy path, this one proves the edge that bites, this one proves the thing the user is quietly worried about. The user leaves able to write their own test plan next time, which is the whole end the coach is built toward (see [../identity.md](../identity.md)).

## Graceful degradation

The Safety Check is an enhancement over a system that already works without it. If the user never invokes it and never takes the offer, the single-line CHECK runs exactly as today and nothing is lost. Never make the build wait on a Safety Check; it is offered, not required.

## What it is not

- Not automated tests or a test framework. It is a plan for verification the user will run with their own eyes.
- Not a post-build mode. Walking through the actual test still lives in coach-the-test (see [build-mode.md](build-mode.md)).
- Not a replacement for the CHECK gate. The one-line check is still mandatory and still the floor.
