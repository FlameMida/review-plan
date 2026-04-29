# Review Checklist

Use this checklist to prepare a codebase-aware review. Do not include every item in the final answer; report only findings that matter.

## Severity Model

- `P0 阻塞`: the plan cannot be implemented responsibly until this is fixed. Examples: contradictory requirements, missing target artifact or repo, unsafe security/data assumptions, no acceptance mechanism for core behavior, impossible sequencing, or unhandled migration risk.
- `P1 高风险`: implementation is possible but likely to fail, regress, or require rework. Examples: mismatch with existing architecture, outdated API assumptions, missing tests for critical paths, unclear rollout or rollback, or incomplete data/auth/error handling.
- `P2 中风险`: implementation quality, maintainability, or reviewability is materially weakened. Examples: vague affected files, weak decomposition, incomplete edge cases, missing observability, or unclear ownership.
- `P3 改进`: useful improvements that do not block execution. Examples: wording clarity, examples, optional diagrams, naming refinements, or extra non-critical validation.

Each finding should include:

- severity;
- concise issue title;
- evidence from the artifact, project code, or Exa research;
- concrete impact;
- suggested correction;
- whether the suggestion should be applied if the user chooses `修正`.

## Artifact Quality

- State the goal, users, non-goals, constraints, and success criteria.
- Define current behavior, desired behavior, and migration path.
- Separate requirements from implementation ideas.
- Identify assumptions that need validation.
- Make dependencies, ownership, sequencing, and rollout explicit.
- Include acceptance criteria that can be tested.

## Project Fit

- Compare the plan to existing architecture, module boundaries, naming, style, and helper APIs.
- Check whether proposed files or abstractions duplicate existing functionality.
- Verify that data models, API contracts, routing, auth, state management, and build tooling match the current project.
- Note conflicts with local instructions, CI expectations, linting, tests, or deployment setup.
- Identify existing tests that should be extended and missing test layers.
- Confirm the plan respects the current worktree. Do not assume unrelated dirty files are available to edit or commit.

## Technical Risk

- Look for hidden migrations, backward compatibility concerns, feature flags, rollback needs, and operational impact.
- Check security, privacy, permission, injection, secrets, dependency, and supply-chain risks.
- Check performance, concurrency, caching, pagination, rate limits, and failure handling.
- Check observability needs: logs, metrics, traces, audit events, and alerting.
- Check accessibility, i18n/l10n, error states, loading states, and edge cases for user-facing work.

## Research Fit

- Prefer official docs, release notes, specs, and primary repositories.
- Verify current API signatures, configuration names, deprecations, and compatibility requirements.
- Compare plan assumptions against recent best practices or known pitfalls.
- Mark claims as `依据` when sourced and `推断` when inferred from code or context.
- For unstable or fast-moving technologies, include publication or access dates when they affect the recommendation.

## Recommendation Quality

- Make recommendations actionable: specific file areas, concrete wording, test targets, or decision options.
- Separate blockers from nice-to-have improvements.
- Provide tradeoffs where there are multiple viable paths.
- Ask only decision-changing questions before editing or planning.
- Present the post-review choice explicitly: `修正` applies selected recommendations to the artifact; `放弃修正` leaves it unchanged. In both cases, remind the user about execution-plan splitting requirements before creating any plan files.
