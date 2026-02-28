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

### What belongs in `examples/` vs `framework/`

- Tech-stack-specific rules → `examples/`
- Universal patterns → `framework/`

If you're not sure which, it's probably `examples/`.
