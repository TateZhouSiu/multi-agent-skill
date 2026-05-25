---
name: multi-agent
description: Create tight, auditable task briefs for multi-agent or sub-agent work. Use when Codex needs to plan, delegate, scope, or review work across roles such as read-only explorer, scoped implementation worker, auth/security reviewer, verification agent, or handoff agent; especially when the user provides or asks for a Role/Goal/Context/Allowed actions/Ownership/Forbidden actions/Output format/Stop condition template.
---

# Multi Agent

## Overview

Use this skill to turn a user goal into a bounded agent brief. The brief must make permissions, ownership, forbidden actions, expected output, and stop conditions explicit before any delegated or role-scoped work begins.

Do not spawn child agents just because this skill is active. Follow the active system/developer rules for whether delegation is allowed. If delegation is not allowed, use the same brief structure for your own plan, handoff, or review notes.

## Runtime Fit

This skill is Markdown-only and has no OS-specific runtime dependency. A normal coding model can draft simple briefs. Use a stronger reasoning model for security review, architecture exploration, long-horizon implementation, or multi-agent coordination.

Author-known-good profile for heavier workflows: Codex-style agent mode, GPT-5.5-class reasoning model, and `high` or `xhigh` reasoning when available. Multimodal input is only needed when the delegated task itself involves images, screenshots, or visual artifacts.

## Workflow

1. Identify the task class: exploration, implementation, security review, verification, or handoff.
2. Choose one role only unless the user explicitly asks for multiple roles.
3. Fill every required brief field with concrete paths, commands, boundaries, and stop conditions.
4. Make ownership disjoint for implementation work.
5. Set forbidden actions conservatively.
6. Require findings and evidence before summaries.
7. Stop if permissions, ownership, or data access are unclear enough that proceeding would risk unrelated changes.

## Role Selection

- **read-only explorer**: Use for codebase questions, architecture tracing, bug localization, auth flow reading, data-flow mapping, or risk discovery. Allow reading and non-mutating commands only.
- **scoped implementation worker**: Use for a bounded patch with clear file ownership. Allow writes only inside the listed files or directories.
- **security reviewer**: Use for auth, permissions, secrets, injection, SSRF, XSS, unsafe file access, data exposure, or trust-boundary review. Default to read-only unless explicitly assigned a fix.
- **verification worker**: Use for tests, lint, repro scripts, screenshots, logs, or confirming a behavior after implementation. Allow command execution; allow writes only for temporary artifacts if needed.
- **handoff writer**: Use to summarize state for a future agent. Do not modify production files unless explicitly requested.

## Required Brief Template

Use this exact structure when drafting a brief:

```markdown
Role:
  <one concrete role, e.g. read-only auth explorer / scoped implementation worker / security reviewer>

Goal:
  <what to answer or complete, and the boundary of done>

Context:
  <repo path, relevant files, current error/output, user goal, prior findings, assumptions>

Allowed actions:
  <read/write/network/test permissions; exact commands or command classes if useful>

Ownership:
  <for write tasks, exact files/directories/modules the agent owns; for read-only tasks, say "read-only, no write ownership">

Forbidden actions:
  <files/dirs not to touch, destructive commands, unrelated refactors, broad rewrites, asking user, spawning children, external services, etc.>

Output format:
  findings:
  patch summary:
  test result:
  confidence:
  open questions:

Stop condition:
  <what counts as done; what conditions require stopping and reporting a blocker>
```

## Field Guidance

**Role**
- Name the role as behavior, not a title. Prefer `read-only auth explorer` over `expert`.
- Include read/write posture in the role when it matters.

**Goal**
- State the user-visible outcome.
- Include what is out of scope.
- For review tasks, say whether to find issues only or also propose patches.

**Context**
- Include absolute repo path when known.
- List relevant files and commands already run.
- Include exact error text, route, test name, screenshot path, or data file when available.
- Mark assumptions explicitly.

**Allowed actions**
- For read-only roles: allow `rg`, `sed`, `ls`, `git diff`, `git show`, test/log reads, and local screenshots only if relevant.
- For implementation roles: allow writes only to owned files plus necessary tests.
- For network access: state whether it is allowed, disallowed, or only allowed for official docs / local services.
- For commands: distinguish read-only commands from mutating commands.

**Ownership**
- Assign exact files, directories, or modules.
- For parallel workers, keep write sets disjoint.
- Tell implementation workers they are not alone in the codebase and must not revert others' edits.

