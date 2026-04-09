# Session Continuity

When resuming a CID workflow in a new session:

## 1. Check for Existing State

1. Look for `.vos/in-progress/` — any VOS here has unfinished work
2. Read the VOS document and check which tasks are marked `[x]`
3. Verify completed tasks against git history (`git log --stat`)
4. If inconclusive, ask the user which tasks are actually complete

## 2. Resume Protocol

1. Load the VOS document
2. Load the context bundle referenced in the CONTEXT section
3. Identify the next uncompleted task
4. Verify the working tree is clean (`git status`)
5. Ask the user: "Resuming VOS #<number> from Task <next-task>. Ready to continue?"
6. Proceed only after user confirmation

## 3. Context Reset

When resuming in a new session, the AI agent starts with a fresh context window. This is a feature, not a bug — it prevents context drift that degrades output quality over long sessions.

Load only:
- The VOS document (current state with checkboxes)
- The context bundle files referenced in the VOS
- The development workflow rules
- Any extension rules that are enabled

Do not attempt to reconstruct the full history of the previous session. The VOS document and git history are the source of truth.

## 4. State Recovery

If the VOS document and git history are inconsistent:
- Git history is authoritative for what code exists
- VOS checkboxes are authoritative for what the team considers complete
- If they disagree, ask the user to reconcile before proceeding
