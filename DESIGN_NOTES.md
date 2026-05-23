# Pre-Flight, Design Notes

*A short design rationale for the coach: why it exists, the decisions behind it, what it proved, and what comes next.*

---

## Why this domain

The brief asked for a folder-based AI coach. The easy targets were the safe ones: a writing coach, an interview coach, a generic "learn to code" tutor. Useful, but none of them were the thing the moment actually needs.

The real problem is the one a whole wave of people are living right now: they are building software with an AI that will happily write anything they ask for, and that is exactly the danger. The AI never says "wait, do you understand what you just asked me to build?" It never holds a line. It ships whatever the prompt says, and the person ends up with a working-looking app they cannot read, cannot verify, and cannot defend.

Pre-Flight is built for that person: the new developer, the vibecoder, the founder building their own first tool. Not to slow them down, but to make sure that when the work is done, they own it.

The domain is not "learning to code" in the abstract. It is the specific judgment work of building real software with an AI:

- understanding your own idea before any code exists
- knowing what "done" looks like before you start
- verifying with your own eyes instead of trusting "tests pass"
- naming what is out of scope so the build stays honest
- carrying the craft habits (save before risky changes, leave a trail) that survive past the first feature

That specificity shaped the whole folder.

---

## The core coaching rule

The design principle behind Pre-Flight is simple:

**Earn the right to build.**

That is the line between a coach and a code generator. A code generator hears "build me a login page" and starts typing. A coach asks what the page is for, what "working" means, what happens when the password is wrong, and what it cannot see from where it sits. Then, and only then, it builds.

So Pre-Flight is not designed to be helpful first. It is designed to be honest first. It leads the user to understand their own idea, runs a four-question gate before writing a line of code, and refuses to advance until the user can restate, in their own words, what is being built and how they will know it worked. A yes is not enough. A restatement is.

The behavior is encoded across the folder:

- `identity.md` defines who Pre-Flight is, who it serves, and what it refuses to do.
- `rules.md` defines the five-stage arc, the four questions, the comprehension checkpoint, and the loop.
- `examples.md` shows the calibration target: nine worked dialogues of the coaching voice under different opening conditions.
- `reference/` holds the frameworks, loaded only when the moment calls for them.
- The user's project files (`PREFLIGHT.md`, `DECISIONS.md`) carry memory and the leveling signal across sessions.

The working mechanism became:

> Pre-Flight does not let a build start vague. Every feature passes through a gate: a clear definition of done, a real way to verify it, an explicit out-of-scope, and a named account of what the build cannot see, before any code is written.

That is the bar. If code gets written before the user understands what they asked for, Pre-Flight has not done its job.

---

## Architecture decisions

### 1. Memory is two files with two distinct jobs

Memory could have been a single log of everything that happened. That would have been easy and useless. Instead it splits into two files, each with one job, both living only in the user's own project:

| File | Job | Shape |
| --- | --- | --- |
| `PREFLIGHT.md` | Human-readable continuity: that the intro happened, the user's level, and a feature log of what was built and how it was tested | Written for a person to read |
| `DECISIONS.md` | The leveling signal: a structured, per-feature record of how the user answered the four scoping questions | Written for the coach to read a trend |

The split matters. `PREFLIGHT.md` is the trail a person can follow. `DECISIONS.md` is the substrate that powers adaptive leveling: across features, the coach reads a trend in how this user thinks and meets them there. Keeping them separate keeps each one honest about its purpose. Neither is a marketing document, and the coach never fabricates a signal to make the trend look better than it was.

### 2. Leveling is the system, not a feature

The headline is a coach that talks to you differently on feature five than on feature one. The level is set once at intake, then the decision log takes over: the coach stops over-scaffolding what the user has mastered, reuses the vocabulary they have earned, and pushes harder where they are ready. It dials down as readily as up, and it never announces itself.

This is also the highest reliability risk in the whole design, because it leans on the coach actually reading the log every session, in a prose system that cannot force it to. So leveling was built for **graceful degradation**: if the log is missing or unread, the coach falls back to the static intake level and loses nothing essential. The log is an enhancement layer over a system that already works without it.

### 3. Reference material is loaded on demand, not all at once

The `reference/` folder is not loaded into every session. The always-on context stays focused on coaching behavior (`identity.md`, `rules.md`); the heavier material loads only when the moment calls for it: project-type adaptation, the failure modes, the avoidance tells, the learning-mode catalog, the principles canon. This keeps the coach fast and focused, and it keeps the core behavior files from bloating into an encyclopedia.

### 4. Learning is grounded in the user's own work, never a generic lecture

Pre-Flight teaches, but only after building, and only from the user's real artifact. The `/learn` mode is opt-in and post-build: it pauses to teach the *why* from the code the user just wrote, never from a pre-written lesson. Ask it to teach before anything is built and it defers ("once we've built this, I'll teach it from your code"). A principles canon maps each skill to its recognized authoritative source, so the framing is standard and has somewhere to scale toward, but the lesson always starts from what the user actually made.

### 5. Discipline is teachable, not a gotcha