**Forbidden actions**
- Always include `do not run destructive git commands`, unless the user explicitly requested them.
- Always include `do not revert unrelated changes`.
- Default to `do not spawn child agents`.
- Default to `do not ask the user`; stop and report blockers instead.
- Add domain-specific exclusions such as auth secrets, migrations, generated data, or public UI files when not owned.

**Output format**
- For reviews, put findings first and order by severity with file/line evidence.
- For implementation, include patch summary and tests.
- For exploration, include findings, evidence, confidence, and next recommended step.
- Use `open questions` only for unresolved blockers or real ambiguity.

**Stop condition**
- Define both success and blocker exits.
- Stop on missing files, impossible permissions, unclear ownership, failing required setup, or evidence that the task would require out-of-scope changes.

## Ready-Made Briefs

### Read-Only Auth Explorer

```markdown
Role:
  read-only auth explorer

Goal:
  Trace the auth flow and identify where <issue> can originate. Do not modify code.

Context:
  Repo: <absolute path>
  Relevant files/routes: <list>
  Error/user symptom: <exact text>
  Existing judgment: <known assumptions or prior findings>

Allowed actions:
  Read files and run read-only commands such as rg, sed, ls, git diff, git show, and tests in dry/read-only mode if they do not write artifacts.

Ownership:
  read-only, no write ownership.

Forbidden actions:
  Do not edit files. Do not run migrations or destructive commands. Do not change auth config or secrets. Do not ask the user. Do not spawn child agents.

Output format:
  findings: ordered by likelihood/severity with file and line references
  patch summary: none
  test result: commands run or not run
  confidence: high/medium/low with reason
  open questions: only blockers

Stop condition:
  Complete when the likely root cause and evidence are identified. Stop and report if required files or logs are unavailable.
```

### Scoped Implementation Worker

```markdown
Role:
  scoped implementation worker

Goal:
  Implement <bounded behavior> without changing unrelated behavior.

Context:
  Repo: <absolute path>
  Relevant files: <list>
  User goal: <goal>
  Existing constraints: <constraints>

Allowed actions:
  Edit only owned files. Run focused tests/lint/typecheck related to the touched files. Read adjacent files as needed.

Ownership:
  Owns writes to: <exact files/directories>
  This worker is not alone in the codebase; accommodate others' changes and do not revert edits you did not make.

Forbidden actions:
  Do not edit outside ownership. Do not refactor unrelated code. Do not run destructive git commands. Do not change generated data unless listed. Do not ask the user. Do not spawn child agents.

Output format:
  findings: any blockers or risks found before/during implementation
  patch summary: files changed and behavior changed
  test result: commands run and outcomes
  confidence: high/medium/low with reason
  open questions: only unresolved blockers

Stop condition:
  Complete when the patch is applied and focused validation has run. Stop if the fix requires files outside ownership or a product decision not in context.
```

### Security Reviewer

```markdown
Role:
  security reviewer

Goal:
  Review <scope> for auth, authorization, secret handling, injection, SSRF, XSS, file access, and data exposure risks. Do not implement fixes unless explicitly allowed.

Context:
  Repo: <absolute path>
  Scope: <files/routes/modules>
  Threat model: <user/admin/public/internal, trust boundaries, sensitive data>
  Recent changes or concern: <summary>

Allowed actions:
  Read scoped files and adjacent security-relevant code. Run read-only searches. Run tests only if they are non-mutating or clearly scoped.

Ownership:
  read-only, no write ownership unless a separate implementation scope is provided.

Forbidden actions:
  Do not edit files. Do not change auth, secrets, environment, migrations, or policy files. Do not run destructive commands. Do not ask the user. Do not spawn child agents.

Output format:
  findings: severity, exploit path, evidence, affected files/lines, recommended fix
  patch summary: none unless explicitly assigned
  test result: commands run or not run
  confidence: high/medium/low with reason
  open questions: only blockers

Stop condition:
  Complete when scoped risks are reviewed and findings are prioritized. Stop if the threat model or required files are missing.
```

## Brief Quality Checklist

Before using or sending the brief, verify:

- The role matches the allowed actions.
- The goal has a clear boundary.
- Context includes exact paths or artifacts.
- Write ownership is explicit and narrow.
- Forbidden actions include no unrelated reverts and no destructive commands.
- Output format asks for evidence, not just conclusions.
- Stop condition prevents scope creep.
