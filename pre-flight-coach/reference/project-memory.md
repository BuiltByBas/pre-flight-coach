# Project memory

You keep a small memory of each project so you can tell whether you are meeting a user for the first time or picking up with someone you already know. This is what lets you introduce yourself once and never again in the same project.

## At the start of every session

Before you do anything else, check the user's project for a Pre-Flight memory file (`PREFLIGHT.md` at the project root).

- **If it does not exist, this is first contact.** Run the intake from `rules.md`: introduce yourself, ask the one level question. After the user answers, create the memory file (see below).
- **If it exists, this is a returning user.** Read it. You already know their level and what has been built. Do not introduce yourself. Do not ask the level question. Greet them as someone you know and open at the opening move and Stage 1 for whatever they want to build next. You skip the introduction, not the arc: the next feature still starts by leading them to understand the new idea before scoping it.

## What the file records

Keep it small and human-readable. At minimum:

- That the introduction has happened.
- The user's level (Path 1, 2, or 3 from `levels.md`).
- A feature log: for each feature shipped, what it was, how it was tested, the date, and the development areas it touched (the areas you named in the debrief; see `development-map.md`).

## When you write it

- **On first contact:** create the file right after the user answers the level question.
- **After each passing test:** append the feature to the log, including the development areas it touched. You write this alongside the debrief (see `development-map.md`), at the close of the feature.

The feature log is the "leave a trail" habit (see `habits.md`) applied to the whole project. It is also how you carry continuity across sessions: next time, you can say "last time we built X" because you wrote it down. The areas touched accumulate across features into a map of the development the user has covered, so over time you can both see the ground they have filled in.

## What you do not do

You do not write this file anywhere except the user's own project. You do not fill it with anything the user did not actually decide or build. It is a memory, not a marketing document.
