# Framework 05 — Cross-Project Protocol Pattern

> **The core problem**: Two systems that must communicate are built independently.  
> Each side implements based on memory, chat logs, or verbal agreement.  
> Integration day: field names don't match, one side uses snake_case, the other camelCase, one side added a required field the other doesn't send.
>
> This problem is **not about team size**. A single person managing two projects is just as exposed as a 10-person team — more so, because the false confidence of "I know both sides" delays the moment of discovery.

---

## The One Non-Negotiable Rule

> Interface documents live in a neutral location that both projects can read.  
> Changes to a confirmed interface require both sides to update before the old behavior is removed.

Everything else in this pattern is implementation detail that scales with team size and project complexity.

---

## Choose Your Scale

### Minimal (1 person, 2 projects, same machine)

```
~/shared-protocols/
└── DESKTOP_PET_PROTOCOL.md    ← Single file, both projects reference it
```

- No proposals/ or protocols/ split — one file per interface
- No SemVer — use `-draft` / `-confirmed` suffixes in the filename or document header
- Change process: edit the file, make sure you've updated both implementations before the session ends
- Copilot instructions on both sides: add a keyword trigger pointing to this file

**When to move up:** when you find yourself unsure which version of the interface each side has actually implemented.

---

### Standard (2+ people, or 1 person with async work across sessions)

```
shared-protocols/
├── README.md           ← Collaboration rules + document registry
├── protocols/          ← Confirmed interfaces (change-controlled)
├── proposals/          ← Draft proposals (free to edit unilaterally)
└── decisions/          ← Cross-team decision log
```

- Proposals move to protocols/ after both sides confirm
- SemVer on confirmed documents (MAJOR for breaking changes)
- YAML front matter in each file: `from`, `to`, `status`, `version`

**When to move up:** when you need a PR trail or when multiple people might edit the same file.

---

### Full (distributed teams, public APIs, formal change control)

- Git repository for shared-protocols/ (not just a local directory)
- PRs required to merge into protocols/
- Changelog maintained per document
- Old MAJOR versions retained during migration window

---

## The Pattern in Full (Standard scale)

Both sides' AI assistants are configured to **read from the protocols/ directory** when working on integration topics.

---

## Document Lifecycle

```
proposals/draft.md
    │
    ├─ One team writes a proposal
    ├─ Other team reads, comments, proposes changes
    ├─ Both agree on the contents
    │
    └─→ protocols/confirmed.md    ← version number, -draft removed
              │
              └─ LOCKED: changes require new proposal + re-confirmation
```

| Directory | Who can edit | When | Versioning |
|-----------|-------------|------|-----------|
| `proposals/` | Either team | Anytime | No strict versioning |
| `protocols/` | Both teams, after confirmation | After mutual agreement | SemVer required |
| `decisions/` | Either team | To record cross-team decisions | Date-stamped |

---

## Versioning Convention

Protocol documents use Semantic Versioning:

| Change type | Version bump | Example |
|-------------|-------------|---------|
| Breaking change (field removed, format changed) | MAJOR | `1.0.0 → 2.0.0` |
| Backward-compatible addition (new optional field) | MINOR | `1.0.0 → 1.1.0` |
| Documentation correction only | PATCH | `1.0.0 → 1.0.1` |
| Not yet confirmed | `-draft` suffix | `1.1.0-draft` |

---

## File Routing Metadata

Each document in `protocols/` or `proposals/` must include a YAML front matter header:

```markdown
---
from: TeamA         # Who authored this document
to: TeamB           # Who needs to read and implement
status: draft | proposal | confirmed
version: x.y.z
created: YYYY-MM-DD
---
```

The `README.md` maintains a table:

```markdown
| Document | Location | From | To | Status | Version |
|----------|----------|------|----|--------|---------|
| Service API | protocols/SERVICE_API.md | Backend | Frontend | confirmed | v2.1.0 |
| New Feature Proposal | proposals/FEATURE_X.md | Frontend | Backend | proposal | — |
```

---

## Integration with copilot-instructions

Each team adds a trigger rule to their `copilot-instructions.md`:

**Team A's instructions:**
```markdown
| cross-project / shared API / integration | read_file: /path/to/shared-protocols/protocols/*.md | Protocol has version control; implementing from memory risks version mismatch |
```

**Team B's instructions:**
```markdown
| Team A integration / shared protocol | read_file: /path/to/shared-protocols/protocols/*.md | Same |
```

**Effect**: Whenever either AI works on integration code, it reads the confirmed protocol first — not the training-data guess of what the API might look like.

---

## Change Flow After Confirmation

Once in `protocols/`, a document is considered a contract. Changes follow this flow:

```
1. Proposer creates proposals/CHANGE_FOR_V2.md
   → Describes what changes and why
   → Includes migration path for existing implementations

2. Both teams review
   → Check their own implementation for breakage
   → Propose amendments in the same file

3. Agreement reached
   → Update protocols/ document (bump version)
   → Both teams update their implementations
   → Record decision in decisions/

4. Old version retention policy
   → Keep previous MAJOR version for 1 migration cycle
   → Then delete
```

---

## Decision Log Format

```markdown
# decisions/YYYY-MM-DD_decision-name.md

**Date**: YYYY-MM-DD
**Participants**: Team A, Team B
**Topic**: [What was decided]

## Decision
[The decision made]

## Rationale
[Why this option over alternatives]

## Alternatives Considered
- Option X: rejected because...
- Option Y: rejected because...

## Implementation Impact
- Team A: [what they need to change]
- Team B: [what they need to change]
```

---

## Why Not a Shared Git Repository

A dedicated git repo is cleaner for distributed teams. A shared local directory works when:
- Both projects are on the same machine (monorepo-adjacent)
- The team is small (1-3 people per side)
- The overhead of git hosting is not worth it at this stage

The pattern is identical either way — the directory structure, lifecycle, and metadata conventions all apply. When the project grows to need a proper repo, convert the local directory to a git remote without changing the file structure.
