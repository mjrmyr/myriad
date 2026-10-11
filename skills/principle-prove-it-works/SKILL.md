---
name: principle-prove-it-works
description: Apply before declaring work done. Verify against the real command, artifact, UI, value, or behavior rather than a proxy.
disable-model-invocation: true
---

# Prove it works

Prove the change against the real artifact. Do not rely on "looks right" if a command, test, page, or script can check it.

Use this principle when:

- Finishing any code change.
- Reporting that something works.
- Comparing alternatives.
- A check passes suspiciously easily.
- An indirect signal stands in for behavior.

Rules:

- Run the command that proves the behavior.
- Inspect the real output, value, page, or diff.
- Prefer a deterministic script over a one-time eyeball check.
- If verification fails, question the observation method before guessing.
- Say exactly what you verified and what remains unverified.

The test:

A reviewer should be able to rerun or inspect the same evidence without trusting your summary.
