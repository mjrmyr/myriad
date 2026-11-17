---
name: principle-no-compat-before-live
description: Apply when changing data shapes, config, storage or APIs in a project that isn't live yet. Make the breaking change directly; add no migrations, legacy guards or compat shims.
---

# No Compatibility Before Live

In a project that isn't live, nothing depends on the old shape. Change it directly.

**Why:** "we do not plan for backwards compatibility at the moment, this is still in development". Compatibility code for users who don't exist is permanent debt.

**The pattern:**
- No migrations, legacy guards, deprecation errors, backfills, compat shims or "historical data" branches.
- Breaking changes are fine. Say in the PR what to change by hand when deploying, such as resetting a database or renaming a config key.
- Once a project is live, this no longer applies; ask before breaking anything.

**The test:** Does this code exist only to handle data or callers from before the change? If so, delete it.
