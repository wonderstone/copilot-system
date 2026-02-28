# Framework 08 — Async Thread Protocol

> **The core problem**: Two teams share a protocol or interface document and need
> to resolve open questions before implementation begins. They use one big document,
> comments accumulate in-place, status is tracked by eye-scanning, and nobody
> knows which issues are resolved vs. awaiting reply.
>
> This problem gets worse as the number of open questions grows.

---

## The One Non-Negotiable Rule

> Each open question lives in its own file.
> The file's name carries the date, sequence number, and topic.
> The file's frontmatter carries who asked, who should respond, and what the current status is.

Status never lives in prose. It lives in a structured field that tools and humans
can read without parsing the entire document.

---

## When to Use This Pattern

| Situation | Use threads? |
|-----------|-------------|
| 2+ open questions need cross-team resolution before coding | ✅ Yes |
| One quick clarification, same-day response expected | ❌ No — reply inline or in chat |
| Decision needs a permanent record that outlives the conversation | ✅ Yes |
| You're asking something that will generate a multi-paragraph response | ✅ Yes |

---

## Directory Layout

Add a `threads/` directory to whichever shared repository both teams already use
(e.g., a shared-protocols repo, a monorepo docs folder):

```
shared-repo/
  threads/
    README.md                                  ← This pattern applied to your project
    YYYY-MM-DD-NNN-{slug}.md                   ← One thread per issue
  archive/
    threads/
      YYYY-MM-DD-NNN-{slug}.md                 ← Resolved threads move here
```

`threads/README.md` doubles as a live index of all open threads (see §Index Table).

---

## File Naming Convention

```
YYYY-MM-DD-NNN-{slug}.md
```

| Field | Rule | Example |
|-------|------|---------|
| `YYYY-MM-DD` | Date the thread was opened | `2026-02-28` |
| `NNN` | 3-digit sequence within the day, never reset | `001`, `002` |
| `{slug}` | Topic summary, lowercase, hyphen-separated | `auth-token-expiry` |

**Rationale**: The filename alone tells you when it was raised, which one it was,
and what it's about — before opening the file.

---

## Frontmatter Schema

Every thread file **must** begin with this YAML block:

```yaml
---
thread_id: YYYY-MM-DD-NNN
topic: One-sentence description of the issue
from: team-a-name          # who opened the thread
to: team-b-name            # who needs to respond
type: question | decision-request | notification | approval-request
status: open | awaiting-reply | awaiting-confirm | resolved
priority: normal | high | blocking
created: YYYY-MM-DD
updated: YYYY-MM-DD        # update this every time the file is edited
---
```

### `type` Values

| Type | When to use | Expected response |
|------|-------------|-------------------|
| `question` | You have a specific question for the other team | Answer the question |
| `decision-request` | You've analyzed options, need them to pick one and confirm | Choose an option |
| `notification` | You're informing them of a decision; no reply needed | None (read and implement) |
| `approval-request` | You have a final answer, need formal APPROVED before coding | `[x]` in the Confirmation section |

### `status` Flow

```
open
  │   (thread opener writes the question/proposal)
  ▼
awaiting-reply
  │   (other team is writing their response)
  ▼
awaiting-confirm
  │   (opener has given a final answer; other team must APPROVED)
  ▼
resolved
  │   (both sides have confirmed; move to archive/)
  ▼
closed        (file moved to archive/threads/)
```

**Rule**: Every time you save a thread file, update `status` and `updated` in the frontmatter.

---

## Document Body Template

```markdown
## Background

[Why does this issue exist? Reference the section of the shared document
that's in question — e.g., "Protocol §3.2 states X, but..."]

## [Opener Team]'s Question / Proposal

[State the question clearly, or list the options for a decision-request:

**Option A**: ...
**Option B (recommended)**: ...because...]

## [Responder Team]'s Reply

> Updated: YYYY-MM-DD

[Responder fills this section. Do not edit the Opener's section above.]

## [Opener Team]'s Final Answer (if needed)

> Updated: YYYY-MM-DD

[If the reply generates further clarification, opener fills this.
If the reply already resolves it, skip this section.]

## Resolution

> Status: ⏳ Pending | ✅ Confirmed | ❌ Rejected

[One sentence: what was decided.]

## Confirmation

- [ ] [Responder Team] confirms the resolution above and will implement accordingly
```

