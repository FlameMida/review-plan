# Execution Protocol

Use this reference only after the user explicitly confirms that the reviewed document or development plan should be split into a detailed execution plan or executed.

## Plan Directory

Create one parent directory unless the user specifies another location:

```text
plans/<yyyymmdd>-<short-slug>/
  README.md
  progress.md
  decisions.md
  research.md
  commits.md
  handoff.md
  steps/
    001-<step-slug>/
      plan.md
      progress.md
      evidence.md
      handoff.md
    002-<step-slug>/
      plan.md
      progress.md
      evidence.md
      handoff.md
```

If the repository already has a planning convention, follow it and preserve the same tracking concepts.

## Parent Files

- `README.md`: plan goal, scope, non-goals, assumptions, step index, and definition of done.
- `progress.md`: table with step id, status, owner, start/end time if known, commit hash, validation, and notes.
- `decisions.md`: decision log with date, decision, options considered, rationale, and follow-up.
- `research.md`: Exa sources, project files inspected, and important inferences.
- `commits.md`: commit hashes, messages, step mapping, and verification summary.
- `handoff.md`: current state and the exact next step to load after context is cleared.

## Step Files

Each `steps/<id>-<slug>/plan.md` should include:

- objective;
- affected files or modules;
- prerequisites;
- implementation tasks;
- acceptance criteria;
- validation commands;
- rollback or recovery notes when relevant;
- expected commit message draft.

Each `progress.md` should track:

- `pending`, `in_progress`, `blocked`, `done`, or `skipped`;
- changes made;
- validation result;
- open questions;
- linked commit hash after completion.

Each `evidence.md` should record:

- commands run and important outputs;
- files changed;
- tests or checks passed/failed;
- screenshots or manual verification notes when relevant.

Each `handoff.md` should contain the minimal context needed for a fresh session:

- completed step summary;
- commit hash;
- remaining work;
- next step path;
- warnings about dirty worktree, skipped tests, or unresolved decisions.

## Splitting Rules

- Split by independently verifiable feature points or implementation steps.
- Keep each step small enough to review, test, and commit alone.
- Make dependencies explicit; do not place prerequisite work in a later step.
- Avoid steps that mix unrelated modules solely to reduce the number of commits.
- Include a validation command or manual verification method for every step.
- If a step needs a user decision, mark it `blocked` and ask before continuing.

## Execution Rules

When the user asks to execute the plan:

1. Load the parent `README.md`, `progress.md`, `handoff.md`, and the current step's `plan.md`.
2. Confirm no blocking decision is open for the current step.
3. Inspect the worktree with `git status --short`. Preserve unrelated user changes and avoid committing them.
4. Implement only the current step's scope.
5. Run the step's validation commands, or state exactly why a validation could not run.
6. Update the parent and step progress files, evidence files, commit log, and handoff files.
7. Commit only the current step's relevant changes.
8. After the commit, clear context before beginning the next step. If no automatic context-clear mechanism exists, stop and instruct the user to start a fresh context with the parent `handoff.md` and next step path.

## Commit Message Rules

Use Chinese, be detailed, and keep messages tied to one step or feature point. Use this shape:

```text
<类型>(<范围>): <中文一句话概述>

原因：
- <为什么需要这一步>

变更：
- <关键变更一>
- <关键变更二>

验证：
- <运行的测试或检查>
- <无法运行的验证及原因，如有>
```

Recommended `类型`: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`, `build`, `ci`, `perf`.

Example:

```text
feat(步骤001-认证状态): 接入会话过期后的统一跳转处理

原因：
- 原计划要求补齐认证失败场景，但现有前端仅处理显式退出。

变更：
- 在请求封装中识别会话过期响应并触发登录跳转。
- 为受保护页面补充会话过期状态的回归测试。

验证：
- npm test -- auth-session
- npm run lint
```
