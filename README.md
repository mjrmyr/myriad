# Myriad

This repository includes agent skills that I use daily either in my job as a software engineer or while working on private projects.

It includes a couple of design and engineering principles that are very helpful in the work with coding agents.
They help steer the agents thinking process and make the agents work more reproducible.
These principles will be updated and extended over time as I see need.

In addition to principles this repository also includes more traditional skills that the agent can use to follow a certain engineering or reasoning process.
These skills provide prompts, templates or simply descriptions that guide those processes. This often makes use of spawning sub-agents.

There is also a personal mode `p-mode` that is supposed to resemble my style of engineering and solving problems.
It enables the coding agent to work on architecture, features or bugs in great autonomy by also having the ability to verify its own work.
I don't want to sit there babysitting agents all day long, instead I want to get to a state where I can trust the work of my agents.
This way I not only free up capacity to think about important decisions, I also get a partner to brainstorm and challenges ideas with.

Some of the principles and "traditional" skills are entangled by design especially when it comes to `p-mode`.
The reasoning behind that is that the agents gains alternative approaches and references it can draw from for relatable tasks and problems.
This seems to make the agent more capable. However, it also makes it more difficult to use only a subset of these skills.

## Kaeno agent profiles

`agents/kaeno/AGENT.md` and `agents/swe/AGENT.md` are repository-backed profiles for Kaeno's configured environment loader. They retain the homelab assistant and engineer instructions and models:

- `mjrmyr/myriad:kaeno`: conversational coordinator using `openai/gpt-6-luna`, with an exact delegation/skill-reading tool list and no direct filesystem or command tools.
- `mjrmyr/myriad:swe`: software engineer using `openai/gpt-6.1-sol`. Its `skills: [mjrmyr/myriad:*]` selects all valid skills at the resolved revision; tools are omitted so the host's approved coding/delegation defaults apply.

For a runtime implementing repository-backed definitions, use one owner configuration:

```json
{
  "sourceDirectory": "./sources",
  "agentRepositories": ["mjrmyr/myriad:<full-commit-sha>"],
  "skillRepositories": ["mjrmyr/myriad:<full-commit-sha>"],
  "kaeno": {
    "agent": "mjrmyr/myriad:kaeno",
    "skills": ["mjrmyr/myriad:p-mode", "mjrmyr/myriad:how"]
  },
  "workers": ["mjrmyr/myriad:swe"]
}
```

Replace both `<full-commit-sha>` placeholders with the **same** published 40-character commit. Start with `KAENO_CONFIG=/path/to/config.json`; relative paths resolve beside that file. The loader acquires the repository once for both catalogs. Use writable host-only persistent storage for sources; do not mount it into workers. Kubernetes execution-backend configuration remains separate from this source/role configuration.

The Kaeno profile intentionally has no default skill assignments: the owner selects `p-mode` and `how` above. SWE's wildcard is expanded and frozen to concrete skills when loading/admitting work; it does not fetch new skills during a task or grant tools. Updating a pin affects future loads, not saved work.

Profile frontmatter supports `name` and `description` (required), plus `model`, `thinkingLevel`, `skills`, and `tools`. Omitted tools use host defaults; an explicit list, including `[]`, is exact and must be installed/approved by the host. Additional metadata is inert, not an authority grant; delegation policy and execution-backend options belong to host configuration, not profile metadata. These profiles add no native extensions and do not install repository code. Existing skill installers below remain usable independently; a legacy Kaeno image cannot consume the new owner configuration.

## Installation

This repository is public so you can use the obvious way of installing these skills via `skills.sh`.
Installation is project-scoped, for global scope add `--global` to the command.

```bash
# Inspect skills before installing
npx skills add mjrmyr/myriad --list

# Add all skills
npx skills add myrmjr/myriad

# Add only a certain skill
npx nx skills add mjrmyr/myriad --skill pr-review
```


---


I find this idea of equipping the agent with important principles it can use to reason from very apt. 
Also, the idea of having a more personalized coding mode for an autonomous agent is very appealing.
These originate from poteto and `p-stack`, where I also borrowed the `arena`, `architect` and `how` skill from.
