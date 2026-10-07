---
name: principle-smallest-useful-change
description: Apply when a refactor is tempting, a change starts growing, or a new abstraction appears. Prefer the smallest change that solves the real problem.
disable-model-invocation: true
---

# Smallest useful change

Prefer the smallest change that solves the real problem. Delete or simplify before adding layers.

Use this principle when:

- A refactor is tempting.
- The change starts growing.
- A new abstraction appears.
- The solution requires passing a new flag or option through many layers.
- The code would become harder to read after the change.

Rules:

- Prefer deletion before addition.
- Keep the diff small.
- Avoid new abstractions unless they remove repeated decisions or branches.
- Collapse pass-through wrappers when they do not carry real ownership.
- Do not preserve compatibility paths unless a current caller needs them.
- Stop when the problem is solved.

The test:

A future maintainer should be able to see why the change exists and why it is no larger than necessary.
