---
name: principle-required-over-optional
description: Apply when adding a parameter, field or mode, or reaching for a default fallback. Make it required and update the callers instead of making it optional.
---

# Required Over Optional

Make values required unless being absent is a real, reachable state.

**Why:** "i don't want it to be optional only because of laziness". An optional value forces every reader to ask what happens when it is missing, even when it never is.

**The pattern:**
- Don't make a parameter optional to avoid updating callers. Update the callers.
- Don't add absent modes or `?? default` fallbacks for states that can't happen.
- Keep optional only where absence means something the domain actually has.

**The test:** Can you name a real situation where this value is missing? If not, make it required.
