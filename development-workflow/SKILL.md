---
name: development-workflow
description: Coordinate the user's human-gated software delivery process - question-driven planning, agent-created Git worktree/branch, independent implementation and review sessions mediated by the user, and user-only commits, merges, GitHub builds and remote server commands. Use when planning or executing a software \change.
---

# Development Workflow — Human-Gated Delivery

## Core operating contract

The user orchestrates delivery and retains control of consequential operations. Agents work in **separate sessions**: planning, implementation, and independent review. The user carries information and decisions between the implementation and review sessions. Never assume shared chat history across sessions.

Use `task-sizing` to assess complexity **before** a detailed plan when task size is uncertain or the requested scope warrants it. Task sizing is a distinct activity: its size assessment is **not** a fixed-phase implementation plan.

## Ownership and roles

- **User:** responds to planning questions; approves the scope/plan; mediates between implementation and review sessions; decides acceptance; performs commits and merges; triggers version builds on GitHub; runs scripts/commands on non-local servers.
- **Planning agent:** analyzes requirements and repository, asks consequential questions, produces a bounded plan and handoff. Does not implement without user approval.
- **Implementation agent (own session):** creates the approved worktree and branch, implements bounded units, runs permitted local checks, reports changes. Never self-certifies as an independent reviewer.
- **Review agent (different session):** inspects the real diff and relevant context, identifies verified defects/risks, and reports findings to the user. Does not silently modify code.

## 1. Interactive planning

1. Confirm objective, boundaries and relevant repository conventions; inspect code when available.
2. Ask the user questions **before locking important requirements**; focus on observable behavior, interfaces, migration, failure handling, constraints, and acceptance criteria. Ask manageable batches, not speculative trivia.
3. Record decisions, assumptions, unresolved questions, and dependencies.
4. If the change is not obviously tiny, size it using `task-sizing`; for L/XL/XXL, decompose into independently deliverable slices.
5. Prepare a detailed implementation plan **only for the next approved slice**. Make it proportional to the task, not a boilerplate ten-phase document. Include acceptance criteria, relevant files/interfaces, tests, review checkpoints, dependencies and **USER ONLY** actions.
6. **Wait for the user's explicit approval** before creating the worktree or implementing.

## 2. Agent-created worktree and branch

After approval, the implementation agent inspects existing worktrees/branches, uses the agreed base, then creates **a new worktree and descriptive branch** for this change.

- If the base branch is unclear, ask the user.
- Report branch name, base revision and worktree path.
- Keep changes inside the worktree; do not alter the main checkout.
- Do not reset, clean, delete, force-update, or overwrite unrelated work.
- Follow any repository-specific instructions.

## 3. Iterate: implementation ↔ user ↔ independent review

### Implementation session

- Implement only the approved unit(s); stop at a reviewable checkpoint.
- Run relevant local tests, linting and builds where safe. **Local Docker is permitted** when consistent with the environment/project policy.
- Report changed behavior/files, commands and their actual results, failed or skipped tests, assumptions and open risks.
- Supply a portable handoff suitable for a new review session.

### User mediates

The user passes the branch/worktree reference, diff and handoff to an **independent review session**. No direct agent-to-agent orchestration is assumed.

### Review session

- Inspect the **actual changes**, requirements, and tests; do not trust only the implementer's summary.
- Prioritize correctness, requirements, regressions, data integrity, security, and test coverage.
- Give actionable findings: severity, file/line, evidence, how to verify, recommended fix. Clearly separate confirmed defects from risks and style preferences.
- Report what was reviewed, what could not be verified, and whether any blocking findings remain.
- Do not fix code unless the user independently changes the reviewer's assignment.

### User passes findings back

The user chooses which findings to pass to the implementation session. The implementer addresses them and provides a new handoff. Repeat as needed; **do not assume a fixed number of review iterations**. The user decides when to accept.

## 4. Git operations — USER ONLY

**Only the user commits and merges.** Without explicit, operation-specific override, agents must **not** execute `git commit`, `git merge`, `git rebase`, `git push`, open/merge PRs, or perform similarly consequential Git actions.

Agents may inspect `git status`, logs and diffs, and suggest exact commands or commit messages. Leave uncommitted changes in the worktree and disclose the state at each handoff.

## 5. GitHub version build — USER ONLY

**Only the user triggers GitHub builds / Actions workflows and releases.** Agents may inspect workflow configuration, help with failures and suggest changes, but cannot trigger workflows, publish releases or deploy unless explicitly authorized for that exact action.

## 6. Server scripts — USER ONLY

**Only the user runs scripts or commands on servers outside the local development environment.** Agents can prepare, explain and review remote commands/scripts, including prerequisites, verification, dry-run and rollback, but must not SSH, run remote Docker contexts, apply server migrations or deploy without explicit task-specific permission.

**Local Docker exception:** agents may run local containers/tests. This does *not* grant permission for remote hosts, remote contexts or production resources.

## Handoff templates

### Implementation → User → Reviewer

```text
Branch / worktree:
Base revision and diff range:
Approved unit(s) and acceptance criteria:
Changed files and behavior:
Tests/checks actually run (results):
Known issues and uncertainty:
Requested review focus:
Commands to inspect/verify:
```

### Reviewer → User → Implementer

```text
Reviewed diff/revision:
Findings (severity, file:line, evidence, required action):
Unverified assumptions / other risks:
Checks performed and results:
Disposition: blocking / non-blocking / no blocking findings
```

### Ready for user-controlled finish

```text
Delivered scope:
Worktree / branch:
Local checks and review status:
Outstanding risks:
Suggested commit message (not executed):
USER ONLY: commit, merge, GitHub build, remote server scripts/deployment
```

## Anti-patterns

- Do not implement while requirements need user decisions.
- Do not generate 400–1000 lines of boilerplate regardless of feature size.
- Do not let the implementer act as the independent reviewer in the same session.
- Do not skip the user-mediated handoff or claim unstated approval.
- Do not silently perform user-only operations.
- Do not claim build/review/test success without actual evidence.
