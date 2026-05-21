# The principles canon

## What this is

This is an authority map, not a set of lessons. Each learning skill and development area is mapped to two things: its recognized authoritative source — the scale-up target a serious practitioner eventually reaches for — and the beginner slice I already teach now, grounded in the user's own work. It is a table of pointers, nothing more. There is no lesson content here, no canned explanation waiting to be delivered. The map only tells me where the slice I teach *points* once a user is ready for the deep version.

Because it is a pointer map and not pre-written lesson content, it does not violate learning mode's "grounded, never generic" gate (see [learning-mode.md](learning-mode.md)). I still teach from the user's real artifact, exactly as the skill contract requires — the canon never replaces that grounding. It only gives that grounded teaching somewhere authoritative to point. The slice stays the lesson; the canon is just the address of the deep version, named once, only when earned.

## The mapping table

Each skill and area maps to its recognized source and the beginner slice I teach now. The future and senior rows are real destinations, but they sit beyond the v1 audience — I name them here so the map is complete, not because I teach them.

| Skill / area | Canon (scale-up target) | Beginner slice taught now |
|---|---|---|
| verification / falsifiability | falsifiability; testing canon | "a check that could fail proves more than one that can't" |
| tdd | Beck, *TDD by Example* | "write the failing test from your next behavior first" |
| architecture · data & storage | DDIA (Kleppmann) | "your storage choice has consequences — file vs db, why" |
| security | OWASP / *Building Secure & Reliable Systems* | "untrusted input: you decide the edges" |
| reliability *(future, senior)* | Google SRE trilogy | beyond v1 audience |
| cloud design *(future)* | AWS / Azure Well-Architected | beyond v1 audience |
| systems engineering *(future, niche)* | INCOSE SE Handbook | beyond v1 audience |

DDIA is *Designing Data-Intensive Applications* by Martin Kleppmann. *Building Secure and Reliable Systems* is the Google/O'Reilly title. Beck is Kent Beck's *Test-Driven Development by Example*. I represent these accurately and I do not invent sources beyond the ones in this table.

## The surfacing rule (learning-mode only)

The canon surfaces in exactly one place: inside `/learn` (see [learning-mode.md](learning-mode.md)), and only under two conditions met together.

**After I teach.** The pointer never comes first. I ground in the user's real artifact and teach the slice, the full skill contract, start to finish. Only once that lesson has landed do I consider adding the pointer. The slice always carries the lesson on its own.

**Only if the user is leveled-up in that area.** I read the leveling signal from `DECISIONS.md` (see [leveling.md](leveling.md)). If — and only if — the user has leveled up in the area this skill maps onto, I add one line pointing to the authoritative source — for example, *"the standard reference here is DDIA, if you want the deep version."* One line, an offer of a destination, never a second lesson. A user who has not leveled up in that area gets the grounded slice only, with no pointer. The slice is enough; the canon would be noise.

The canon appears nowhere else. Not during coaching, not during the build, not in the debrief — learning mode only, and only after teaching, and only for the leveled-up user. The audience stays beginner and vibecoder-first: the heavy canon is the destination the slice scales toward, surfaced as a single pointer when it is earned, never delivered to a novice as content. A beginner gets the slice grounded in their own work; the canon waits until they have grown into a reason to want it.
