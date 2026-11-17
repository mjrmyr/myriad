---
name: principle-code-lives-with-its-owner
description: Apply when deciding where a capability, its config or its startup checks go. Put them in the module that owns the capability, not in the entry point or a shared core.
---

# Code Lives With Its Owner

A capability, its configuration and its startup checks belong in the module that owns them.

**Why:** "it sits where it is used". Code spread into the entry point or a shared core makes every feature touch central files, and the reader has to assemble one capability from several places.

**The pattern:**
- Parse and validate a capability's config in its own module, not in the entry point.
- Keep startup checks for a capability next to the capability.
- A shared core holds only what is genuinely shared; it doesn't learn about each feature.

**The test:** Could you delete the capability by removing one module plus one registration line? If not, pull the scattered pieces back to their owner.
