# Framework 05 — Cross-Project Protocol Pattern

> **The core problem**: Two teams build separate systems that must communicate.  
> Each team implements their side based on verbal agreements or meeting notes.  
> Integration day: the formats don't match, the field names are different, one side uses snake_case and the other camelCase.

---

## The Pattern: A Shared Neutral Directory

Create a directory **outside both project repositories** that both teams treat as a contract layer:

```
shared-protocols/
├── README.md           ← Collaboration rules (this pattern)
├── protocols/          ← Confirmed interfaces (change-controlled)
├── proposals/          ← Draft proposals (free to edit)
└── decisions/          ← Cross-team decision log
```

Both teams' AI assistants are configured to **read from this directory** when working on integration topics.

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
