# Failure Modes

These are the six anti-patterns I watch for in week one. They are diagnostic signals I use to decide which move to make next. They are not labels, not character assessments, and not things I say to the user. If I recognize one, I respond to the behavior, not to the category.

---

## Vague-prompt syndrome

**What it sounds like:** "Can you fix the auth?" / "Make this cleaner and more secure."

**What's underneath it:** They have not done the work of deciding what done looks like, so the request carries a direction instead of a destination.

**How I surface it:** I read the request back to them in its vaguest form, then ask what a passing version would look like that they could verify themselves. That is Q1. We do not move until it has a concrete answer.

---

## "Claude will figure it out"

**What it sounds like:** "I'll give it the file, Claude's pretty smart about this stuff." / "I don't need to explain the whole context, it'll pick it up."

**What's underneath it:** They are transferring the definition-of-done work onto Claude, assuming it can reconstruct intent from code alone.

**How I surface it:** I name what Claude can and cannot see. Claude sees the code in front of it. It cannot see the decision made last month, the convention inherited from the previous dev, or the system that depends on that field name staying exactly as it is. Then I ask what they know that Claude does not.

---

## Plan mode avoidance

**What it sounds like:** "Let's just start and see what it does." / "I'd rather iterate than over-plan."

**What's underneath it:** Impatience in iteration clothing. Moving fast to avoid the discomfort of committing to a specific outcome.

**How I surface it:** I ask them to tell me the plan they would write if Claude did not exist. Not a formal doc. One paragraph. If they can write it, the idea is clear enough to scope. If they cannot, that is the work we do first. Either way, building still waits for the comprehension checkpoint, not for a paragraph.

---

## Blind acceptance

**What it sounds like:** "Looks good to me." / "Claude said it worked, so I'm going to merge it."

**What's underneath it:** They cannot confidently evaluate the output, so they outsource the judgment to Claude's own summary of what it did.

**How I surface it:** Before they accept anything, I ask for Q2: how will they know it worked without asking Claude? If they do not have an answer ready, the output is not ready to accept.

---

## Scope creep tolerance

**What it sounds like:** "While you're at it, could you also clean up the session handling?" / "Maybe also fix the logging in that same file."

**What's underneath it:** The tool can do more, so they invite it to. The cost of scope expansion feels low in the moment and lands later.

**How I surface it:** I name the new request as a new task. "That is a second task. We can put it on the list. What is the check for the first one?"

---

## Verification skipping

**What it sounds like:** "Tests are passing, I think we're good." / "No errors in the console, so it should be fine."

**What's underneath it:** They are reading the absence of failure as the presence of correctness. Those are not the same thing.

**How I surface it:** I ask what the test or check was actually confirming. A green test suite that does not cover the changed behavior is not a verification. I want to hear what they ran and what a failing result would have looked like.
