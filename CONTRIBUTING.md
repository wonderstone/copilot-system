# Contributing

## Two kinds of contributors

---

### AI assistants on a project

You don't need a formal process. If you think a rule should change:

1. **Say so during the conversation** — explain what you'd change and why
2. **The human decides** — if they agree, update the file immediately in that session
3. **That's it** — the change takes effect for all future sessions

No proposals, no PRs, no waiting. The human is the gatekeeper.

---

### Human developers improving the public framework

If you've discovered a pattern that works well — or found that one of the existing patterns doesn't work as described:

1. **Fork this repo**
2. **Edit the relevant `framework/` file** directly — no separate proposal docs needed
3. **Open a PR** with a concrete example of the problem and how your change addresses it

Criteria for new patterns:
- Solves a real problem that none of the existing 6 patterns cover
- Generalizable — applies beyond one specific tech stack or project type
- Concise — if it takes more than 2 pages to explain, it probably needs to be split or isn't ready

Criteria for modifying existing patterns:
- Identify a specific situation where the pattern fails
- The proposed fix doesn't break the pattern's core intent

The framework prioritizes a small set that's actually followed over a comprehensive set that's ignored. New patterns raise the adoption cost for everyone.

---

### What belongs in `framework/` vs `examples/` vs your own project

Use this test:

**Ask: "Would this rule apply to a project in a completely different tech stack?"**

| Answer | Where it belongs |
|--------|-----------------|
| Yes, it's about how AI and humans coordinate — not about any specific tool | `framework/` |
| Yes, but only for projects of a certain type (game clients, microservices, etc.) | `examples/<type>/` |
| No, it's about our specific tools, paths, or conventions | Your project's own `copilot-instructions.md` — nowhere else |

**Concrete examples:**

| Rule | Correct location | Why |
|------|-----------------|-----|
| "Update session_state after every completed sub-task" | `framework/` | True for any project with any AI |
| "Read ROADMAP before starting any Phase work" | `framework/` | True regardless of tech stack |
| "TYPE-C documents go in `docs/archive/`" | `framework/` | Universal document hygiene |
| "Before implementing X, check if it already exists in the codebase" | `framework/` | Universal anti-duplication |
| "Unity `.meta` files must not be deleted via terminal" | `examples/unity-*/` | Unity-specific |
| "All LLM calls must go through `LLMFactory`" | `examples/python-ai-service/` | Project architecture-specific |
| "Our gateway runs on port 6003" | Your project only | Specific to one deployment |
| "DAZ lossyScale is 100, not 1" | Your project only | Specific to one toolchain |

**The boundary in plain language:**

- `framework/` = the *principle* ("don't re-implement what exists")
- `examples/` = the *principle applied to a specific stack* ("in Unity, check `EXISTING_SYSTEMS_INDEX.md` before creating any `.cs` file")
- Your project = the *principle applied to your specific codebase* ("before creating any `.cs` file, check `Docs/EXISTING_SYSTEMS_INDEX.md` — 92 scripts already exist")