When Pre-Flight enforces a gate or catches a dodge, it names the rule it is applying. A gate-skip is called a gate-skip. A hedge is called a hedge. The point is not to win the exchange. It is to make the discipline legible, so the user learns the move and can run it themselves next time.

---

## Character and tone

The name is literal. A pre-flight check is the ritual a pilot runs before every takeoff: not because they expect failure, but because the cost of skipping it is too high. That is the posture the coach takes toward building software. Check the things that matter before you commit, every time, no shortcuts.

The voice is empathetic candor: patient, direct, and never placating. It describes behavior, not the person. It leads with curiosity over assumption, especially when a user is stuck or frustrated, where it steps back and asks what is actually happening rather than drilling the same question or telling a blunt builder to "take a break." Directness is not disrespect, and the coach is built not to confuse the two.

The register is not fixed. It calibrates to the user through leveling: lighter scaffolding and sharper questions as the user earns them, full support where they still hedge. That adaptation is the personality, applied at the right moment.

---

## The showcase choice

The repo ships an interactive showcase page rather than a live, API-backed demo.

That was deliberate. A live demo would be more technically impressive, but it would introduce avoidable problems for a public submission: exposed-credential risk, token cost, rate limits, inconsistent output, and the chance of failing in front of a judge.

The showcase does a different job. It demonstrates the intended coaching experience reliably: the gate holding the line, the adaptive leveling, the mode labels, the named-rule citations, all walkable without installing anything. For a submission, controlled experience quality mattered more than live generation.

---

## Proof of concept

Pre-Flight was not only designed. It was validated end-to-end across three live tests before submission, with a learner role-played against the real folder.

1. **Single feature, first contact (Claude.ai).** A first-time user built a countdown. This proved the core arc and the memory substrate: the coach wrote `PREFLIGHT.md` and `DECISIONS.md` from a real session, and it degraded gracefully with no prior log to read.

2. **Multi-feature session, roughly four hours (Claude Code).** A beginner grew across three features: a countdown, then hourly beeps with a midnight alarm, then blooming flowers. This is where the system proved itself. The coach:
   - shrank a question down to a falsifiable check when the user got stuck ("i will see it?" became a concrete, observable test)
   - logged honest per-question signal, including "reached after heavy scaffolding," not flattering self-assessment
   - **moved the growth edge as the user grew**: verification on feature one, version-control and safety on feature two, because verification had genuinely improved. That movement is the headline proof that the leveling is real and not a gimmick.
   - caught its own bug (an on-the-hour beep error) and held the user's own check-versus-goal mismatch
   - backed up a working file before a risky edit, then used learning mode to teach why git is the scale-up

3. **Cold restart (Claude Code, fresh session).** A brand-new session with zero conversation history opened the same project folder, detected the returning user, skipped the intro and intake, and recalled specific feature texture ("the after-idle clipping you debugged on your own") read straight from the memory files. This proved continuity is driven by the files, not by chat memory.

The verdict from that validation: the promise, a coach with memory that grows with the user, was demonstrated rather than asserted.

---

## What I would improve next

Pre-Flight is submission-ready. It is not finished.

1. **Confirm the register calibrates cold, not just the greeting.** The cold-restart test proved continuity and recall. The one thing left to confirm is that the very first scoping turn of a cold session opens sharper on the user's strong axes by reading the decision log alone. Expected to pass, worth a direct check.

2. **Refine the growth-edge timing.** In testing, the growth-edge nudge fired usefully after a single feature when the struggle was genuine, slightly ahead of the multi-feature window the design described. That was grounded and it landed well, so the rule now allows a real single-feature edge while still promoting the overall register slowly. More sessions will show whether that balance holds.

3. **Document the drop-in story honestly.** Claude Code loads the persona only when launched from inside the folder that holds `CLAUDE.md`, not from a nested subfolder. "Drop it into any project and it takes over" is not possible by design, and the README now says so plainly. A future version could package the persona as an explicitly invoked skill for existing projects.

4. **Test with people who are not the author.** The coach was validated through role-played sessions, which is a real signal but a controlled one. Outside users will expose blind spots that role-play cannot.

5. **Grow the principles canon as users level up.** The canon maps beginner slices to authoritative sources today. The senior-end material (reliability, cloud design, systems engineering) is scaffolding for where the coach scales, and it needs real leveled-up sessions to pressure-test.

---

## What this project demonstrates

Pre-Flight is a folder, but the design work is the system around it:

- a specific domain with recurring judgment problems
- a coaching behavior model with a gate that actually refuses, not just a helpful prompt
- transparent, file-based memory that carries both continuity and a leveling signal
- adaptive leveling that meets the user where they are and grows with them
- learning grounded in the user's own artifact, never a generic lecture
- named-rule citations that make discipline teachable
- a showcase that makes the experience legible before anyone opens the repo

The folder is the product. The harder design challenge was building a coach that can hold a line, adapt to the person in front of it, and make sure the user owns the work when the build is done.

---

*Built by Basiliso Rosario (BuiltByBas) for Skool Weekly Comp #5, May 2026. Licensed CC BY-NC 4.0, see [`LICENSE`](LICENSE).*
