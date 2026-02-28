# [Project Name] — ROADMAP

> TYPE-A document — project global plan, updated monthly.
> Last updated: YYYY-MM-DD

---

## Vision

[One paragraph: what does this project look like when fully realized?]

---

## Capability Tracks (Optional)

If your project has multiple independent dimensions of progress, name them here.
Each Phase can advance one or more tracks simultaneously.

| Track | Name | Scope |
|-------|------|-------|
| 🏗️ Core | Foundation | [what core infrastructure covers] |
| 🎨 UX | User Experience | [what UX covers] |
| 🤖 AI | Intelligence | [what AI integration covers] |

---

## Phase Overview

| Phase | Name | Track | Status | Est. |
|-------|------|-------|--------|------|
| 0 | [Foundation] | 🏗️ | ✅ | — |
| 1 | [Next milestone] | 🏗️ | 🔄 | 2-3 weeks |
| 2 | [Planned] | 🎨 | ○ | 2-3 weeks |
| 3 | [Planned] | 🤖 | ○ | 3-4 weeks |

Status: `✅` complete · `🔄` in progress · `○` not started

---

## Phase 0 — [Foundation] ✅

**Completion date**: YYYY-MM-DD

**Outcomes**:
- [What was built]
- [Key decisions made]
- [Technical foundation established]

---

## Phase 1 — [Name] 🔄

**Goal**: [One sentence: what must be true when this Phase is done?]

**Track**: [Which capability track]

**Sub-tasks**:

| Step | Content | Status |
|------|---------|--------|
| 1A | [Specific deliverable] | 🔄 |
| 1B | [Specific deliverable] | ○ |
| 1C | [Specific deliverable] | ○ |
| 1D | [Validation / acceptance test] | ○ |

**Acceptance Criteria**:
- [ ] [Measurable condition 1]
- [ ] [Measurable condition 2]
- [ ] [End-to-end behavior that proves the Phase goal is met]

**Depends on**: Phase 0 ✅

---

## Phase 2 — [Name] ○

**Goal**: [One sentence]

**Track**: [Track]

**Sub-tasks**:

| Step | Content | Status |
|------|---------|--------|
| 2A | [Deliverable] | ○ |
| 2B | [Deliverable] | ○ |

**Acceptance Criteria**:
- [ ] [Condition]
- [ ] [Condition]

**Depends on**: Phase 1

---

## Dependency Graph

```
Phase 0 ✅
  └─→ Phase 1 🔄
        ├─→ Phase 2
        └─→ Phase 3
```

---

## Maintenance Rules

- AI updates status markers (`○` → `✅`) at Completion Checkpoints
- Acceptance criteria checked on Phase completion (Graduation Protocol)
- Each completed Phase gets an archive entry: `docs/archive/Phase_N_<Name>_YYYY-MM-DD.md`
- New Phases appended at the end — existing Phase numbers never change

---

*Related: `session_state.md` (current Phase detail) · `.github/copilot-instructions.md` (Rule 5-6)*
