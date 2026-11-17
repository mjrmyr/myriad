---
name: git-workflow
description: The owner's rules for commits, branches and pull requests. Load whenever committing, pushing or opening a PR for the owner, in any repository.
---

# Git Workflow

These rules apply in every repository, including ones with no linting for them.

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
