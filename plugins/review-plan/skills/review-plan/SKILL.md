---
name: review-plan
description: >-
  Review documents, specs, proposals, or development plans before implementation.
  Use for the ordered workflow: read the artifact, research current context with
  Exa MCP, inspect related project code, produce a strict graded review with
  fixes, wait for fix/skip feedback, then remind or convert the artifact into a
  traceable execution plan with step folders, harness acceptance, Chinese
  commits, and context-clear handoffs.
---

# Review Plan

## Overview

Use this skill to review a document or development plan rigorously before implementation. The workflow must combine the supplied artifact, current Exa MCP research, and related project code when a project exists.

Follow the ordered workflow below. Do not modify the artifact until the user chooses `修正`. Do not create a split execution plan until the user confirms splitting or the original request explicitly asked for splitting after review feedback.

## Ordered Workflow

1. **Read the artifact**: identify and read the given document, spec, proposal, development plan, or implementation plan. If the artifact path, target repository, desired outcome, or review depth is unclear and cannot be inferred safely, ask concise questions and wait.
2. **Research with Exa MCP**: perform relevant, current, and detailed research before judging the plan. Prefer `web_search_advanced_exa` for official docs, release notes, standards, security guidance, and comparable implementations. Use `get_code_context_exa` for current library/API examples. If Exa MCP is unavailable, state that the required research path is blocked and ask whether to continue with fallback research.
3. **Read project code if present**: inspect local project context when a project exists. Read local agent instructions, repository docs, manifests, architecture notes, affected source files, relevant tests, and current git status. State clearly if no project context is available.
4. **Synthesize the strict review**: evaluate the artifact against the project code and Exa research. Separate sourced evidence from inference. Load `references/review-checklist.md` when preparing findings.
5. **Present graded interactive findings**: output findings by severity, include concrete modification suggestions, and ask decision-changing questions only.
6. **Wait for feedback**: ask the user to choose how to handle findings: `修正` or `放弃修正`. Wait before editing. If the user chooses `修正`, edit only the reviewed artifact unless broader edits are explicitly requested. If the user chooses `放弃修正`, leave the artifact unchanged and continue to the split reminder.
7. **Remind about plan splitting**: after the feedback gate, remind the user that a split execution plan must be detailed, executable, stored in subdirectories, trace progress per step and feature point, include strict harness acceptance for every step, loop on failures until acceptance succeeds or a true blocker is recorded, auto-commit each accepted step with a detailed Chinese commit message, clear context after each commit before the next step, and run without per-step confirmation.
8. **Split when confirmed or already requested**: after confirmation, or when the original request already required splitting after review feedback, load `references/execution-protocol.md` and `references/harness.md`, create the plan directory, and report that splitting is complete.

## Review Workflow

Use this checklist while executing the ordered workflow:

- Summarize the artifact's goal, scope, assumptions, non-goals, deliverables, success criteria, and acceptance criteria.
- Build project context with targeted exploration:
   - locate the repo root and read local instructions such as `AGENTS.md`, `README*`, architecture docs, and contribution docs;
   - inspect manifests such as `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`, or equivalent;
   - use `rg --files` and focused `rg` searches to locate code paths affected by the plan;
   - read representative implementation and tests rather than relying on filenames;
   - run safe read-only commands such as `git status --short` and dependency listing when helpful.
- Record Exa source names or URLs in the review, and distinguish `依据` from `推断`.
- Write the review in Chinese unless the user requested another language.

## Review Output

Use this structure unless the user requests a different format:

- `结论`: overall judgment, readiness level, and whether implementation should proceed.
- `严重等级`: classify findings as `P0 阻塞`, `P1 高风险`, `P2 中风险`, or `P3 改进`.
- `P0 阻塞`: contradictions, missing decisions, unsafe assumptions, or acceptance gaps that prevent implementation.
- `P1 高风险`: important issues in scope, architecture, compatibility, security, data, tests, operations, UX, or rollout.
- `P2 中风险`: quality or maintainability issues that should be fixed before execution when practical.
- `P3 改进`: refinements that improve clarity or execution quality without blocking progress.
- `建议修改`: concrete wording, structure, or technical changes.
- `项目现状依据`: code/docs/files inspected and how they affect the review.
- `Exa 调研依据`: current external sources used and the relevance of each.
- `待确认问题`: only questions that change the decision or implementation path.
- `请选择如何处理审查结果`: ask the user to choose `修正` or `放弃修正`, and whether to proceed to plan splitting after that choice.

## Editing The Artifact

After the user confirms edits:

1. Keep changes narrowly scoped to the reviewed artifact unless the user approves broader edits.
2. Preserve the author's intent while fixing gaps, contradictions, ambiguous sequencing, missing acceptance criteria, and unsupported assumptions.
3. Show the changed files and summarize the substantive changes.
4. If the artifact is versioned, do not commit unless the user explicitly requested commits for this editing phase.

## Splitting Into An Execution Plan

After the feedback gate and split confirmation, load `references/execution-protocol.md` and `references/harness.md`, then create a plan directory that tracks:

- one parent directory for the overall plan;
- separate subdirectories for each step or feature point;
- progress, decisions, research sources, acceptance criteria, validation commands, and handoff notes;
- harness acceptance criteria for every step, with a repair-and-retry loop until success or a documented blocker;
- a commit protocol that commits after each accepted step or feature with a detailed Chinese commit message;
- a context-clear handoff after each commit before the next step begins;
- autonomous execution rules that avoid asking for the next-step confirmation once the execution plan has been approved.

If the user asks to execute the approved plan, execute steps in order without per-step confirmation. For each step, load its subdirectory, implement only its scope, run the harness, fix failures until accepted or blocked, commit only that step's relevant changes, update tracking files, write a handoff, then clear context before the next step when the runtime exposes a context-clear mechanism. If no such mechanism exists, record the limitation in the handoff and tell the user the exact file and next step needed for a fresh context.
