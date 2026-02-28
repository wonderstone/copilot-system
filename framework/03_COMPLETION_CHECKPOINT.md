# Framework 03 — Completion Checkpoint

> **The core problem**: AI completes a sub-task, then immediately moves on.  
> The state files are never updated. Three sessions later, the AI re-discovers what was already done — or worse, tries to do it again.

---

## The Mechanism

**Trigger**: A sub-task is verified as complete.  
Verification means any of: user confirms ("looks good", "it works"), test passes, log shows success.

**What must happen before discussing the next task** (all 3 steps, in order):

```
Step 1: ROADMAP.md
        Sub-task row: ○ → ✅
        Acceptance criteria: - [ ] → - [x] + completion date

Step 2: session_state.md
        Move sub-task from "In Progress" → "Completed This Phase"
        Update "Acceptance Criteria" checkboxes to match ROADMAP

Step 3: Footer 📍
        Update current step indicator
        Set "Next:" to the following sub-task
```

**Then**: confirm completion in the reply, then discuss next steps.

---

## The Rule in Plain Language

> When the user says the thing works, update the files first, then continue.

Not after. Not "I'll update at the end of the session." Now.

---

## Violation Detection

These are signs the Checkpoint was skipped:

| Signal | What it means |
|--------|--------------|
| Footer shows ✅ but ROADMAP still shows ○ | Files weren't updated |
| session_state "In Progress" has tasks the user already confirmed | Stale state |
| AI says "let me re-check if X was done" | State was never written |
| User says "we already did that" | AI lost track of completed work |

If any of these are detected mid-session: stop, update all three files, then continue.

---

## User Confirmation Language

These phrases trigger a Completion Checkpoint:

| User says | Means |
|-----------|-------|
| "looks good", "it works", "OK" | Sub-task verified ✅ |
| "ready to move on", "next" | Sub-task verified ✅ |
| "that's wrong" / "doesn't work" | Sub-task NOT verified — investigate |
| "close enough for now" | Partial — mark as 🔄, note what remains |

When ambiguous, ask explicitly: "Should I mark [sub-task name] as complete?"

---

## Phase Graduation

When **all** acceptance criteria in a Phase are `- [x]`:

```
1. Create archive/Phase_N_<Name>_YYYY-MM-DD.md
   → Completed items + key decisions + technical notes

2. Promote durable insights
   → Long-term findings → update relevant TYPE-A documents
   → Cross-cutting rules → update copilot-instructions

3. Rotate session_state
   → Clear Phase-specific content
   → Copy next Phase goals + acceptance criteria from ROADMAP

4. Mark ROADMAP
   → Phase row: 🔄 → ✅ + date
   → session_state: update phase number and phase_name
```

---

## Why Forced Checkpoints Work

Without enforcement, state updates happen "when there's time" — which means almost never. The discipline of updating **immediately on confirmation** creates a self-correcting system:

- If the AI skips the update, the next session catches it immediately (footer doesn't match ROADMAP)
- If the user skips confirmation, the AI explicitly asks before moving on
- The 3-file update is atomic: all three or none — partial updates cause worse confusion than no update

The friction of the 3-step update is also intentional: it creates a natural pause between sub-tasks, reducing the "treadmill" pattern where AI and human race through tasks without consolidating what was learned.
