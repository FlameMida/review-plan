# Harness Template

Use this reference when creating the parent `harness.md` and each step-level `harness.md` in a split execution plan.

## Parent Harness

````markdown
# Harness

## Global Acceptance Rules

- Every step must pass its own harness before it can be marked `done`.
- Failed checks must be fixed within the current step scope and rerun.
- Do not weaken acceptance criteria only to make a failing step pass.
- If a failure cannot be fixed without a user decision, external approval, unavailable credential, or unrelated code change, mark the step `blocked`.

## Required Validation

| Check | Command or Method | Pass Criteria | Applies To |
| --- | --- | --- | --- |
| Repository status | `git status --short` | Only expected step files are changed before commit | Every step |
| Lint | `<command>` | Exits 0 | Steps touching linted code |
| Tests | `<command>` | Exits 0 and covers changed behavior | Steps touching behavior |
| Build/typecheck | `<command>` | Exits 0 | Steps touching buildable code |

## Retry Loop

1. Run all checks for the current step.
2. Record failures in the step `evidence.md`.
3. Fix failures within the current step scope.
4. Rerun failed checks and any checks affected by the fix.
5. Repeat until all checks pass or a true blocker is documented.

## Blocker Record

- Failing check:
- Command or method:
- Important output:
- Suspected cause:
- Why it cannot be fixed in this step:
- Required decision or external action:
````

## Step Harness

````markdown
# Harness

## Step Acceptance

| Item | Check | Pass Criteria | Failure Fix |
| --- | --- | --- | --- |
| <acceptance item> | <command/manual check> | <observable pass condition> | <what to change and rerun> |

## Validation Commands

```bash
<command 1>
<command 2>
```

## Manual Verification

- Required only if automation is unavailable.
- Record exact observations, screenshots, or inspected files in `evidence.md`.

## Retry Notes

- Fix failures within this step's affected files/modules.
- Rerun failed checks after each fix.
- Rerun broader checks when a fix can affect shared behavior.

## Blocker Criteria

Mark this step `blocked` only if:

- a required decision is missing;
- a required credential or external service is unavailable;
- the fix requires unrelated work outside this step;
- a destructive action would be needed and has not been approved.
````
