# Pre-Flight

*A coaching and building companion for new developers.*

---

## What this is

Pre-Flight is a coaching and building companion for new developers. It opens with one quick question to learn where you are on the road to shipping, then runs the four questions every feature: what done looks like, how you will verify it without trusting the machine, what is outside the task, and what context the build cannot see. When you and Pre-Flight have concrete answers, it builds the feature with you, then coaches you through testing it with your own eyes, then loops to the next one. It holds a gate before it builds, and it never stops coaching while it does.

Pre-Flight will not build until you have answered the four questions. That gate is the point.

---

## Who it's for

A new developer learning to build their own software for the first time, who has not yet shipped anything to real users. You might have never written a line of code, have a half-built project open right now, or have a finished thing on your machine you have never launched. Pre-Flight meets you at your first feature and stays with you through the ones after. If you are a seasoned developer, or you have already shipped real software to real users, this folder is not for you yet.

---

## How to use it

1. Clone this repo.
2. Activate Pre-Flight on your surface (see Activation below). On Claude Code, the folder activates automatically via `CLAUDE.md`. On Claude.ai, you set it up manually by pasting `CLAUDE.md` into Custom Instructions and uploading the rest as Project Knowledge.
3. Say hello. On a new project Pre-Flight introduces itself and asks one quick question to tailor your coaching, then takes it from there. It remembers you, so it only does that once per project.

---

## Activation

### Claude Code (auto-load)

Claude Code reads `CLAUDE.md` automatically when you open a workspace. After cloning, `cd` into the inner `pre-flight-coach/` folder (the one that holds `CLAUDE.md` and `reference/`) and start a conversation. Pre-Flight is active from your first message. No pasting, no manual setup.

### Claude.ai Projects (manual setup)

1. Create a new Claude.ai Project (or open an existing one). Open Project Settings → Custom Instructions. Paste the contents of `CLAUDE.md` into the Custom Instructions field.
2. Upload these 12 files as Project Knowledge, all found inside the `pre-flight-coach/` folder: `identity.md`, `rules.md`, `examples.md`, and the nine files in `reference/` (`the-four-questions.md`, `levels.md`, `project-types.md`, `habits.md`, `build-mode.md`, `project-memory.md`, `failure-modes.md`, `inquiry-patterns.md`, `avoidance-tells.md`). Claude.ai Projects does not accept folder uploads, so add the files individually. The folder structure does not matter for Claude; all project knowledge loads into context.
3. Start a fresh conversation. Pre-Flight is active from your first message.

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

## What's in this folder

| File | One job |
|---|---|
| `CLAUDE.md` | Activates the persona. Tells Claude it is Pre-Flight, points at the other files, and lists the three behaviors Pre-Flight must show from turn one. |
| `identity.md` | Defines who Pre-Flight is, who it serves, and what it will not do. |
| `rules.md` | Teaches the loop: the intake, the opening move, the four questions, the behaviors, the refusals, the switch into building. |
| `examples.md` | Five worked dialogues so the AI calibrates by example. |
| `reference/the-four-questions.md` | The canonical pre-flight checklist with worked good-answer vs hedge examples. |
| `reference/levels.md` | How the coach calibrates its register and scaffolding to the user's intake answer. |
| `reference/project-types.md` | How the coach adapts CHECK, CONTEXT, and OUT OF SCOPE to the inferred platform. |
| `reference/habits.md` | The three craft habits (git, logs, commit-before-risky-change), surfaced as questions. |
| `reference/build-mode.md` | The switch into building, how it builds, how it coaches the test, and the loop. |
| `reference/project-memory.md` | How Pre-Flight remembers a project across sessions so it introduces itself only once. |
| `reference/failure-modes.md` | The six week-one anti-patterns Pre-Flight watches for. |
| `reference/inquiry-patterns.md` | Socratic question templates the coach reaches for mid-conversation. |
| `reference/avoidance-tells.md` | Five language patterns that signal the user is dodging the work. |

---

## Built for

This coach was built for Skool Weekly Comp #5. The premise: a folder of markdown files that turns Claude into a domain-specific coach for a specific person at a specific moment. No server, no dependencies, no extra API key.

Pre-Flight grows with the developer. It starts at the first feature and walks the loop, coach, build, test, repeat, across a whole project, adapting as the developer's skill grows on the journey from `print("hello")` toward shipping full-stack work.

---

MIT. See `LICENSE`.
