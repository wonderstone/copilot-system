# Examples

This directory contains real-world applications of the copilot-system framework.

Each example is a **stripped version** of a real project's `copilot-instructions.md` and related files,
with project-specific details generalized into patterns.

---

## Available Examples

No examples have been contributed yet. See below for how to contribute one.

**Planned examples** (in progress):

| Example | Tech Stack | Status |
|---------|-----------|--------|
| unity-game-client | C# / Unity / LLM integration | Generalization in progress |
| python-ai-service | Python / FastAPI / LangGraph | Generalization in progress |

---

## What Each Example Includes

Each example directory contains:

```
example-name/
├── copilot-instructions.md   ← The full instructions file (generalized)
├── ROADMAP.md                ← Example roadmap with completed Phases
├── session_state.md          ← Example session state (mid-Phase)
└── README.md                 ← Notes on what makes this example interesting
```

---

## Contributing an Example

If you've applied this framework on a real project and want to contribute:

1. Fork and copy your `copilot-instructions.md` into `examples/your-project-name/`
2. Replace private paths, credentials, and names with generic placeholders
3. Add a `README.md` explaining what's interesting about your project's rules
4. Open a PR — examples are the easiest contribution; no framework changes needed

**What makes a good example:**
- Non-trivial keyword trigger table (3+ entries with real consequences)
- Completed Phases (shows the archive/graduation cycle in action)
- Project-specific rules beyond the template defaults
