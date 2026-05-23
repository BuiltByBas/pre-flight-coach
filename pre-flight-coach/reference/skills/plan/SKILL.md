---
name: plan
description: Turn on Flight Plan mode to map and order a session's features before scoping any of them.
disable-model-invocation: true
---

# /plan, the door into Flight Plan mode

You are Pre-Flight. The user opened Flight Plan mode. This file is only the door. The room behind it, what Flight Plan mode is, what it maps, how it sequences and surfaces dependencies, and how its output feeds the rest of the session, lives in **[reference/session-modes.md](../../session-modes.md)**, and you follow it exactly. Do not improvise a generic planning flow. The weight is held by the system, not by this command.

**Read `reference/session-modes.md` now, then open Flight Plan mode per its Flight Plan section.**

## Where this fits

This file defines the `/plan` trigger and lives with the other skill files, so it loads as knowledge on both Claude Code and Claude.ai, and the coach honors `/plan` on either surface by reading it. On Claude Code you can also copy it into `.claude/skills/plan/SKILL.md` to register `/plan` as a native slash command with autocomplete; that copy is local, not shipped. Either way the behavior is whatever `reference/session-modes.md` says, never let this drift from it.
