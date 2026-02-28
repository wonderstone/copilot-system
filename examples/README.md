# Examples

This directory contains real-world applications of the copilot-system framework.

Each example is a **stripped version** of a real project's `copilot-instructions.md` and related files,
with project-specific details generalized into patterns.

---

## Available Examples

| Example | Tech Stack | Interesting Aspects |
|---------|-----------|---------------------|
| [unity-game-client/](unity-game-client/) | C# / Unity | Long-running client with protected asset directories, Editor menu rules, HITL tuning tools |
| [python-ai-service/](python-ai-service/) | Python / FastAPI | Multi-service architecture, LLM abstraction rules, Windows GPU deployment notes |

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
