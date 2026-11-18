---
name: hidden-intent-review
description: Find intent that lives only in comments, suppressions or deleted prose, and rank the code changes that make it explicit. Use when asked to audit comments, before or after a PR that removes comments, or to find hidden invariants in a diff, branch or package.
---

# Hidden Intent Review

A comment that explains code means the code does not say it. This review finds that intent and turns it into ranked code changes: names, types, extracted functions, deletions. Never better comments.

## Scope

- Review what the user names: a diff, a PR, a directory. With nothing named, review the current branch against the default branch.
- For a change that deletes comments, the deleted text is the input. Read it from the diff and check the code still carries its intent.
- When the user ranks areas, for example core before plugins, rank only the prioritized area and list the rest separately.

## Classify

A comment survives only if it is:

- A legal or license header.
- Behavior forced by something outside the repository (vendor, platform, protocol) that the code cannot reshape, and still true today.
- A tool directive (formatter, linter, type checker) whose rule is wrong for this case or style-only.
- A contract of a public API consumed outside the repository.
- A link to an issue or spec for a constraint code cannot express.

Everything else is a finding. When unsure, it is a finding. A suppression whose rule protects correctness is a finding too.

## Verify

- Read the surrounding code and trace every symbol the comment names. Back each claim with callers, tests or history.
- Drop a finding when code already enforces it elsewhere, such as a runtime check or a test, and say where.
- Every finding names a real symbol in scope. Invent nothing.

## Fix

For each finding, name the exact symbol and the smallest code change that carries the intent:

- Rename, so the identifier says what it guarantees.
- Extract a named function or constant, so the invariant has one home and duplicated literals or checks cannot drift.
- Type it: a union, a required field, a typed result instead of a string or boolean convention.
- Delete it: dead branches, swallowed errors, code kept only for a past state.

When two similar checks differ on purpose, name both instead of merging them.

## Rank

Order by what losing the intent costs:

1. Security and authority boundaries.
2. Invariants whose violation loses data or corrupts state.
3. Duplicated literals or checks that can drift.
4. Hidden contracts between components.
5. Names and magic numbers.

Report the top ten as a table: rank, location, problem, fix. List the rest in one line each, then the dropped findings and why.

## Stored data

When a fix renames or retypes anything that is persisted (database rows, durable state, caches, config, external identifiers):

- List each stored change: record kind, field, old to new.
- Find every reader and state what an old record does after deploy: fails to load, fails validation, or reads as missing, and what that changes.
- Keep code-only renames apart from stored ones.
- State whether the deploy needs a reset or migration, or whether only in-flight work is affected.

## Output

Report only, unless asked to fix. When fixing, make one change per finding and verify with the repository's checks and tests.
