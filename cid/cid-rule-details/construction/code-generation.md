# Code Generation — Detailed Steps

## Purpose
Generate code following the VOS task plan with TDD-first verification.

## Two Parts

### Part 1 — Planning
1. Load the VOS task plan
2. Load the context bundle
3. Load any enabled extension rules
4. Create a detailed code generation plan with explicit steps
5. Map each task to specific files and functions
6. Identify test files and test patterns from the context bundle
7. Present the plan to the user for approval
8. Do NOT proceed to Part 2 until the user approves

### Part 2 — Generation
Execute the approved plan following the development workflow:

**For each parent task:**
1. Ask user for confirmation to start
2. Execute each sub-task following TDD:
   - Create module/function with placeholder
   - Write test (RED)
   - Implement minimal code (GREEN)
   - Refactor if beneficial
3. Mark each sub-task `[x]` immediately on completion
4. When all sub-tasks complete:
   - Run full test suite
   - Run linting/type checking
   - Stage and commit: `VOS-<number> Task <N>: <description>`
   - Mark parent task `[x]`
5. Verify clean working tree

**Extension Compliance:**
- At each task completion, verify compliance with enabled extension rules
- Non-compliance is a blocking finding — fix before proceeding
- Log compliance status

**Completion:**
Present standardized 2-option message:
- A) Request Changes — describe what needs to change
- B) Continue to Next Stage

Do NOT create 3-option menus or other navigation patterns.

## Code Quality Standards

- Code must be clean, readable, and maintainable
- Follow existing patterns from the context bundle
- No commented-out code
- No TODO comments (the VOS task plan tracks remaining work)
- Security best practices (input validation, parameterized queries, etc.)
- Error handling for all external interactions
- Logging for observability

## Test Quality Standards

- Tests must be independent and repeatable
- No test interdependencies
- Clear test names that describe the scenario
- One assertion concept per test (multiple asserts are fine if testing one behavior)
- Use real dependencies where possible, mock only external services
- Test the behavior, not the implementation
