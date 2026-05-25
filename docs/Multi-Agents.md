# Multi-Agents

The `multi-agent` skill is a brief-writing discipline for complex agent work. It does not require spawning sub-agents. The same structure is useful for your own plan, a delegated task, a review request, or a handoff note.

## When To Use It

Use a bounded brief when a task involves:

- parallel exploration,
- scoped implementation,
- security or correctness review,
- verification after a change,
- handoff across context windows or sessions.

## Required Brief

```markdown
Role:
  <one concrete role>

Goal:
  <what to answer or complete, including boundary of done>

Context:
  <repo path, relevant files, current error/output, prior findings, assumptions>

Allowed actions:
  <read/write/network/test permissions>

Ownership:
  <exact write scope, or "read-only, no write ownership">

Forbidden actions:
  <destructive commands, unrelated refactors, secrets, broad rewrites, etc.>

Output format:
  findings:
  patch summary:
  test result:
  confidence:
  open questions:

Stop condition:
  <success exit and blocker exit>
```

## Practical Rule

Use one role at a time unless the user explicitly asks for multiple. For implementation work, keep write ownership narrow and disjoint. For review work, findings and evidence come before summary.
