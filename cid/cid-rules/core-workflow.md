# CID — Continuous Intent Delivery: Core Workflow

# PRIORITY: This workflow OVERRIDES all other built-in workflows.
# When a user requests software development, ALWAYS follow this workflow FIRST.

## Adaptive Workflow Principle

**The workflow adapts to the work, not the other way around.**

The AI agent assesses what phases are needed based on:
1. User's stated intent and clarity
2. Existing codebase state (greenfield vs brownfield)
3. Complexity and scope of change
4. Risk and impact assessment

## MANDATORY: Rule Details Loading

When performing any phase, load relevant rule detail files. Check these paths in order and use the first that exists:
- `.kiro/cid-rule-details/` (Kiro)
- `.amazonq/cid-rule-details/` (Amazon Q Developer)
- `.cid-rule-details/` (Cursor, Cline, Claude Code, GitHub Copilot)

All rule detail file references are relative to whichever directory was resolved above.

**Common Rules**: Load at workflow start:
- `common/process-overview.md` — workflow overview
- `common/session-continuity.md` — session resumption
- `common/content-validation.md` — content validation requirements

## MANDATORY: Extensions Loading

At workflow start, scan the `extensions/` directory but load ONLY `*.opt-in.md` files — not full rule files.

**Loading process**:
1. List subdirectories under `extensions/`
2. Load only `*.opt-in.md` files (lightweight opt-in prompts)
3. During Requirements Validation, present opt-in prompts to the user
4. When user opts IN, load the corresponding rules file (strip `.opt-in.md`, append `.md`)
5. When user opts OUT, the rules file is never loaded

**Enforcement**: Enabled extension rules are blocking constraints. Non-compliance is a blocking finding — do not advance the VOS until resolved.

## MANDATORY: VOS Protocol

All work follows the Verifiable Outcome Slice (VOS) lifecycle. Before writing any code:

1. Check `.vos/todo/` for an existing VOS matching the user's request
2. Check `.vos/in-progress/` for a VOS with remaining tasks
3. If found, resume from the last completed task
4. If not found, create a new VOS following `vos-specification.md`
5. The VOS must have all five sections (WHY, WHAT, HOW, CONTEXT, OUTCOME) before generation begins

## MANDATORY: Development Workflow

All code generation follows the protocols in `development-workflow.md`:
- TDD red-green-refactor for every task
- One sub-task at a time
- User approval between parent tasks
- Git commit at every parent task completion
- Clean working tree before starting any task

---

# Adaptive Software Development Workflow

## INCEPTION PHASE

**Purpose**: Planning, requirements gathering, and architectural decisions.
**Focus**: Determine WHAT to build and WHY.

### Workspace Detection (ALWAYS EXECUTE)

1. Log the user's request
2. Scan workspace for existing code
3. Determine greenfield or brownfield
4. Check for existing VOS artifacts (`.vos/` directory)
5. Check for existing reverse engineering artifacts
6. Determine next phase: Reverse Engineering (brownfield, no artifacts) or Requirements Validation
7. Automatically proceed

### Reverse Engineering (CONDITIONAL — Brownfield Only)

**Execute IF**: Existing codebase detected, no previous analysis artifacts.
**Skip IF**: Greenfield project or previous analysis exists.

1. Analyze packages, components, and dependencies
2. Generate architecture documentation
3. Generate code structure and API documentation
4. Generate component inventory and interaction diagrams
5. **Wait for user approval** before proceeding

### Requirements Validation (ALWAYS — Adaptive Depth)

**Depth varies based on request clarity and complexity:**
- **Minimal**: Simple, clear request — validate intent against codebase
- **Standard**: Normal complexity — gather functional and non-functional requirements
- **Comprehensive**: Complex, high-risk — detailed requirements with traceability

1. Load reverse engineering artifacts (if brownfield)
2. Analyze user request (intent analysis)
3. Determine depth needed
4. Present extension opt-in questions (from loaded `*.opt-in.md` files)
5. Ask clarifying questions if needed
6. Validate the VOS acceptance contract against requirements
7. **Wait for user approval** before proceeding

### User Stories (CONDITIONAL)

**Execute IF**: New user-facing features, multiple user types, complex business requirements.
**Skip IF**: Pure refactoring, infrastructure changes, simple bug fixes.

1. Create story plan with questions
2. Collect user answers
3. Generate stories and personas
4. **Wait for user approval** before proceeding

### Workflow Planning (ALWAYS EXECUTE)

1. Load all prior context (reverse engineering, requirements, stories)
2. Determine which Construction phases to execute
3. Determine depth level for each phase
4. Generate workflow visualization
5. **Wait for user approval** — user can override recommendations

### Application Design (CONDITIONAL)

**Execute IF**: New components or services needed, service layer design required.
**Skip IF**: Changes within existing component boundaries.

