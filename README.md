# Pre-Flight

*A coaching tool for new developers using Claude Code.*

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
3. Open a conversation with `Tell me what you're about to build.` as the opening cue. Pre-Flight does the rest.

---

## Activation

### Claude Code (auto-load)

Claude Code reads `CLAUDE.md` automatically when you open a workspace. After cloning, `cd` into the inner `pre-flight-coach/` folder (the one that holds `CLAUDE.md` and `reference/`) and start a conversation. Pre-Flight is active from your first message. No pasting, no manual setup.

### Claude.ai Projects (manual setup)

1. Create a new Claude.ai Project (or open an existing one). Open Project Settings → Custom Instructions. Paste the contents of `CLAUDE.md` into the Custom Instructions field.
2. Upload these 7 files as Project Knowledge, all found inside the `pre-flight-coach/` folder: `identity.md`, `rules.md`, `examples.md`, and the four files in `reference/` (`the-four-questions.md`, `failure-modes.md`, `inquiry-patterns.md`, `avoidance-tells.md`). Claude.ai Projects does not accept folder uploads, so add the files individually. The folder structure does not matter for Claude; all project knowledge loads into context.
3. Start a fresh conversation. Pre-Flight is active from your first message.

---

## Example opening

The first 60 seconds look something like this. You arrive fuzzy. Pre-Flight scaffolds.

**You:** I want to clean up my login page. It's gotten kind of messy.

**Pre-Flight:** Before we shape what we're going to build, I want to understand what "messy" means to you. What does the login page look like right now that made you want to clean it up?

**You:** I don't know, it's just a lot. The file is really long. There's a bunch of stuff for the email field and the password field. It all feels jammed together.

**Pre-Flight:** Okay. So when you say "clean up," part of what you mean is the file feels too long. What does the page actually do, from the user's side, when someone visits it?

That's the shape. Pre-Flight scaffolds you through all four questions until you have concrete answers, then hands you a pasteable prompt to open with Claude. You arrive fuzzy. You leave with a prompt that will hold under pressure.

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

Pre-Flight is the first stop in a longer arc. Future coaches will cover the stages beyond week two, each tuned for a different point on the journey from `print("hello")` to shipping full-stack work.

---

MIT. See `LICENSE`.
