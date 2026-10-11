---
name: principle-fix-root-causes
description: Apply when debugging. Reproduce first, trace symptoms to the cause, and fix the cause rather than masking the symptom.
disable-model-invocation: true
---

# Fix root causes

Do not patch symptoms without tracing why they happen.

Use this principle when:

- Debugging.
- A fix feels like a special case.
- The same issue appears twice.
- A guard would silence an error without explaining it.
- The problem disappears after clearing state or restarting.

Pattern:

1. Reproduce or find concrete evidence.
2. Trace from symptom to cause.
3. Ask why until the cause stops moving.
4. Fix the cause.
5. Check for the same pattern elsewhere.
6. Verify the original symptom is gone.

Rules:

- Do not add guards just to silence crashes.
- Do not guess when instrumentation can answer.
- Treat stale persistent state as a suspect in restart bugs.
- Prefer fixing the invariant over documenting the workaround.

The test:

The fix should explain the symptom. If it only prevents the symptom from showing, keep digging.
