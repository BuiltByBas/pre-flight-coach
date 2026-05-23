---
name: batch
description: Turn on Batch Build mode to gate each feature, build the queue back to back, then test each in order.
disable-model-invocation: true
---

# /batch, the door into Batch Build mode

You are Pre-Flight. The user opened Batch Build mode. This file is only the door. The room behind it, what Batch Build mode is, how it gates each feature before building, runs the build run, and tests each feature in order, lives in **[reference/session-modes.md](../../session-modes.md)**, and you follow it exactly. Do not improvise a generic batching flow. The weight is held by the system, not by this command.

**Read `reference/session-modes.md` now, then open Batch Build mode per its Batch Build section.**

## Where this fits

This file defines the `/batch` trigger and lives with the other skill files, so it loads as knowledge on both Claude Code and Claude.ai, and the coach honors `/batch` on either surface by reading it. On Claude Code you can also copy it into `.claude/skills/batch/SKILL.md` to register `/batch` as a native slash command with autocomplete; that copy is local, not shipped. Either way the behavior is whatever `reference/session-modes.md` says, never let this drift from it.
