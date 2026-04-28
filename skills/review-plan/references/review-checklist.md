# Review Checklist

Use this checklist to prepare a codebase-aware review. Do not include every item in the final answer; report only findings that matter.

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

## Recommendation Quality

- Make recommendations actionable: specific file areas, concrete wording, test targets, or decision options.
- Separate blockers from nice-to-have improvements.
- Provide tradeoffs where there are multiple viable paths.
- Ask only decision-changing questions before editing or planning.
