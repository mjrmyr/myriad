---
name: mise-en-place
description: Project specifics for the cook skill (the agent kitchen). Load it when the cook skill asks for the project's mise-en-place.
---

# Mise-en-place

<!-- Describe this project's kitchen. The cook skill reads every section below. Keep each section to what an agent
cannot find out on its own, and point to existing project skills instead of copying them. -->

## Ticket intake

<!-- Where the ticket content comes from (a tracker the agent can read, or pasted after the key), the key format, and
the ticket url pattern. -->

## Preflight

| check | pass condition | fix |
| --- | --- | --- |
| <tool or credential> | <command and expected result> | <command or action for the user> |

## Workspace

<!-- Where the isolated worktree goes, the branch naming, the base branch, and the install steps that run inside it. -->

## Environment

<!-- How the taster starts and stops the application from the workspace. Name the shared and singleton resources
(ports, containers, databases) and how to tell who owns them. -->

## Validation

<!-- Commands the line cook runs before every commit, for example lint and unit tests of the affected projects. -->

## Verification

<!-- The project skill the taster runs, and the facts it needs: feature flags, test accounts, known dialogs. -->

## Conventions

<!-- Commit and pull request titles, the target branch, the pull request body format, and code conventions that are
not already in the agent instructions. -->

## Never run

<!-- Commands that destroy shared state or publish things, for example database resets or force-pushes. -->

## Agents

<!-- Optional. Harness-specific worker definitions, for example a Claude Code taster agent without edit tools. -->
