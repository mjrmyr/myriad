---
name: pr-review
description: Review an existing Github pull request. Use when the user provides a PR number or URL and asks for a review.
compatibility: Requires git and Github CLI (gh) authenticated for the target repository.
---

# Pull request review

## Instructions

1. Create a new isolated worktree.
2. Switch to the new worktree.
3. Run the `gh pr view` command to retrieve all relevant details from the pull request.
4. Review the complete base-to-head diff and also consider the surrounding implementations, not only the changed files.
5. Remove the created worktree.
6. Summarize the changes implemented by the pull request 
    - Format the PR review summary as documented in this template "../assets/template.md".
    - Follow the guidelines for the PR review summary as documented in this docoument "../assets/guidelines.md"
7. Add the summary to the PR via the following command: `gh pr comment <pr-number> --body <pr-summary>`

## Out of scope
- Don't run linters
- Don't run tests
- Don't run formatters
- Don't modify the code
