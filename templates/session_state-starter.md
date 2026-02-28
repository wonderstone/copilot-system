---
updated: YYYY-MM-DD
phase: N
phase_name: [Phase Name]
---

## Current Goal

[One sentence: what must be true when this Phase completes?]

## In Progress

[What the AI is currently working on, at what step. Be specific: "Implementing X in file Y, step 3 of 4."]

## Completed This Phase

- ✅ [Sub-task name] - [one-line note on what was done]
- ✅ [Sub-task name]

## Blocked / Pending

- [What external dependency is blocking progress]
- [What the human needs to confirm before the AI can proceed]

## Phase Decisions (≤10 entries, archive on Phase completion)

| # | Decision | Reason | Date |
|---|----------|--------|------|
| 1 | [What was decided] | [Why this over alternatives] | YYYY-MM-DD |

## Acceptance Criteria (copied from ROADMAP — do not paraphrase)

- [ ] [Exact text from ROADMAP]
- [ ] [Exact text from ROADMAP]
- [ ] [Exact text from ROADMAP]

---
## Notes for the AI

**On session start**: Read ROADMAP.md first to confirm the current Phase, then read this file.
If this file's phase number doesn't match ROADMAP's current Phase, flag the inconsistency.

**On session end / interruption**: Update "In Progress" to reflect exactly where work stopped.
Include enough detail for a cold-start next session to pick up without re-discovery.

**File size rule**: If this file exceeds ~100 lines, content wasn't archived at Phase completion.
Stop and archive before continuing.
