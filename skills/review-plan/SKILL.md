---
name: review-plan
description: Review documents, specs, proposals, or development plans before implementation. Use for codebase-aware plan critique, Exa MCP research, interactive clarification, approval-gated edits, and optional conversion into a traceable execution plan with per-step folders, progress tracking, Chinese commit messages, and context-clear handoffs.
---

# Review Plan

## Overview

Use this skill to review a document or development plan rigorously before implementation, combining the supplied artifact, the current project state, relevant code, and current external research. Do not modify the artifact or split it into an execution plan until the user explicitly approves that next action.

## Required Gates

1. Identify the artifact to review. If the artifact path, repository, desired outcome, or review depth is unclear and cannot be inferred safely, ask concise questions and wait.
2. Inspect project context when a project exists: read local agent instructions, repository docs, dependency manifests, architecture notes, relevant source files, tests, and current git status. State clearly if no project context is available.
3. Use Exa MCP for current related research whenever available. Prefer primary or authoritative sources for libraries, frameworks, standards, and security-sensitive claims. If Exa MCP is unavailable, state that the required research path is blocked and ask whether to continue with fallback research.
4. Present the review first. Include evidence from the artifact, codebase, and research. Do not edit files yet.
5. Ask the user to choose the next action after the review:
   - modify the original document or plan;
   - split it into a detailed execution plan;
   - do both, in a specified order;
   - stop after the review.
6. Wait for explicit confirmation before making edits, creating plan directories, or executing any implementation step.

## Review Workflow

1. Read the artifact and summarize its goal, scope, assumptions, and stated deliverables.
2. Build project context with targeted exploration:
   - locate the repo root and read local instructions such as `AGENTS.md`, `README*`, architecture docs, and contribution docs;
   - inspect manifests such as `package.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `pom.xml`, or equivalent;
   - use `rg --files` and focused `rg` searches to locate code paths affected by the plan;
   - read representative implementation and tests rather than relying on filenames;
   - run safe read-only commands such as `git status --short` and dependency listing when helpful.
3. Research current context with Exa MCP:
   - use `web_search_advanced_exa` for current best practices, official docs, release notes, standards, and comparable implementations;
   - use `get_code_context_exa` for library/API usage examples when the plan depends on specific frameworks or SDKs;
   - record source names or URLs in the review, and distinguish evidence from inference.
4. Load `references/review-checklist.md` when preparing findings.
5. Write the review in Chinese unless the user requested another language. Lead with the most important gaps and risks, then recommendations.

## Review Output

Use this structure unless the user requests a different format:

- `结论`: brief overall judgment and readiness level.
- `必须先解决`: blockers, contradictions, missing decisions, or risky assumptions.
- `主要不足`: important gaps in scope, architecture, implementation detail, testing, operations, UX, security, data, or rollout.
- `可改进点`: refinements that would improve quality without blocking progress.
- `建议修改`: concrete wording, structure, or technical changes.
- `项目现状依据`: code/docs/files inspected and how they affect the review.
- `Exa 调研依据`: current external sources used and the relevance of each.
- `待确认问题`: only questions that change the decision or implementation path.
- `请选择下一步`: ask whether to modify the artifact, split into an execution plan, do both, or stop.

## Editing The Artifact

After the user confirms edits:

1. Keep changes narrowly scoped to the reviewed artifact unless the user approves broader edits.
2. Preserve the author's intent while fixing gaps, contradictions, ambiguous sequencing, missing acceptance criteria, and unsupported assumptions.
3. Show the changed files and summarize the substantive changes.
4. If the artifact is versioned, do not commit unless the user explicitly requested commits for this editing phase.

## Splitting Into An Execution Plan

After the user confirms plan splitting, load `references/execution-protocol.md` and create a plan directory that tracks:

- one parent directory for the overall plan;
- separate subdirectories for each step or feature point;
- progress, decisions, research sources, acceptance criteria, validation commands, and handoff notes;
- a commit protocol that commits after each completed step or feature with a detailed Chinese commit message;
- a context-clear handoff after each commit before the next step begins.

If the user asks to execute the plan, execute only the current step from its subdirectory, verify it, commit it, write the handoff, then stop or trigger the environment's context-clear mechanism if one exists. If no context-clear mechanism is available, stop after the commit and tell the user exactly which handoff file to use in the next fresh context.