**Rule**: Each team **only edits their own section**. The opener fills
"Opener's Question"; the responder fills "Responder's Reply". Neither
edits the other's section.

---

## Index Table in `threads/README.md`

`threads/README.md` maintains a live index of open threads:

```markdown
## Open Threads

| File | Topic | From | Status | Priority |
|------|-------|------|--------|----------|
| [YYYY-MM-DD-001-slug.md](YYYY-MM-DD-001-slug.md) | ... | team-a | awaiting-confirm | high |
```

**Rule**: Add a row when opening a thread. Remove the row (or move to an
"Archived Threads" table) when the thread reaches `resolved`.

---

## Lifecycle: Open → Resolve → Archive

### Opening a thread

1. Create `threads/YYYY-MM-DD-NNN-{slug}.md`
2. Fill frontmatter (`status: open`), write Background and your Question/Proposal
3. Update sequence (`status: awaiting-reply`) and add a row to `threads/README.md`
4. Notify the other team: "New thread waiting for you: `YYYY-MM-DD-NNN-{slug}`"

### Responding

1. Open the file, fill your team's Reply section
2. Update frontmatter: `status: awaiting-confirm`, `updated: today`
3. Notify the opener

### Confirming

1. Opener reads the reply, writes Final Answer if needed, writes the Resolution
2. Update frontmatter: `status: resolved`
3. Fill `- [x]` in the Confirmation section

### Archiving

1. Move the file to `archive/threads/`
2. Update frontmatter: `status: closed`
3. Remove the row from `threads/README.md`
4. Apply the decision to the actual shared document (protocol, spec, etc.)

---

## Integration with Shared Documents

When a thread resolves a question about a shared document (e.g., a protocol file),
add a backlink in the source document's section:

```markdown
## §3.2 Auth Token Format
...
> 📌 See [threads/2026-01-15-001-token-format.md](../threads/2026-01-15-001-token-format.md)
> for the decision trail on why HMAC-SHA256 was chosen over JWT.
```

And add a forward link in the shared document to the resolved thread:

```markdown
<!-- Resolution documented in threads/2026-01-15-001-token-format.md (closed) -->
```

---

## Anti-Patterns

| Anti-pattern | Problem | Fix |
|-------------|---------|-----|
| Raising multiple questions in one thread | Can't resolve them independently | One question = one thread |
| Editing the other team's section | Destroys the conversation record | Only edit your own section |
| Keeping `status: open` after the other side replied | False alarm on next scan | Update `status` every time you save |
| Using chat/email instead of a thread for important decisions | Decision isn't in the shared repo | If it affects the shared document, it needs a thread |
| Skipping the archive step | `threads/` fills up with resolved files | Move to `archive/threads/` on resolution |

---

## Scaling

### Solo developer, two projects

You probably don't need `type` or a Confirmation section. Use:

```yaml
---
thread_id: YYYY-MM-DD-NNN
topic: ...
status: open | resolved
---
```

### Larger teams or higher-stakes decisions

Consider adding:
- `cc: [other-team-member]` in frontmatter to loop in additional reviewers
- A `decisions/` directory (see Framework 05) for decisions that outlive the thread

---

## Relationship to Other Patterns

- **Framework 05 (Cross-Project Protocol)**: Threads resolve questions about
  documents in `protocols/` or `proposals/`. After a thread closes, update
  the protocol document and bump the version.
- **Framework 02 (Document Taxonomy)**: Threads are TYPE-C (transient). They
  live in `threads/` while open, move to `archive/threads/` when closed.
  They do not go in `protocols/`.
- **Framework 03 (Completion Checkpoint)**: Closing a thread that unblocks
  implementation is itself a checkpoint — update your `session_state.md`
  when a blocking thread moves to `resolved`.
