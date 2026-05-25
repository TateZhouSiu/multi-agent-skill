# Multi-Agent Skill

A reusable Agent Skill for writing bounded, auditable role briefs.

This skill does not require spawning sub-agents. It gives an agent a strict brief format for exploration, implementation, review, verification, and handoff work.

## What It Does

The skill turns vague complex work into a task contract:

- role,
- goal,
- context,
- allowed actions,
- ownership,
- forbidden actions,
- output format,
- stop condition.

That structure makes delegated work easier to review and makes solo agent work less likely to drift.

## Install

Copy the skill folder into a compatible agent skills directory:

```text
skills/multi-agent/
```

For Codex-style local installation, copy it into your local skills directory and restart the agent.

## Runtime Fit

This skill is Markdown-only and has no OS-specific runtime dependency. It works on macOS, Linux, WSL2, and Windows as long as the host agent supports local skills.

Model guidance:

- A normal coding model is enough for simple brief drafting.
- Use a stronger reasoning model for security review, architecture exploration, long-horizon implementation, or multi-agent coordination.
- For the author's heavier workflows, the known-good profile is Codex-style agent mode with a GPT-5.5-class reasoning model and `high` or `xhigh` reasoning when available.
- This skill does not require multimodal input unless the delegated task itself involves images, screenshots, or visual artifacts.

## When To Use

Use it for:

- read-only code exploration,
- scoped implementation work,
- security or correctness review,
- verification after a patch,
- handoff between sessions or agents.

Do not use it for simple one-step tasks.

## Skill Entry

```text
skills/multi-agent/SKILL.md
```

## Relationship To AgentsMD Kit

AgentsMD Kit defines project-level rules and durable memory. Multi-Agent Skill defines task-level role briefs. They are intentionally separate.
