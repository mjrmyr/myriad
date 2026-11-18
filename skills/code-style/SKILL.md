---
name: code-style
description: The owner's rules for how code reads. No comments, explicit names and structure. Load whenever writing or reviewing code for the owner, in any repository.
---

# Code Style

These rules apply in every repository, including ones with no linting for them.

- Write no code comments, including doc comments on new code. Express intent with names, types and structure; if code seems to need a comment, simplify it. Leave existing comments alone unless the repo's tooling requires otherwise.
  Why: comments are a smell.
- Write explicit, readable code. Functions are declarations, not variables. Use descriptive names (no `seen`, `tmp`, `x`). Avoid dense one-liners a reader could miss. Keep error messages precise and update them when behaviour changes. Split files that grow past a few hundred lines.
  Why: the reader should never have to decode the code.