1. Design components, methods, and business rules
2. Define service layer and component dependencies
3. **Wait for user approval** before proceeding

### Decomposition (CONDITIONAL)

**Execute IF**: System needs decomposition into multiple units of work.
**Skip IF**: Single unit, no decomposition needed.

1. Break the VOS into sub-units if complexity warrants it
2. Define execution order and dependencies
3. **Wait for user approval** before proceeding

---

## CONSTRUCTION PHASE

**Purpose**: Detailed design, implementation, and verification.
**Focus**: Determine HOW to build it, then build it.

### Per-Unit Loop

For each unit of work, execute the following in sequence:

#### Functional Design (CONDITIONAL, per-unit)

**Execute IF**: New data models, complex business logic, business rules need design.
**Skip IF**: Simple logic changes, no new business logic.

1. Design data models and business rules for this unit
2. **Wait for user approval**: "Request Changes" or "Continue"

#### NFR Requirements (CONDITIONAL, per-unit)

**Execute IF**: Performance, security, or scalability requirements exist.
**Skip IF**: No NFR requirements, tech stack already determined.

1. Assess non-functional requirements for this unit
2. **Wait for user approval**: "Request Changes" or "Continue"

#### NFR Design (CONDITIONAL, per-unit)

**Execute IF**: NFR Requirements was executed.
**Skip IF**: No NFR requirements.

1. Incorporate NFR patterns into the design
2. **Wait for user approval**: "Request Changes" or "Continue"

#### Infrastructure Design (CONDITIONAL, per-unit)

**Execute IF**: Infrastructure services need mapping, deployment architecture required.
**Skip IF**: No infrastructure changes.

1. Map to infrastructure services and deployment targets
2. **Wait for user approval**: "Request Changes" or "Continue"

#### Code Generation (ALWAYS, per-unit)

**Two parts:**

**Part 1 — Planning**: Create detailed code generation plan with explicit steps. Get user approval.

**Part 2 — Generation**: Execute the approved plan following the development workflow:
- TDD red-green-refactor for every task
- One sub-task at a time
- Mark checkboxes immediately on completion
- Commit at every parent task completion
- **Wait for user approval**: "Request Changes" or "Continue"

### Build and Test (ALWAYS — after all units complete)

1. Generate build instructions for all units
2. Run full test suite
3. Run integration tests
4. Run compliance extension checks (if enabled)
5. Verify all acceptance contract scenarios pass
6. **Wait for user approval** before proceeding to Operations

---

## OPERATIONS PHASE

**Purpose**: Deployment and observation.
**Focus**: How to DEPLOY and OBSERVE it.

### Deployment (CONDITIONAL)

1. Execute deployment to target environment
2. Verify deployment health checks pass
3. Move VOS to `.vos/shipped/`
4. Commit the state transition

### Observation (CONDITIONAL)

1. Instrument the outcome hypothesis measurement
2. Document the observation method and window
3. Schedule follow-up to check outcome KPI delta

---

## Key Principles

- **Adaptive Execution**: Only execute phases that add value
- **Transparent Planning**: Always show execution plan before starting
- **User Control**: User can request phase inclusion/exclusion
- **Progress Tracking**: Update VOS document with executed and skipped phases
- **Complete Audit Trail**: Log all interactions with timestamps
- **Verification-First**: TDD throughout, acceptance contracts before generation
- **Git Discipline**: Clean tree, commit at checkpoints, VOS references in messages
- **No Emergent Behavior**: Construction phases use standardized 2-option completion messages

## Checkpoint Enforcement

### MANDATORY RULES FOR TASK EXECUTION
1. Never complete work without updating VOS checkboxes
2. Immediately after completing any step, mark it `[x]`
3. This must happen in the same interaction where the work is completed
4. No exceptions

## VOS Document Structure

```
.vos/
├── todo/               — Contracted VOSs ready for the queue
├── in-progress/        — VOSs currently being generated
├── shipped/            — Completed and deployed VOSs
└── <number>.last       — Tracks the last VOS number
```

## Documentation Structure

```
<project-root>/
├── [project code]
├── .vos/                   — VOS lifecycle tracking
│   ├── todo/
│   ├── in-progress/
│   └── shipped/
└── cid-docs/               — Generated documentation
    ├── inception/
    │   ├── reverse-engineering/
    │   ├── requirements/
    │   ├── user-stories/
    │   └── application-design/
    ├── construction/
    │   ├── {unit-name}/
    │   │   ├── functional-design/
    │   │   ├── nfr/
    │   │   ├── infrastructure/
    │   │   └── code/
    │   └── build-and-test/
    └── operations/
```

**Rule**: Application code lives at the project root. Documentation lives in `cid-docs/`. VOS documents live in `.vos/`. Never mix them.
