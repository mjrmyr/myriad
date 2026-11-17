---
name: conventions
description: The owner's engineering conventions for code, git, pull requests and docs. Load whenever writing code, commits, PRs or docs for the owner, in any repository.
---

# Conventions

These rules apply in every repository, including ones with no linting for them. Each rule is followed by why it exists.

## Code

- Write no code comments, including doc comments on new code. Express intent with names, types and structure; if code seems to need a comment, simplify it. Leave existing comments alone unless the repo's tooling requires otherwise.
  Why: comments are a smell.
- In projects that aren't live, add no backward compatibility: no migrations, legacy guards, deprecation errors, backfills, compat shims or "historical data" branches. Breaking changes are fine; say in the PR what to change by hand when deploying.
  Why: "we do not plan for backwards compatibility at the moment, this is still in development".
- Prefer required over optional. Don't add optional parameters, absent modes or `?? default` fallbacks to avoid updating callers or for states that can't happen.
  Why: "i don't want it to be optional only because of laziness".
- Put code with its owner. A capability, its config and its startup checks go in the module that owns them, not in the entry point or a shared core.
  Why: "it sits where it is used".
- Bake features into the design instead of bolting them on. If a change adds a hook, an optional field or a new seam that only one caller uses, look for the version that needs none.
  Why: otherwise it is "a patch rather than a feature".
- Write explicit, readable code. Functions are declarations, not variables. Use descriptive names (no `seen`, `tmp`, `x`). Avoid dense one-liners a reader could miss. Keep error messages precise and update them when behaviour changes. Split files that grow past a few hundred lines.
  Why: the reader should never have to decode the code.
- Prefer generic over hard-coded. When a set is meant to grow, use open string identifiers resolved against registrations, not closed enums plus switches.
  Why: adding a member should not mean editing every switch.
- Keep it minimal; everything has to earn its place. No test-only constructs in production code, no tests or scripts that aren't needed, no setup the feature doesn't justify.
  Why: "keep it simple, keep it minimal, it just needs to work for the start".
- Keep owner names and private identifiers out of source. Use neutral words like "user" or "owner".
  Why: source gets shared; personal details should not travel with it.
- Validate configuration strictly and fail loudly at startup. At runtime, let a failing integration degrade as narrowly as possible: drop the one tool, not the server; log, don't crash the host.
  Why: bad config is caught early, and one broken part never takes down the rest.

## Git and pull requests

- Write commit messages as a single all-lowercase headline: `feat|fix|refactor|chore: summary`. Only those four types, no scope, no body. A required co-author trailer is the only extra line.
  Why: the owner rewrote commits that had bodies and "hates" anything else.
- Make one change per commit. Add follow-up commits to the existing PR instead of opening a new one. Don't force-push a shared branch unless asked.
  Why: history stays readable and nobody loses work.
- Keep one feature per PR, and open PRs ready for review, not as drafts. Rationale goes in the PR description, not in commits or code.
  Why: the PR is where review and discussion happen.
- Work in a fresh branch, and a fresh worktree when the tool supports it. Push and open a PR when done.
  Why: parallel work stays isolated and every change gets reviewed.
- Don't add scope nobody asked for. Record decisions in the PR description so other agents and reviewers on the same branch see them. Fetch before pushing; if another agent pushed, look at what it changed before overriding it.
  Why: several agents may share a branch.

## Docs and communication

- Add no design documents, proposals or plan files to the repo. Discuss through a working code draft and the PR description. Keep docs to reference a user needs to configure or operate the software.
  Why: code is easier to discuss than prose about code.
- Explain in simple words, short, facts first. Give a recommendation rather than a survey. When asked to discuss first, don't change code yet.
  Why: the owner wants a judgment to act on, not homework.
