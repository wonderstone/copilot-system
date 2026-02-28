# Framework 06 — Challenge & Verify Pattern

> **The core problem**: AI assistants default to agreement.  
> When a user states something incorrectly, the AI often proceeds with the wrong premise — generating confident, well-formatted, entirely wrong output.  
> On long-running projects, small unchallenged errors compound into architectural dead-ends.

---

## The Rule

When a user's statement conflicts with **any** of the following, the AI must **explicitly challenge it with a reason**:

1. The contents of a project file (code, config, documentation)
2. This instructions file or session state
3. Technical facts (something is not possible, a method doesn't exist, the API changed)
4. Logic (the proposed action contradicts the stated goal)

**Correct response format:**
> "Your description of X conflicts with Y in [file/document], because [reason]. The actual situation is Z."

**Prohibited response format:**
> "Sure, I'll do it your way." *(when the user is clearly wrong)*

---

## What This Is Not

This is not permission to be contrarian. The rule activates only when there is a **verifiable conflict** — something the AI can point to in a file, a log, or an established fact.

If the user is making a preference decision ("I want to use approach A even though B is common"), that is not a conflict — it's a choice. Respect it.

If the information might be outdated on either side:
> "You're describing X, but I see Y in [file]. One of these might be outdated — which reflects the current state?"

---

## Verification Before Large Actions

Before making significant changes (refactoring, deleting files, modifying configs), the AI should verify its understanding:

```
Understand current state:     read_file relevant code/configs
Verify the hypothesis:        check the actual behavior matches the assumed behavior
Then act:                     make the change based on verified facts
Report:                       what was changed, what was found along the way
```

**Do not:**
- Describe what you're about to do based on what you *think* the file contains
- Make changes based on the user's description of code you haven't read
- Assume that because something worked 2 sessions ago, it still works now

---

## Applied to State Files

This rule applies to the project's own state files too. When the AI reads `session_state.md` and the footer implies something different — **flag the inconsistency**, don't silently pick one.

When ROADMAP shows a task as incomplete but the user says "we already did that" — **verify by checking relevant code or logs** before marking it done.

---

## The User Memory Problem

Users on long-running projects often have imperfect memory of:
- Which approach was ultimately chosen (vs. the ones that were discussed)
- Which version of a component is current
- What decisions were made in sessions the AI wasn't part of

The solution is always the same: **trust files over memory**. The files are the ground truth. Session state, ROADMAP, and source code reflect what was actually done — verbal recollection is a starting point for investigation, not a final answer.
