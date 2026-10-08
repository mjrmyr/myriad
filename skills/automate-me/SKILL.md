---
name: automate-me
description: Create or update a personal mode skill that captures the user's working style. Use when the user asks to "automate me", "capture my preferences", "make/update my mode skill", or wants agents to follow how they work.
---

# Automate Me

Turn the user's working conventions into a personal `-mode` skill agents can follow.

The output is one concise mode skill, such as `alex-mode` or `peter-mode`. It should encode durable preferences, not every one-off instruction.

## Flow

### 0. Find the target skill

Search the active skill locations for existing `*-mode/SKILL.md` files:

- The current repository's skill directories, such as `skills/`, `.agents/skills/`, `.claude/skills/`, or `.cursor/skills/` when present.
- The user's personal skill directory when known from the harness, environment, or existing configuration, such as `~/.agents/skills/`.

Prefer the format already used by the current project or agent harness. Do not assume Cursor paths or Cursor-only metadata.

If a matching mode skill exists, update it instead of starting over unless the user explicitly asks for a fresh version. Preserve sections that new evidence does not contradict.

### 1. Gather evidence

Use available sources in this order:

1. Existing mode skills and principle/workflow skills.
2. Current repository conventions.
3. Recent agent transcripts or chat logs, inside the active workspace, the harness-provided transcript path or other popular locations (e. g. `~/.claude`, `~/.agents`)
4. Direct user input.

Do not crawl unrelated project histories or unrelated transcript directories unless the user explicitly opts in.

When mining transcripts, look for repeated signals:

- Response style: length, tone, formatting, examples, directness.
- Autonomy: when to proceed, when to ask, acceptable assumptions.
- Verification: tests, live repros, screenshots, commands, review expectations.
- Code and prose discipline: naming, domain modeling, smallest-change bias, style rules.
- Process: commits, PRs, worktrees, issue tracking, tool preferences.
- Skill habits: when to write, update, or invoke skills.

Treat repeated patterns as strong evidence. Treat a single occurrence as weak unless the user confirms it.

### 2. Ask only for what evidence cannot answer

Ask one or two focused questions, not a survey. Prefer multiple-choice options when useful, plus one free-form prompt for anything missing.

Examples:

- "Should this be a repo skill or a personal skill?"
- "Which preferences matter most: reply style, autonomy, verification, process, code style, skill behavior?"
- "Anything agents repeatedly get wrong about your style?"

If the answer can be found by inspecting files or running commands, do that instead of asking.

### 3. Cluster into sections

Use only sections that carry specific instructions. Common sections:

- Trigger
- Default behavior
- Reply style
- Autonomy
- Understand first
- Verification
- Code discipline
- Process
- Skills

Skip generic advice. "Communicate clearly" is not useful. "Short paragraphs; include commands run when verification matters" is useful.

### 4. Draft the mode skill

Follow the destination harness's skill format. For this repository's format, use:

```yaml
---
name: <handle>-mode
description: Personal mode for <handle>. Use when the user asks for <handle>-mode or wants work done in their style.
mode: true
---
```

Guidelines:

- Keep the description trigger-specific. Avoid broad triggers like "write code".
- Use "the user" or "the human" in instructions, not the person's name.
- Reference other skills by name or path instead of pasting their full content.
- Prefer bullets and imperatives.
- Keep it short enough that an agent will actually follow it.

### 5. Edit ruthlessly

Remove:

- Flattery.
- Vague preferences.
- Duplicated rules from other skills.
- Rules based on one weak signal.
- Tool-specific instructions unless that tool is actually part of the user's workflow.

Every line should change agent behavior.

### 6. Verify with the user

Show the draft or summarize the diff. Ask whether it reads like their working style and what is missing.

If the repository uses git and the user wants the change landed, follow the repo's normal process. Do not assume a PR, worktree, or push workflow unless the project or user indicates one.

## Guardrails

- Be generic across agent harnesses. Do not require Cursor, Cursor transcripts, Cursor tools, or Cursor frontmatter.
- Respect transcript boundaries. Active workspace only by default.
- Do not overfit to one conversation.
- Do not invent preferences to make the skill feel complete.
- Do not inline other skills or long examples.
- Prefer updating existing skills over creating competing ones.
- Keep personal mode skills operational, not biographical.

## When not to use

- The user wants a task-specific workflow skill. Write a regular skill instead.
- The user wants to capture one narrow convention, such as commit messages. Add or update a focused skill unless it clearly belongs in their mode skill.
