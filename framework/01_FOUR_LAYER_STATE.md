# Framework 01 — Four-Layer State Architecture

> **The core problem**: AI assistants have no persistent memory across conversations.  
> Without a deliberate state system, every new session starts blind — re-discovering context, repeating mistakes, losing progress.

---

## The Architecture

```
Layer 1 — Strategic    ROADMAP.md          ← 6-12 month horizon
               ↓ (Phase start: copy goals)
Layer 2 — Tactical     session_state.md    ← Current Phase (2-4 weeks)
               ↓ (Every reply: live snapshot)
Layer 3 — Operational  Footer 📍           ← Minute-by-minute status
               ↑ (Phase complete: archive)
Layer 4 — Historical   archive/            ← Permanent record
```

Each layer has a different **time horizon**, **update frequency**, and **audience**:

| Layer | File | Time Horizon | Updated By | Read By |
|-------|------|-------------|------------|---------|
| Strategic | `ROADMAP.md` | 6-12 months | Human (monthly) | AI at session start |
| Tactical | `session_state.md` | 2-4 weeks | AI (each reply) | AI at session start |
| Operational | Reply footer `📍` | Minutes | AI (each reply) | Human (live) |
| Historical | `archive/*.md` | Permanent | AI (Phase end) | Future reference |

---

## Layer 1: ROADMAP.md

The single source of truth for **where the project is going**.

**Required sections:**
```markdown
## Phase Overview Table
| Phase | Name | Track | Status | Est. |
|-------|------|-------|--------|------|
| 0     | ... | ...   | ✅     | —    |
| 1     | ... | ...   | 🔄     | 3w   |
| 2     | ... | ...   | ○      | 2w   |

## Phase N — [Name]
**Goal**: One sentence.
**Sub-tasks**:
| Step | Content | Status |
|------|---------|--------|
| NA   | ...     | ○      |
**Acceptance Criteria**:
- [ ] Condition 1
- [ ] Condition 2
**Depends on**: Phase X ✅
```

**Rules:**
- Sub-task table **must have a Status column** — without it, Completion Checkpoints cannot trigger
- Acceptance criteria use `- [ ]` / `- [x]` markdown checkboxes
- Phase status: `✅` complete · `🔄` in progress · `○` not started

---

## Layer 2: session_state.md

The AI's working memory for the **current Phase only**. One file, always in the project root.

**Required format:**
```markdown
---
updated: YYYY-MM-DD
phase: N
phase_name: [Name]
---

## Current Goal
[One sentence: what this Phase must achieve]

## In Progress
[What is happening right now, at what step]

## Completed This Phase
- ✅ Sub-task A
- ✅ Sub-task B

## Blocked / Pending
[What must happen before work can continue]

## Phase Decisions (≤10, archive on Phase complete)
| # | Decision | Reason | Date |
|---|----------|--------|------|

## Acceptance Criteria (copied from ROADMAP)
- [ ] Condition 1
- [ ] Condition 2
```

**Hard rules:**
- If file exceeds ~100 lines → information wasn't archived on time → clean immediately
- `Phase Decisions` table clears on Phase completion (content moves to archive)
- `Acceptance Criteria` section is a direct copy from ROADMAP — no paraphrasing

---

## Layer 3: Operational Footer

Every AI reply in a multi-step task **must end** with a status footer. This keeps humans oriented without reading files.

**Format:**
```
📍 **[Current Task]** | In progress: [step] | Next: [action] | Pending: [if any]
🗺️ Phase N/T [Phase Name] | A(✅) B(🔄) C(○) D(○)
   ★ [Step A](✅) → 【Current Step B】 → [Step C] → [Step D]
```

**Rules:**
- Footer is the **visible mirror** of session_state.md — they must be consistent
- Phase progress symbols shown inline: `✅` done · `🔄` current · `○` not started
- Current phase steps come from ROADMAP sub-task table — not invented ad hoc

---

## Layer 4: Historical Archive

When a Phase completes (all acceptance criteria `✅`), before moving on:

```
archive/Phase_N_<Name>_YYYY-MM-DD.md
```

**Archive document contains:**
- What was completed
- Key decisions (lifted from session_state Phase Decisions table)
- Technical notes with long-term value
- Lessons learned

**After archiving:**
1. Promote long-term technical insights → update TYPE-A docs
2. Clear session_state Phase-specific content
3. Mark Phase `✅` + date in ROADMAP
4. Load next Phase goals into session_state

---

## Information Flow Diagram

```
New session starts
       ↓
Read ROADMAP.md → confirm current Phase
       ↓
Read session_state.md → load working context
       ↓
Reconstruct footer → show to human
       ↓
Work on sub-tasks...
       ↓ (each sub-task completes)
Update ROADMAP + session_state + footer immediately
       ↓ (all acceptance criteria ✅)
Archive → rotate session_state → mark ROADMAP ✅
```

---

## Why This Works

The fundamental insight is that **AI context windows are ephemeral, but files are persistent**. By externalizing state into a structured file hierarchy, you create a "memory prosthetic" that survives context resets, session ends, and even model switches.

The four layers prevent two failure modes:
- **Over-documenting operationally** (writing essays in session_state → noise)
- **Under-documenting strategically** (no ROADMAP → AI can't find north)

Each layer has exactly one job. That separation is what makes the system scale.
