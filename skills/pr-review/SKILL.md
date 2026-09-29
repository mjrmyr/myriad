---
name: pr-review
description: Review an existing Github pull request. Use when the user provides a PR number or URL and asks for a review.
compatibility: Requires git and Github CLI (gh) authenticated for the target repository.
---

# Pull request review

## Instructions

1. Create a new isolated worktree.
2. Switch to the new worktree.
3. Run `gh pr view` to retrieve all relevant details from the pull request.
4. Review the complete base-to-head diff.
5. Read the surrounding implementations, not only the changed files.
6. Remove the created worktree.
7. Summarize the changes implemented by the pull request.

## Out of scope
- Don't run linters
- Don't run tests
- Don't run formatters
- Don't modify the code
