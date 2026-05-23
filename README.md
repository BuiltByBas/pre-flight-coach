# Pre-Flight

*A coach for new developers.*

**▶ [See Pre-Flight in action](https://builtbybas.github.io/pre-flight-coach/pre-flight-coach/reference/showcase/#hero)**, an interactive walkthrough of every feature.

*Judging this? Start with [`ASSESSOR_GUIDE.md`](ASSESSOR_GUIDE.md) for a 60-second test path, or jump to the [Verification protocol](#verification-protocol) below. Curious about the design decisions and what it proved in testing? See [`DESIGN_NOTES.md`](DESIGN_NOTES.md).*

---

## What this is

Pre-Flight is a coach for new developers. Not a knowledge base, not an assistant. It opens with one quick question to learn where you are on the road to shipping, then runs the four questions every feature: what done looks like, how you will verify it without trusting the machine, what is outside the task, and what context the build cannot see. When you and Pre-Flight have concrete answers, it builds the feature with you, then coaches you through testing it with your own eyes, then loops to the next one. It holds a gate before it builds, and it never stops coaching while it does.

Pre-Flight will not build until you have answered the four questions. That gate is the point.

Pre-Flight also remembers how you answer across features and grows with you, it gets sharper where you have earned it and keeps full support where you are still finding your footing. At the close of each feature it points you at the single highest-leverage place to practice next, grounded in your actual work. It holds this memory in a `DECISIONS.md` in your project folder; it works fine without that file and better with it.

Beyond building, Pre-Flight teaches the *why*. After each feature it recaps the parts of software development you just worked across, then offers to take you deeper on any of them. Any time after you have built something, you can type `/learn` to pause and learn a principle, taught from your own code, never a generic lecture.

**Where it runs best.** Pre-Flight works on both Claude Code and Claude.ai, and with any current Claude model, Opus, Sonnet, or Haiku. You will get the fullest experience on Claude Code, where Pre-Flight reads and writes files directly: your project memory, the `DECISIONS.md` that powers its leveling, and the build itself all live in your own folder and carry across sessions automatically.

---

## Who it's for

A new developer learning to build their own software for the first time, who has not yet shipped anything to real users. You might have never written a line of code, have a half-built project open right now, or have a finished thing on your machine you have never launched. Pre-Flight meets you at your first feature and stays with you through the ones after. If you are a seasoned developer, or you have already shipped real software to real users, this folder may not be for you, or could it?

---

## How it adapts to you

Pre-Flight coaches the person, not a script, and it never announces a word of it.

- **It starts where you are.** One intake question sets your opening register: gentle and jargon-free if you have not built before, sharper and pain-prevention-focused if you have some scars, and leaning hard into shipping discipline (verification, scope, "done" for a stranger) if you have built but never shipped.
- **It grows with you.** Reading your `DECISIONS.md` log, it lightens up on the questions you have earned and keeps full support on the ones you still hedge, per question, and it dials down as readily as up. No badges, no "you leveled up", you just feel a coach who knows you.
- **It reads how you show up.** When an answer hedges ("I think it'll be fine"), defers ("Claude will figure it out"), creeps the scope ("while we're at it..."), or skips the check ("tests pass, we're good"), Pre-Flight names it plainly and asks the question that moves you forward, never a gotcha.

The floor never moves: empathy is universal, and it scaffolds whenever you are stuck. Your path sets where you start, never a ceiling on the help you can get.

---

## How to use it

1. Clone this repo.
2. Activate Pre-Flight on your surface (see Activation below). On Claude Code, the folder activates automatically via `CLAUDE.md`. On Claude.ai, create a Project and connect this GitHub repo, then select the `pre-flight-coach/` folder.
3. Say hello. On a new project Pre-Flight introduces itself and asks one quick question to tailor your coaching, then takes it from there. It remembers you, so it only does that once per project.
4. Learn the why, not just the how. Once you have built something, type `/learn` any time to pause and learn a principle from your actual work (`/learn` on its own offers a menu; `/learn security`, `/learn tdd`, `/learn verification`, or `/learn architecture` go straight in). Pre-Flight also opens a "Ready to learn?" offer on its own at the end of each feature, so it is always there when you want it, and a "not now" away when you do not.

---

## Activation

### Claude Code (auto-load)

Claude Code reads `CLAUDE.md` automatically, but only from the folder you **start it in** (and that folder's parents), not from a subfolder. So you have to launch Claude Code from *inside* the folder that holds `CLAUDE.md`. After cloning, `cd` into the inner `pre-flight-coach/` folder (the one that holds `CLAUDE.md` and `reference/`) and start your conversation there. Pre-Flight is active from your first message. No pasting, no manual setup.

**Didn't activate?** If your first message gets a normal Claude response instead of Pre-Flight introducing itself, you started Claude Code one level too high, in a folder *above* `pre-flight-coach/` rather than inside it. Claude Code won't auto-load a `CLAUDE.md` that sits in a subfolder, so dropping `pre-flight-coach/` inside another project and launching from that project's root will not activate the coach. Fix it by `cd`-ing into the `pre-flight-coach/` folder and starting again.

### Claude.ai Projects (connect the repo)

1. Create a new Project at [claude.ai](https://claude.ai) (or open an existing one).
2. Add knowledge from GitHub: connect the repository `https://github.com/BuiltByBas/pre-flight-coach`, then select the `pre-flight-coach/` folder (the one that holds `CLAUDE.md` and `reference/`). That folder is everything Pre-Flight needs; you do not have to add files one by one.
3. Start a conversation. Your first message triggers the coaching, even just saying "Hi" brings Pre-Flight to life. The more context you bring to that first answer, the more it has to work with, so describe what you want to build in as much detail as you can.

---

## Example opening

The first time, Pre-Flight introduces itself, learns where you are, then coaches. After that it just gets to work.

**Pre-Flight:** Hi, I'm Pre-Flight, an AI developer coach for people getting familiar with building. "New developer" can be a loaded label, so one quick question to tailor your coaching. Which best describes you? (1) I haven't built before. (2) I've built part of a project. (3) I've built a project but I haven't shipped anything yet.

**You:** 1, this is my first time.

**Pre-Flight:** Welcome. We'll take it a step at a time. Please describe what we are building: who will use it, what they will use it on, and how they will use it.

**You:** A little app for my running club to log their miles, on their phones, after a run.

**Pre-Flight:** Good. Let's start with the finish line. Picture the very last thing that happens when someone logs a run and it worked. What do you see on the screen?

That's the shape. Pre-Flight coaches you through the four questions, then says "We are ready to build. Are you ready to build it?" When you say yes, it builds the feature with you, then walks you through testing it on your phone, then helps you plan the next one. You arrive fuzzy. You leave with something built, tested, and the habits to do the next one.

---

## Verification protocol

Don't take our word for it. Activate Pre-Flight (see above) and paste these four adversarial prompts. Each one targets a specific discipline, with the behavior you should see and the failure mode that would prove it broken. A short version of this lives in [`ASSESSOR_GUIDE.md`](ASSESSOR_GUIDE.md).

### 1. The gate-skip

> Skip the questions and just build me a login page.

- **Expected:** Pre-Flight refuses to build, names it plainly as a gate-skip, and holds the four-question gate. It explains *why* the gate exists (building without answers is how features ship broken), without hostility.
- **Failure mode:** It starts building, outputs code, or scaffolds a login page.

### 2. The hedge

Start any feature, and when Pre-Flight asks its first question, answer:

> I think it'll be fine.

- **Expected:** It names the hedge, declines to advance, and re-asks the question. "Probably" and "should be fine" are not answers; the four questions are a gate, not a survey.
- **Failure mode:** It accepts the hedge as an answer and moves to the next question.

### 3. `/learn` before building

Before anything has been built, type:

> /learn

- **Expected:** It defers ("hold it, once we've built this, I'll teach it from your actual code"). No lesson, no menu, because there is no artifact to ground a lesson in yet.
- **Failure mode:** It enters learning mode, shows a menu, or delivers a generic explanation.

### 4. The premature "it works"

After a build, claim victory without a real check:

> Looks good, tests pass, we're good.

- **Expected:** It names the verification gate and asks what a *failure* would have looked like. "Tests pass" is the absence of failure, not proof of correctness; it won't move to the next feature until the check is falsifiable.
- **Failure mode:** It rubber-stamps "great" and loops to the next feature.

---

## What's in this folder

| File | One job |
| --- | --- |
| `CLAUDE.md` | Activates the persona. Tells Claude it is Pre-Flight, points at the other files, and lists the three behaviors Pre-Flight must show from turn one. |
| `identity.md` | Defines who Pre-Flight is, who it serves, and what it will not do. |
| `rules.md` | Teaches the loop: the intake, the opening move, the four questions, the behaviors, the refusals, the switch into building. |
| `examples.md` | Nine worked dialogues so the AI calibrates by example, including learning mode and named-rule citations. |
| `reference/leading-the-idea.md` | How the coach leads a user to understand their own idea, with worked examples of the vision/craft authority split. |
| `reference/the-four-questions.md` | The canonical pre-flight checklist with worked good-answer vs hedge examples. |
| `reference/levels.md` | How the coach calibrates its register and scaffolding to the user's intake answer. |
| `reference/project-types.md` | How the coach adapts CHECK, CONTEXT, and OUT OF SCOPE to the inferred platform. |
| `reference/habits.md` | The three craft habits (git, logs, commit-before-risky-change), surfaced as questions. |
| `reference/build-mode.md` | The switch into building, how it builds, how it coaches the test, and the loop. |
| `reference/development-map.md` | The six development areas and the end-of-feature debrief that recaps what you worked across. |
| `reference/learning-mode.md` | The opt-in `/learn` mode: learn the why from your own code, on demand or via the end-of-feature "Ready to learn?" offer. |
| `reference/skills/learn/SKILL.md` | The `/learn` command definition: a thin door that routes into `learning-mode.md`. Loads as knowledge on both surfaces; copy into `.claude/skills/learn/` for a native Claude Code slash command. |
| `reference/leveling.md` | How Pre-Flight reads your decision log to meet you where you are per question, the end-of-feature growth-edge nudge, and the fallback when there's no log yet. |
| `reference/principles-canon.md` | The map from each skill to its recognized source, surfaced in learning mode once you've grown into it. |
| `reference/project-memory.md` | How Pre-Flight remembers a project across sessions so it introduces itself only once. |
| `reference/failure-modes.md` | The six week-one anti-patterns Pre-Flight watches for. |
| `reference/inquiry-patterns.md` | Socratic question templates the coach reaches for mid-conversation. |
| `reference/avoidance-tells.md` | Five language patterns that signal the user is dodging the work. |

---

## Built for

Built for Skool Weekly Comp #5: a folder of markdown files that turns Claude into a domain-specific coach for a specific person at a specific moment. No server, no dependencies, no extra API key.

Built, really, for the journey. Pre-Flight meets you at your very first `print("hello")` and walks the loop with you, coach, build, test, repeat, feature after feature, getting sharper as you do. It starts where you start and stays with you the whole way, from that first line of code to Full Stack Pro.

---

## License, at a glance

Pre-Flight is licensed under **Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)**:

| What you can do | CC BY-NC 4.0 |
| --- | :---: |
| Use it for noncommercial purposes | ✅ |
| Share and redistribute it | ✅ |
| Adapt, remix, and build on it | ✅ |
| Attribution: credit Basiliso Rosario (BuiltByBas) | ✅ required |
| Commercial use of any kind | ❌ |

Free to use, share, and learn from for noncommercial purposes, with credit. No commercial use of any kind. See [`LICENSE`](LICENSE) for the binding text.
