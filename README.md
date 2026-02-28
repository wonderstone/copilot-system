# copilot-system

**A structured operating system for AI-assisted software projects.**

Designed for projects that use GitHub Copilot, Claude, or similar AI assistants, where the development spans weeks or months across many conversations.

---

## The Problem

AI assistants are powerful, but they have no persistent memory. Every new conversation starts from zero:

- The AI re-discovers context it already found last week
- Completed tasks get redone because state wasn't written down
- Conventions get violated because they live in someone's head, not in files
- Two sessions from now, nobody remembers *why* that architectural decision was made

The longer the project, the worse this gets.

---

## The Solution

Externalize everything the AI needs to do its job into a small set of structured files. Make reading those files **mandatory and automatic** — not aspirational.

This repo contains:

1. **Framework** — Seven universal patterns distilled from long-running real projects
2. **Templates** — Copy-paste starting points for every required file
3. **Examples** — Real project applications of the framework (generalized)

---

## The Seven Patterns

| # | Pattern | What It Solves |
|---|---------|----------------|
| [01](framework/01_FOUR_LAYER_STATE.md) | **Four-Layer State Architecture** | Context loss between sessions |
| [02](framework/02_DOCUMENT_TAXONOMY.md) | **Document Taxonomy & Directory Hygiene** | Decaying documentation trust |
| [03](framework/03_COMPLETION_CHECKPOINT.md) | **Completion Checkpoint** | Completed work not recorded |
| [04](framework/04_KEYWORD_TRIGGERS.md) | **Keyword Trigger Pattern** | AI ignoring project conventions |
| [05](framework/05_CROSS_PROJECT_PROTOCOL.md) | **Cross-Project Protocol Pattern** | Integration mismatches between teams |
| [06](framework/06_CHALLENGE_AND_VERIFY.md) | **Challenge & Verify Pattern** | Silent errors from unchallenged wrong premises |
| [07](framework/07_OBSERVABILITY_FIRST_DEBUG.md) | **Observability-First Debug Loop** | Speculative fixes that compound bugs |

These patterns are **independent** — adopt any subset. They compose well, but none requires the others.

---

## Quick Start

**Minimum viable setup (30 minutes):**

```
your-project/
├── .github/
│   └── copilot-instructions.md    ← Copy from templates/, fill in your specifics
├── ROADMAP.md                      ← Copy from templates/, define your Phases
└── session_state.md                ← Copy from templates/, fill in current state
```

1. Copy [templates/copilot-instructions-starter.md](templates/copilot-instructions-starter.md) → `.github/copilot-instructions.md`
2. Copy [templates/ROADMAP-starter.md](templates/ROADMAP-starter.md) → `ROADMAP.md`
3. Copy [templates/session_state-starter.md](templates/session_state-starter.md) → `session_state.md`
4. Fill in the `[placeholder]` sections with your project specifics
5. Open a new conversation with your AI assistant. It will read these files automatically.

**Full setup adds:**
- `docs/EXISTING_SYSTEMS_INDEX.md` —Inventories your codebase for the anti-duplication rule
- Keyword trigger entries for your specific modules and conventions
- `docs/archive/` directory for the document lifecycle system

---

## Design Principles

**Files over memory.** Everything the AI needs to function on your project should exist in files, not in conversation history that disappears.

**Enforcement over aspiration.** A rule that says "you should read this file" is ignored. A rule that says "when you see keyword X, you must read file Y before taking any action" is followed.

**Explicit over implicit.** If the AI should know something, write it down. If you want a certain behavior, specify the trigger condition. Assuming the AI "knows" your project conventions is how conventions get violated.

**State is structured, not narrative.** `session_state.md` is not a log. It's a structured document with specific fields that the AI reads in a specific sequence. Narrative updates ("we made some progress on X") don't survive context resets.

---

## Adopted From

These patterns were extracted from active development of two projects:
- A Unity desktop pet application (C# / Unity, long-running, multi-phase)
- A Python AI service platform (Python / FastAPI / LangGraph, multi-component)

Both projects use these patterns in production AI-assisted development sessions. The framework here is the **generalized extraction** — all project-specific details have been replaced with abstract templates.

---

## Contributing

- **Examples** are the easiest contribution: generalize your own project's instructions and add them to `examples/`
- **Framework corrections**: if you find a pattern that doesn't work as described, open an issue with a concrete counter-example
- **New patterns**: open an issue first — new patterns should solve a real problem that isn't already covered

The framework is intentionally conservative about adding new patterns. Every pattern added increases the adoption cost. A smaller set that's actually followed beats a comprehensive set that's ignored.

---

## License

MIT. Use freely, including in commercial projects.
