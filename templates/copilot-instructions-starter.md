---
# Project Copilot Instructions

> Copy this file to `.github/copilot-instructions.md` in your project.
> Remove the sections that don't apply, fill in the ones that do.
> The more specific you are, the more reliably the AI follows project conventions.

---

## Rule 0: Challenge & Verify (🔴 Mandatory)

User statements may be incorrect. When a user's claim conflicts with file contents,
documented decisions, or technical facts, **explicitly challenge it with a reason**:

> "Your description conflicts with [file/document] because [reason]. The actual state is X."

Never proceed on a wrong premise to avoid friction. Read: [framework/06_CHALLENGE_AND_VERIFY.md]

---

## Rule 1: Protected Directories (🔴 Mandatory)

The following directories must **never be deleted, emptied, or overwritten** without explicit user confirmation:

| Directory | Contents | Risk |
|-----------|----------|------|
| `[fill in]` | [description] | [consequence] |

---

## Rule 2: Project Map (Read Before Acting)

> This section provides navigation coordinates only — not content substitutes.
> Always read the actual file; this table may be outdated.

| Directory | Responsibility | Entry Point |
|-----------|---------------|-------------|
| `[module/]` | [what it does] | `[key file]` |

**Default**: If the user doesn't specify a target, assume `[main module]`.

---

## Rule 3: Keyword Triggers → Read Before Acting (🔴 Mandatory)

When any of these topics appear in the user's message, **read the specified file before taking any action**:

| Keyword | Must read | Cost of skipping |
|---------|-----------|-----------------|
| [overall plan / roadmap / next phase] | `ROADMAP.md` | Phase dependencies exist; skipping causes broken acceptance criteria |
| [new module / new script / implement X] | `docs/EXISTING_SYSTEMS_INDEX.md` | Functionality may already exist |
| [topic specific to your project] | `[docs/RELEVANT_FILE.md]` | [specific consequence] |

---

## Rule 4: Document Taxonomy (🔴 Mandatory)

Three types, three locations, no mixing:

| Type | Definition | Location |
|------|------------|----------|
| TYPE-A | Long-lived architecture, API specs, usage guides | `docs/` |
| TYPE-B | Module-specific design, evolves with code | Module directory |
| TYPE-C | Phase reports, debug logs, one-time analyses | `docs/archive/` |

Before creating any document: determine its type and announce the placement.

Read: [framework/02_DOCUMENT_TAXONOMY.md]

---

## Rule 5: Four-Layer State Architecture (🔴 Mandatory)

State lives in four layers with different time horizons:

```
ROADMAP.md          ← Strategic (6-12 months)
session_state.md    ← Tactical (current phase, 2-4 weeks)
Footer 📍           ← Operational (live, each reply)
archive/            ← Historical (permanent)
```

**New session startup protocol:**
1. `read_file: ROADMAP.md` — find current Phase
2. `read_file: session_state.md` — load working context
3. Reconstruct footer — confirm alignment with user

Read: [framework/01_FOUR_LAYER_STATE.md]

---

## Rule 6: Completion Checkpoint (🔴 Mandatory)

When a sub-task is confirmed complete (user says "works", "OK", test passes):

**Before discussing the next task**, execute all 3:
1. `ROADMAP.md` — mark sub-task ✅, check acceptance criteria checkbox
2. `session_state.md` — move sub-task to "Completed This Phase"
3. Footer — update current step and next

Read: [framework/03_COMPLETION_CHECKPOINT.md]

---

## Rule 7: Anti-Duplication (🔴 Mandatory)

Before implementing any new functionality:
1. Check `docs/EXISTING_SYSTEMS_INDEX.md`
2. If found → reuse or extend (don't create parallel implementation)
3. If replacing → document the decision in session_state Phase Decisions table

Read: [framework/04_KEYWORD_TRIGGERS.md] (Existing Systems Index section)

---

## Rule 8: Reply Footer (🔴 Mandatory — Every Reply)

Every reply in a multi-step task must end with:

```
📍 **[Current Task]** | In progress: [step] | Next: [action] | Pending: [if any]
🗺️ Phase N/T [Phase Name] | A(✅) B(🔄) C(○)
   ★ [Step A](✅) → 【Step B(current)】 → [Step C]
```

When idle: `📍 **Idle** | No active tasks`

---

## Rule 9: Language

[Fill in your project's language requirements]

---

## Project-Specific Rules

> Add rules here that are specific to your tech stack, team conventions, or environment.
> Examples: deployment constraints, API versioning rules, code style requirements.
