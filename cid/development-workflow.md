# CID Development Workflow

This document defines the development protocols for executing VOS task plans. It governs how AI agents and human practitioners work through tasks incrementally with verification at every step.

## 1. Verification-First Development (TDD)

All code generation follows a strict TDD red-green-refactor cycle. This is not optional.

### 1.1 TDD Rules

1. Write the test first. It must fail (RED). The code under test either does not exist or returns a placeholder.
2. Write the minimal code to make the test pass (GREEN). No more than what the test requires.
3. Refactor for clarity, security, and maintainability without changing behavior. The test still passes.
4. During the cycle, run tests only for the file or function being tested — not the full suite.
5. Fix all compiler warnings and errors before proceeding to the next task.
6. Do not use mocks for internal modules or database access. Mock only external services (third-party APIs, email, webhooks).
7. Do not write dummy or sanity-check tests. Start with the primary success scenario, then cover alternate paths, error paths, and edge cases.

### 1.2 TDD Workflow Steps

For each task in the VOS task plan:

1. Create the module/function with a placeholder return (e.g., `throw new Error('TODO')` or equivalent).
2. Write one test for the primary success scenario.
3. Run the test — it must fail (RED) because the implementation is a placeholder. If it fails for any other reason (syntax, imports, compilation), fix those first.
4. Implement the minimal code to make the test pass (GREEN). The code must be clean, readable, and secure.
5. Refactor if there are opportunities to improve readability, performance, or security. Ask the user before refactoring if the change is non-trivial.
6. Repeat for remaining scenarios: alternate paths, error paths, edge cases.

## 2. Incremental Generation Protocol

Work is executed one sub-task at a time with user oversight.

### 2.1 Rules

- Never propose code changes for multiple sub-tasks in one interaction.
- Keep changes focused on the single sub-task being executed.
- Brief explanations — state what was done, not verbose details.
- If the user provides feedback, adjust and re-present before proceeding.
- If the user says "skip," skip that task or sub-task.
- If the user says "edit" or "refine," iterate until they approve.

### 2.2 Task Execution Protocol

For each task and sub-task in the VOS:

1. **Clean working tree.** Run `git status`. If uncommitted changes exist, ask the user how to proceed. Do not start work on a dirty tree.
2. **Ask before starting parent tasks.** Parent tasks (1.0, 2.0, etc.) require explicit user confirmation. Sub-tasks (1.1, 1.2, etc.) proceed without separate approval unless ambiguity arises.
3. **Execute one sub-task.** Propose code changes for exactly one sub-task.
4. **Mark completion.** Immediately mark the sub-task `[x]` in the VOS document when done.
5. **Task completion protocol.** When all sub-tasks under a parent task are `[x]`, execute the completion protocol (section 2.3).

### 2.3 Task Completion Protocol

When all sub-tasks of a parent task are complete:

1. Run the full test suite. All tests must pass with no warnings or errors.
2. Run any configured linting, type checking, or static analysis.
3. Stage all changes: `git add .`
4. Mark the parent task `[x]` in the VOS document.
5. Commit with a message referencing the VOS:

```
VOS-<number> Task <task-number>: <brief description>
```

Example: `VOS-12 Task 1.0: Create recurring invoice generation service`

6. Verify the working tree is clean before proceeding to the next parent task.

## 3. VOS State Management

### 3.1 Starting a VOS

1. Verify the VOS is in `.vos/todo/` and all five sections are complete.
2. Move the VOS to `.vos/in-progress/`.
3. Commit the move (this commit happens with the first task completion).

### 3.2 Resuming a VOS

1. Check for tasks already marked `[x]`.
2. Verify completed tasks against git history — if inconclusive, ask the user.
3. Resume from the last confirmed completed task.

### 3.3 Completing a VOS

1. All tasks and sub-tasks are `[x]`.
2. Full test suite passes.
3. All acceptance contract scenarios (Gherkin) pass.
4. Move the VOS to `.vos/shipped/`.
5. Commit: `VOS-<number>: Shipped — <VOS title>`

## 4. Git Discipline

- Every parent task completion is a commit.
- Every VOS state transition is a commit.
- The working tree is clean before starting any new task.
- Commit messages reference the VOS number.
- Never commit with failing tests.

## 5. Context Management

- At the start of each VOS, load the context bundle referenced in the CONTEXT section.
- At approval gates (between parent tasks), reset context if the AI tool supports it. This prevents context drift.
- If the agent's context window is filling up, summarize completed work and reload only the remaining tasks and relevant context.

## 6. Handling Failures

- If a test fails for reasons unrelated to the current task, fix it before proceeding. Do not accumulate broken tests.
- If the task plan is wrong (steps are incorrect or incomplete), update the VOS HOW section with the correct steps before continuing. The VOS is the source of truth — if reality diverges from the plan, update the plan.
- If the agent is stuck, say so. Do not guess or trial-and-error. Ask the user for guidance.
