---
name: pr-review
description: Review an existing Github pull request. Use when the user provides a PR number or URL and asks for a review.
compatibility: Requires git and Github CLI (gh) authenticated for the target repository.
---

# Pull request review

## Instructions

1. Create a new branch and an isolated worktree, switch to the new worktree
2. Run the `gh pr view` command to retrieve the relevant details from the pull request
3. Review the complete base-to-head diff and also consider the surrounding code, not only the changed files
4. Remove the created worktree, and also delete the created branch
5. Summarize the changes implemented by the pull request
    - Format the PR review summary as documented in this template "assets/template.md"
    - Follow the guidelines for the PR review summary as documented in this document "assets/guidelines.md"
6. Add the summary to the PR via the following command: `gh pr comment <pr-number> --body <pr-summary>`

## Out of scope
- Don't run linting
- Don't run tests
- Don't run formatting
- Don't modify the code
