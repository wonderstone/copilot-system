# Framework 04 — Keyword Trigger Pattern

> **The core problem**: AI assistants reason from training data by default.  
> On a specific project with specific conventions, that's wrong — the conventions live in project files, not in the model.  
> The result: AI confidently implements things in ways that contradict the project's actual design.

---

## The Pattern

Define a table in `copilot-instructions.md` that maps **topics** to **mandatory reads**:

```markdown
| Keyword | Must read_file | Consequence of skipping |
|---------|---------------|------------------------|
| [topic] | [file path]   | [what breaks]          |
```

When the AI detects a keyword in the user's message, it **reads the file before taking any action**.

The third column — consequence — is critical. It's not there for documentation. It's there to make the cost of skipping viscerally concrete.

---

## What Goes in the Trigger Table

Include a topic if **all three** are true:
1. The project has a specific convention for it
2. The AI would plausibly do something different without reading
3. Doing the wrong thing causes real cost (broken builds, data loss, wasted time)

Do **not** include:
- Topics where the convention matches general best practice (AI already does it right)
- Topics where the cost of getting it wrong is trivially reversible
- Everything (over-triggering causes the AI to read files before every action, defeating the purpose)

---

## Example Trigger Table

```markdown
| Keyword | Must read_file | Consequence of skipping |
|---------|----------------|------------------------|
| animation / controller | `docs/ANIMATION_GUIDE.md` | Parameter names have specific casing requirements; wrong names fail silently |
| database / migration | `docs/MIGRATION_POLICY.md` | Migrations are irreversible; wrong direction destroys data |
| API design / new endpoint | `docs/API_CONVENTIONS.md` | Naming deviations require coordinated client updates |
| overall plan / roadmap | `ROADMAP.md` | Phase dependencies exist; skipping phases breaks acceptance criteria |
| new script / new module | `docs/EXISTING_SYSTEMS_INDEX.md` | 80% of needed functionality already exists; re-implementing wastes days |
```

---

## File Structure for Triggers

The keyword table belongs inside `copilot-instructions.md` (the AI's system-level instruction file). See [templates/copilot-instructions-starter.md](../templates/copilot-instructions-starter.md) for placement.

Recommended section heading: `## Keyword Triggers → Read Before Acting`

---

## Existing Systems Index

One of the highest-value trigger targets is an index of what already exists in the codebase.

The pattern:
```markdown
# Existing Systems Index

## Quick Search
| If you need...           | It already exists as...     | Location |
|--------------------------|----------------------------|----------|
| HTTP client              | `ApiClient`                | `src/api/client.ts` |
| event dispatching        | `EventBus.publish<T>()`   | `src/core/events/` |
| user authentication      | `AuthService`              | `src/auth/` |
| data validation          | `Validator.check(schema)`  | `src/utils/validator.ts` |
```

The index is maintained as a TYPE-A document. Every new module added → new row. Every module removed → row deleted.

**Trigger rule**: Any phrasing of "create a new [X]" → check the index first.

---

## Anti-Duplication Rule

Paired tightly with the keyword trigger pattern:

> Before implementing new functionality, confirm nothing equivalent exists locally.

Steps:
1. Search the index (if it exists)
2. If matched → read the existing implementation's header (first 50 lines) + public API
3. Decision:
   - Fully covers the need → **reuse directly**
   - Partially covers → **extend the existing implementation**
   - Interface conflict or fundamentally different purpose → **replace, but record the decision in session_state**

Replacement decisions must be documented:
```markdown
| # | Decision | Reason | Date |
|---|----------|--------|------|
| N | Replaced OldModule with NewModule | OldModule doesn't support async; rewriting would take longer than new | YYYY-MM-DD |
```

---

## Why Triggers Beat Prompting

The alternative is to write the conventions everywhere and hope the AI absorbs them. This doesn't work because:

1. Long context windows don't mean uniform attention — content at the end of `copilot-instructions.md` gets less weight
2. The AI doesn't know what it doesn't know — it can't flag "I should probably check if there's a convention for this"
3. Conventions drift — the best documentation is the code itself, but the AI can't read the whole codebase every turn

Keyword triggers solve this by creating **explicit read obligations** tied to specific words. The AI doesn't need to know the convention in advance; it just needs to read before acting when the topic comes up.
