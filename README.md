# Pre-Flight

*A Claude Code coach for solo developers in their first two weeks.*

---

## What this is

Pre-Flight is a coach, not a tutorial. It is built for one person: a solo developer who has installed Claude Code, finished a tutorial, and is now about to ask it to do their first real task on a project that matters. The coach runs a single loop, the same four questions in the same order every time: what done looks like, how you will verify it without asking Claude, what is outside the task, and what context Claude cannot see by reading the file. Pre-Flight will not explain features, generate templates, or hand you a checklist to fill out before opening the conversation.

Pre-Flight will not write code for you. That is the point.

---

## Who it's for

A solo developer in their first two weeks of using Claude Code, who has installed it and finished a tutorial, and is now about to ask it to do their first real task on a project that matters to them. If that is not you, this folder is not for you yet.

---

## How to use it

1. Clone this repo.
2. Activate Pre-Flight on your surface (see Activation below). On Claude Code, the folder activates automatically via `CLAUDE.md`. On Claude.ai, you set it up manually by pasting `CLAUDE.md` into Custom Instructions and uploading the rest as Project Knowledge.
3. Open a conversation with `Tell me what you're about to ask Claude to do.` as the opening cue. Pre-Flight does the rest.

---

## Activation

### Claude Code (auto-load)

Claude Code reads `CLAUDE.md` automatically when you open a workspace. After cloning, `cd` into the folder and start a conversation. Pre-Flight is active from your first message. No pasting, no manual setup.

### Claude.ai Projects (manual setup)

1. Create a new Claude.ai Project (or open an existing one). Open Project Settings → Custom Instructions. Paste the contents of `CLAUDE.md` into the Custom Instructions field.
2. Upload these 7 files as Project Knowledge: `identity.md`, `rules.md`, `examples.md`, and the four files in `reference/` (`the-four-questions.md`, `failure-modes.md`, `inquiry-patterns.md`, `avoidance-tells.md`). Claude.ai Projects does not accept folder uploads, so add the files individually. The folder structure does not matter for Claude; all project knowledge loads into context.
3. Start a fresh conversation. Pre-Flight is active from your first message.

---

## Example opening

The first 60 seconds look something like this.

**You:** I want to add a dark mode toggle to my app.

**Pre-Flight:** Tell me what done looks like. Not the feature, the outcome.

**You:** I guess the toggle switches the theme and it persists across page reloads?

**Pre-Flight:** "I guess" is not done. Say it in one sentence, the way you would write it in a commit message.

**You:** Clicking the dark mode toggle switches the theme and saves the preference to localStorage. On reload, the saved preference is applied before the page renders.

**Pre-Flight:** That's a real definition of done. How will you know it worked, without asking Claude?

That exchange is the whole model. Pre-Flight keeps going through the remaining three questions until all four have concrete answers, then hands you a pasteable prompt to open with Claude.

---

## What's in this folder

| File | One job |
|---|---|
| `CLAUDE.md` | Activates the persona. Tells Claude it is Pre-Flight, points at the other files, and lists the three behaviors Pre-Flight must show from turn one. |
| `identity.md` | Defines who Pre-Flight is, who it serves, and what it will not do. |
| `rules.md` | Teaches the coaching loop: the opening move, the four questions, the six behaviors, the refusals, the clean exit. |
| `examples.md` | Five worked dialogues so the AI calibrates by example. |
| `reference/the-four-questions.md` | The canonical pre-flight checklist with worked good-answer vs hedge examples. |
| `reference/failure-modes.md` | The six week-one anti-patterns Pre-Flight watches for. |
| `reference/inquiry-patterns.md` | Socratic question templates the coach reaches for mid-conversation. |
| `reference/avoidance-tells.md` | Five language patterns that signal the user is dodging the work. |

---

## Built for

This coach was built for Skool Weekly Comp #5. The premise: a folder of markdown files that turns Claude into a domain-specific coach for a specific person at a specific moment. No server, no dependencies, no API key beyond the one Claude Code already uses.

---

MIT. See `LICENSE`.
