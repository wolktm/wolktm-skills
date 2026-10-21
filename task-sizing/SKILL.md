---
name: task-sizing
description: Evaluate software work in PRE-PLAN mode from requirements or PLAN-REVIEW mode from an existing implementation plan. Assess actual scope, uncertainty, hidden work, risk and implementation units without modifying the plan or executing the development workflow.
---

# Task Sizing — Pre-Plan and Plan Review

## Purpose

Make the actual scope of a change visible and comparable. A fixed number of phases, tasks, or lines is not a measure of project size. Do not turn an isolated edit and a new platform into similarly sized plans.

## Modes and mode selection

Choose the mode from the user's request; do not assume that a plan is absent.

- **PRE-PLAN**: Input is a feature idea, requirements, issue or design proposal. Estimate scope before detailed planning, identify independently verifiable units and unknowns.
- **PLAN-REVIEW**: Input is an existing implementation plan (such as a Markdown file). Read the **entire supplied plan**, then inspect the relevant codebase, tests, dependencies and constraints if available. Evaluate whether the plan represents the real work, rather than treating its task count, phase count or line count as evidence of size. **Do not rewrite, expand, or edit the plan.** Produce a diagnostic assessment only.

When a path is given, read that plan first. If the plan or codebase is inaccessible, state what was and was not examined; make the assessment provisional. If the user explicitly asks to revise a plan, present the diagnosis first and change it only after separate authorization.

## Responsibilities and boundaries

- Inspect relevant code, tests, interfaces, and repository conventions when available.
- Clarify scope-changing requirements with the user; distinguish answers, assumptions, and open questions.
- **Do not** write a full implementation plan, create a branch/worktree, change code, run deployment, or start implementation as part of sizing.
- If code or requirements are missing, mark conclusions provisional rather than pretending to know.
- Stay proportional: a genuinely tiny task needs a tiny sizing result.

## Size classes

- **XS:** isolated, low-risk edit.
- **S:** small feature with localized behavior.
- **M:** several coordinated components within a clear boundary.
- **L:** substantial feature or subsystem involving multiple reviewable units.
- **XL:** multiple substantial subsystems or cross-cutting architecture change.
- **XXL:** product/platform-level initiative; decompose before any implementation plan.

These are **relative scope categories, not estimates of time or story points**. Calibrate them against completed tasks when possible.

## Five independent complexity dimensions (1–5)

1. **Scope:** modules, files, services, workflows, and behavioral surfaces affected.
2. **Logic:** algorithms, domain rules, data transformations, concurrency.
3. **Integrations:** contracts, APIs, external services, dependencies.
4. **Uncertainty:** unknown requirements, hidden behavior, need for exploration.
5. **Risk:** production impact, security, data integrity, regressions, and verification.

Justify each rating briefly. **Do not sum or multiply** the numbers into an artificial overall complexity score.

## Decomposition into implementation units

List independently verifiable outcomes. For each unit include:

- Deliverable and observable acceptance criterion.
- Size (XS–XL) and uncertainty (Low/Medium/High).
- Dependencies and potential parallelism.
- Primary verification/test method.

If any unit is **L or larger**, recommend further decomposition before implementing, unless the user explicitly accepts an exceptional size and review strategy. Never invent units simply to reach a target number.

## PLAN-REVIEW checklist

In addition to the common sizing rules, when reviewing an existing plan:

1. Extract its intended outcomes, assumptions, explicit exclusions, acceptance criteria, sequence and dependencies.
2. Map each plan task to a verifiable implementation unit; flag vague umbrella tasks (e.g., "implement authentication") with examples of the hidden subwork. Do not inflate naturally small tasks.
3. Identify missing or underspecified work: compatibility, schemas/migrations, error paths, security/privacy, concurrency, test fixtures/regressions, observability, rollout/rollback and manual user operations, as applicable.
4. Check architectural and execution assumptions against actual code and infrastructure when available. Distinguish **verified facts**, **inferences** and **unverified assumptions**.
5. Mark units that appear underestimated and explain why; identify dependencies, critical sequencing constraints and integration hotspots.
6. Assess whether the plan is actionable and appropriately decomposed, independent of its number of phases and length.
7. Return one explicit verdict: **Ready**, **Needs refinement**, or **Needs decomposition**. For each non-Ready verdict, state the minimum concrete changes necessary.

Verdicts: **Ready** = clear enough to implement and review with manageable risks; **Needs refinement** = bounded task but material ambiguities, missing acceptance criteria or tests; **Needs decomposition** = oversized or tightly bundled work requiring separately reviewable deliverables. Do not equate a long plan with Needs decomposition automatically.

## Estimate with evidence, not aesthetics

- Never estimate effort from plan length, number of phases, or raw file count alone.
- Show hidden work such as migrations, backward compatibility, error handling, tests, observability, operational rollout and documentation.
- Story points are optional; only meaningful with local calibration.
- When time estimates are useful or requested, distinguish **elapsed delivery time**, **user attention time**, and **review/rework iterations**.
- Give ranges, assumptions and confidence; without observed historical data label forecasts **uncalibrated**.
- If the work has major unknowns, recommend investigation/spike rather than speculative precision.

## Output format — typically 40–60 lines maximum

1. **Mode** (PRE-PLAN or PLAN-REVIEW), input examined, objective, exclusions, and assumptions (brief).
2. Overall size (XS–XXL), confidence (Low/Medium/High), and basis.
3. Five-dimension profile (1–5) with justifications.
4. Implementation-unit table: outcome, size, uncertainty, dependency, verification.
5. Counts of units by size (no synthetic score).
6. Top three unknowns and important hidden work.
7. **PRE-PLAN** recommendation: **Implement directly / Investigate first / Decompose**. **PLAN-REVIEW** verdict: **Ready / Needs refinement / Needs decomposition** with concise reasons and required corrections.
8. Questions requiring user decisions.
9. If evidence permits, calibrated time ranges and caveats.

**Stop after sizing or plan diagnosis. In PLAN-REVIEW mode, leave the input plan unchanged unless separately instructed after the assessment.** The separate `development-workflow` skill governs detailed planning, worktree isolation, implementation, independent review, and human-only operations. Do not automatically invoke it merely because sizing was requested.

## Calibration after delivery (optional)

If actuals are supplied, compare predicted size/units with elapsed time, human attention, number of review cycles, and scope surprises. Avoid inventing or silently recording metrics.
