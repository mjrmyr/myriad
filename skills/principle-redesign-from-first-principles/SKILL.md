---
name: principle-redesign-from-first-principles
description: Apply when integrating a new requirement into an existing design. Redesign as if the requirement had been a foundational assumption from day one, instead of bolting it on.
---

# Redesign From First Principles

When integrating a change, don't bolt it onto the existing design. Redesign as if the requirement had been there from the start.

- Read all affected files and understand the current design
- Ask: "if we were writing this from scratch with this new requirement, what would we build?"
- Propagate the change through every reference: types, docs, examples, rationale sections
- Think about the whole redesign, then deliver it incrementally
- If the change adds a hook, an optional field or a new seam that only one caller uses, it is "a patch rather than a feature"; look for the version that needs none

This is the method for preserving option value when integrating changes into an existing design.
