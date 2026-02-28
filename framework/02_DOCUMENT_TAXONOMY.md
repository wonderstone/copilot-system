# Framework 02 — Document Taxonomy & Directory Hygiene

> **The core problem**: Projects accumulate documents with no sense of "is this still true?"  
> A design doc from last month and a one-time debug log both end up in the same folder, with the same visual weight.  
> The result: nobody trusts any document, so nobody reads them.

---

## The Three Document Types

Every document in a project is exactly one of:

| Type | Definition | Location | Lifespan |
|------|------------|----------|----------|
| **TYPE-A** | Long-lived. Architecture, API specs, usage guides, decision references. Someone will consult this 6 months from now. | `docs/` root or `module/README.md` | Indefinite — keep current |
| **TYPE-B** | Module-specific design notes. Evolves with the code it describes. | Next to the code it describes | As long as the module lives |
| **TYPE-C** | Process artifacts. Phase reports, implementation summaries, one-time analyses, debug logs. True when written, probably stale later. | `docs/archive/` | 3 months after creation, review for deletion |

---

## Decision Flow

```
Need to create a document?
  │
  └─ Will someone need this in 6 months?
        ├─ YES → Is it cross-module or project-wide?
        │          ├─ YES → docs/TYPE-A.md
        │          └─ NO  → module/README.md or module/TYPE-A.md  (TYPE-B)
        └─ NO  → docs/archive/TYPE-C_YYYY-MM-DD.md
```

---

## Naming Signals

Certain filename patterns are **automatic TYPE-C**. Never place these in the `docs/` root:

| Pattern | Type | Correct Location |
|---------|------|-----------------|
| `*_Summary.md`, `*_Report.md` | TYPE-C | `docs/archive/` |
| `*_Complete.md`, `*_Ready.md` | TYPE-C | `docs/archive/` |
| `*_Plan.md`, `*_Fix.md` | TYPE-C | `docs/archive/` |
| `TEMP_*`, `*_TODO.md` | TYPE-C | `docs/archive/` |
| `*_GUIDE.md`, `*_Reference.md` | TYPE-A | `docs/` root |
| `*_ARCHITECTURE.md`, `*_DESIGN.md` | TYPE-A | `docs/` or module root |

---

## The Index Contract

Every `docs/` directory must have an `INDEX.md` (or `README.md`):

```markdown
# Documentation Index

> Only TYPE-A documents listed here. Process documents in `archive/`.

| Document | Description |
|----------|-------------|
| [ARCHITECTURE.md](ARCHITECTURE.md) | System design overview |
| [API_REFERENCE.md](API_REFERENCE.md) | Public API specification |
```

**Rules:**
- Every new TYPE-A document → add a row to INDEX.md
- Every deleted TYPE-A document → remove the row
- INDEX.md itself is TYPE-A (never archive it)

---

## Directory Hygiene Rules

### Project Root (`/`)

Only these items belong at the project root:

| Allowed | Examples |
|---------|---------|
| Runtime config | `package.json`, `pyproject.toml`, `.env.example` |
| Entry points | `main.py`, `index.ts`, `App.cs` |
| Build/run scripts | `Makefile`, `start.sh` |
| Standard metadata | `README.md`, `LICENSE`, `.gitignore` |
| State file | `session_state.md` (one, always here) |
| Standard tool dirs | `.github/`, `.vscode/`, `node_modules/` |

**Never at root:**
- Phase summaries, debug logs, one-time scripts
- Exported build artifacts
- Crash dumps

### Module Directories

Each module owns its own documentation. A module directory typically looks like:

```
module/
├── README.md          ← TYPE-B: what this module does and how to use it
├── src/               ← Code
├── tests/             ← Tests
└── docs/archive/      ← (optional) module-specific TYPE-C process docs
```

---

## AI Behavior Rules

When creating a document, AI must:
1. Determine the type using the decision flow above
2. State the type and location **before** creating: "This is TYPE-C, going to `docs/archive/`"
3. Add the entry to INDEX.md if TYPE-A

When reading documents, AI must:
1. Start from INDEX.md, not by listing the directory
2. If a document is found in the wrong location → flag it and suggest moving

When a document is over 3 months old and TYPE-C:
- If content is superseded → delete
- If content has lasting value → promote to TYPE-A in the right location

---

## Why This Matters for AI Assistants

AI assistants cannot "scan" a docs folder and know which information is current. They read what they're pointed at. A document named `DESIGN.md` from 2 years ago gets the same weight as one from yesterday.

The taxonomy solves this by making **staleness visible through location**: if it's in `docs/archive/`, the AI knows to treat it as historical context, not current truth. If it's in `docs/` root, it's been deliberately maintained.

This is especially critical for long-running projects where the AI accumulates references to old documents across many sessions.
