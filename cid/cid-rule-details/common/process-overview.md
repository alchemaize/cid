# CID Adaptive Workflow Overview

Technical reference for AI agents and developers.

## The CID Loop

CID operates as a continuous loop, not a linear pipeline:

```
INTENT → CONTEXT → GENERATION → VERIFICATION → OBSERVATION → (back to INTENT)
```

- **Intent** — Author the VOS (WHY, WHAT, outcome hypothesis)
- **Context** — Curate the context bundle (code, schemas, prior VOSs)
- **Generation** — AI agents execute the task plan via the adaptive workflow
- **Verification** — Acceptance contracts run, TDD cycles, compliance checks
- **Observation** — Shipped VOS instrumented in production, outcome hypothesis measured

Observations feed back into new intents. The loop is the unit of work.

## The Adaptive Workflow Inside Generation

Generation uses a three-phase adaptive workflow that scales depth to complexity:

Workspace Detection (always) → Reverse Engineering (brownfield only) → Requirements Validation (always, adaptive depth) → Conditional Phases (as needed) → Workflow Planning (always) → Code Generation (always, per-unit) → Build and Test (always) → Deploy and Observe (conditional)

## How It Works

- AI assesses request, workspace, and complexity to determine which phases are needed
- Always-execute phases: Workspace Detection, Requirements Validation, Workflow Planning, Code Generation, Build and Test
- Conditional phases: Reverse Engineering, User Stories, Application Design, Decomposition, per-unit design phases
- Depth adapts: Minimal (simple changes), Standard (normal), Comprehensive (complex/high-risk)

## VOS Lifecycle Integration

Every workflow execution operates on a Verifiable Outcome Slice (VOS):
- VOS enters GENERATING state when the workflow begins
- Inception phases validate and refine the VOS
- Construction phases execute the VOS task plan
- Operations phases ship and observe the VOS
- VOS moves to SHIPPED when all acceptance contract scenarios pass

## Stage Descriptions

**INCEPTION PHASE** — Planning and Architecture
- Workspace Detection: Analyze workspace state and project type (ALWAYS)
- Reverse Engineering: Analyze existing codebase (CONDITIONAL — brownfield only)
- Requirements Validation: Validate VOS against codebase and requirements (ALWAYS — adaptive depth)
- User Stories: Create user stories and personas (CONDITIONAL)
- Workflow Planning: Create execution plan (ALWAYS)
- Application Design: Component and service design (CONDITIONAL)
- Decomposition: Break into units of work (CONDITIONAL)

**CONSTRUCTION PHASE** — Design, Implementation, Build and Test
- Functional Design: Business logic design per unit (CONDITIONAL)
- NFR Requirements: Non-functional requirements and tech stack (CONDITIONAL)
- NFR Design: NFR patterns and logical components (CONDITIONAL)
- Infrastructure Design: Map to infrastructure services (CONDITIONAL)
- Code Generation: TDD-first, incremental, per-unit (ALWAYS)
- Build and Test: Full test suite, compliance checks (ALWAYS)

**OPERATIONS PHASE** — Deployment and Observation
- Deployment: Ship to target environment (CONDITIONAL)
- Observation: Instrument outcome hypothesis measurement (CONDITIONAL)

## Key Principles

- Phases execute only when they add value
- Verification is upstream, not downstream — acceptance contracts before generation
- TDD red-green-refactor for all code generation
- One sub-task at a time, user approval between parent tasks
- Git discipline: commit at every checkpoint, clean tree before starting
- VOS is the source of truth — if reality diverges from the plan, update the plan
