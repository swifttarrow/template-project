---
description: Implement a spec by executing task files sequentially with strict scope control
---

# Implement

Implement a milestone by executing its task files in order.

The current task file is the execution contract.

---

## Usage

```text
implement m1-server-foundation
implement m2-app-registration
```

If no spec is provided:
-> ask for one and list available specs under `docs/**/specs/`

---

## Spec Resolution

Resolve the spec name to exactly one directory using this order:

1. `docs/specs/{spec-name}/`
2. `docs/*/specs/{spec-name}/`

If no directory matches, STOP and report it.

If more than one directory matches, STOP and ask which one to use.

---

## Source Of Truth

Use sources in this precedence order:

1. The current task file
2. Files explicitly referenced by the current task
3. The milestone `README.md` for context, sequencing, and success criteria
4. Existing code patterns, only to stay consistent with the codebase

Do not use PRD, research, or architecture docs to add scope.

Only consult PRD, research, or architecture if the current task explicitly names them as required inputs.
Even then, use them only to clarify the task, never to expand it.

The milestone README describes intent, but it does NOT authorize changes beyond the task.

If any source conflicts with the current task, STOP and report the conflict.

---

## Before Starting Any Task

Read the entire task before making changes.

Create a short task contract from the task file:

- Purpose
- Inputs
- Outputs
- Allowed write set
- Dependencies
- Constraints
- Acceptance criteria
- Validation steps

If any of those are missing, ambiguous, contradictory, or reference paths that do not exist, STOP and report that the task is not executable as written.

Treat every task as if a stateless agent must be able to execute it with no hidden knowledge.
If the task depends on unstated context from a previous task, STOP and report the hidden dependency.

---

## Execution Rules

- Execute tasks strictly sequentially
- Finish the current task completely before reading ahead for implementation details
- Implement only what the current task requires
- Touch only files named in the task outputs, plus the smallest set of supporting files required to satisfy the task's acceptance criteria
- Do not search for alternate files or infer file locations when the task names specific paths
- Do not refactor, optimize, rename, clean up, upgrade dependencies, or extend behavior unless the task explicitly requires it
- Do not treat milestone goals as permission to fill in missing task details
- Do not continue past uncertainty; stop instead

If you discover that an additional file must be changed but that file is not clearly implied by the task's required outputs or acceptance criteria, STOP and ask before proceeding.

---

## Task Execution Loop

For each task:

1. Confirm dependencies and required artifacts exist
2. Confirm the allowed write set
3. Implement only the required changes
4. Validate every acceptance criterion with concrete evidence
5. Check for unintended side effects in touched areas
6. Mark the task complete only if validation passes
7. Proceed to the next task

Do not mark a task complete based on code inspection alone when the task requires runnable validation.

---

## Validation

After each task:

- Verify every acceptance criterion explicitly
- Confirm every expected output exists in the exact location specified
- Run the task's validation steps when possible
- Check for regressions caused by the touched files

If validation fails:
-> STOP and fix it before continuing

If validation cannot be run:
-> STOP and report exactly what could not be verified and why

Do not treat an unverified task as complete.

---

## Resuming

If partial progress exists:

- Determine completion from outputs plus validation evidence, not assumptions
- Treat a partially implemented or unverified task as incomplete
- Resume from the first task that is not fully validated

---

## On Mismatch

If reality does not match the task, STOP and report:

```text
Issue in Task [nnn]:

Blocking Type: [missing path | conflicting instruction | hidden dependency | acceptance gap | pre-existing failure]
Expected: [what the task specifies]
Found: [actual situation]
Why this matters: [impact on correctness or scope]
Smallest safe next step: [minimal options]

How should I proceed?
```

Do not proceed without clarification.

---

## Completion Rules

- Tasks are the only execution units
- Specs define behavior, but tasks authorize implementation
- A task is complete only when its outputs exist and its acceptance criteria are verified
- A milestone is complete only when every task is complete and milestone-level validation passes

When in doubt, prefer stopping over inferring.
