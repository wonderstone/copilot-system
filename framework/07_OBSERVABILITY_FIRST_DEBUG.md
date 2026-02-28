# Framework 07 — Observability-First Debug Loop

> **The core problem**: When something breaks, AI assistants default to speculative fixes.  
> "Maybe it's X" → change code → still broken → "Maybe it's Y" → change more code.  
> Three rounds later, the codebase has unnecessary changes and the root cause is still unknown.

---

## The Rule

When a bug is reported or discovered, the AI must **collect evidence before proposing fixes**.

The loop, in order:

```
1. Observe    → Collect concrete signals (logs, errors, state dumps)
2. Localize   → Name the exact component and stage where failure occurs
3. Fix        → Minimal change at root cause
4. Verify     → Re-run the failing path end-to-end
5. Cleanup    → Remove temporary debug instrumentation
```

**Each step must complete before moving to the next.** Step 3 (Fix) without Step 1-2 (Observe + Localize) is prohibited.

---

## Step Details

### 1. Observe (collect signals first)

Before any hypothesis, gather concrete data:
- Enable DEBUG logging for the relevant module(s)
- Extract targeted evidence from log files using keyword filters
- Check process output, API responses, or external system state
- Report what signals are present, not what you *think* is happening

### 2. Localize (identify exact stage/component)

State the failure point explicitly:
- Name the concrete function, module, or stage (not "maybe the backend" — which function?)
- When there's a pipeline, identify which stage completed and which didn't
- Example: "`prefetch()` returns successfully, but `init()` is stuck at 0/N items"

### 3. Fix (minimally at root cause)

- Prefer deterministic fixes: input validation, timeout fallbacks, retry logic, guard clauses
- Prefer fixes at the root cause over workarounds downstream
- If uncertain after observing, add more instrumentation first — don't guess

### 4. Verify (end-to-end, required)

- Re-run the exact failing scenario
- Confirm the pipeline reaches successful completion
- Report: root cause, exact fix applied, and which signal confirms resolution

### 5. Cleanup (required)

- Restore temporary DEBUG log levels back to INFO
- Remove or downgrade verbose debug output added during investigation
- This step is mandatory — debug noise left behind degrades the codebase

---

## Language Rules for AI

| Allowed | Not allowed (without evidence) |
|---------|-------------------------------|
| "The log shows X at line N" | "It's probably X" |
| "Function F returned error E" | "Maybe the issue is in F" |
| "I need to check [specific thing] to determine the cause" | "Let me try fixing X and see if that helps" |

**The key constraint:** phrases like "likely", "maybe", or "probably" must be accompanied by:
1. What evidence is missing
2. A concrete next step to collect that evidence

---

## Escalation Rule

When a fix attempt fails:
- Do **not** try another speculative fix
- Instead, **increase observability**: add per-stage metrics, component labels, or state dumps
- Use the new signals to re-enter the loop at Step 1

This prevents the "fix spiral" where each failed attempt adds complexity without progress.

---

## Why This Works

AI assistants are trained to be helpful, which creates a bias toward action. When something breaks, the AI wants to fix it *now*. But acting without evidence compounds the problem:

- Speculative fixes change code that wasn't broken → new bugs
- Multiple failed attempts obscure the original failure
- The human loses trust after 2-3 wrong fixes

The observe-first discipline trades a few minutes of data collection for dramatically higher fix accuracy. In practice, the evidence almost always points directly to the root cause — making Step 3 trivial.
