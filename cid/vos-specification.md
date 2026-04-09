# VOS Specification

A Verifiable Outcome Slice (VOS) is a self-contained document that describes a single unit of user-observable value. It is the atomic unit of work in CID.

Every VOS is a markdown file with one H1 header as the title, followed by five required H2 sections. The document must be concise and precise — developers and AI agents prefer communication that is as direct as possible.

## 1. VOS Title and Numbering

### 1.1 Title Format

The title is the first line of the file. Format:

```
# <number> - <type> (<domain>): <intent>
```

Examples:
- `# 12 - Feature (Billing-Invoices): Add recurring invoice generation`
- `# 13 - Bug (Auth-Sessions): Fix token refresh race condition`
- `# 14 - Enhancement (Dashboard-Metrics): Add real-time cycle time widget`

Types: `Feature`, `Enhancement`, `Bug`, `Refactor`, `Infrastructure`, `Security`.

### 1.2 File Naming

```
<number>_<type>_<domain>_<intent-dashed>.md
```

Example: `12_feature_billing-invoices_add-recurring-invoice-generation.md`

## 2. Required Sections

### 2.1 WHY — Intent Statement

The motivation for this change. One paragraph maximum. Must answer:
- What business problem does this solve?
- Who benefits and how?
- How does this connect to the stream's strategic intent?

If you cannot state the intent in one paragraph, the slice is too large. Split it.

Optional subsections:
- **Depends On** — VOS numbers this one requires to be shipped first
- **Enables** — VOS numbers that depend on this one

### 2.2 WHAT — Acceptance Contract

The executable specification that defines "done." Written in Gherkin before generation begins. Every scenario must be independently verifiable.

```gherkin
Feature: <Feature name matching VOS title>

  <User story format: As a..., I want..., So that...>

  Background:
    Given <shared preconditions>

  Scenario: <Primary success path>
    When  <action>
    Then  <observable outcome>
    And   <additional verification>

  Scenario: <Alternate path or edge case>
    ...

  Scenario: <Error handling>
    ...
```

Rules:
- Every VOS must have at least one scenario for the primary success path
- Error and edge case scenarios are required for any VOS that handles user input, external data, or state transitions
- Scenarios must be specific enough that an automated test can be written directly from them
- If the WHAT cannot be expressed in Gherkin (e.g., pure infrastructure), use structured acceptance criteria with clear pass/fail checks

### 2.3 HOW — Task Plan

The step-by-step implementation plan. Proposed by the AI agent, reviewed and approved by the intent engineer before execution begins.

```markdown
### Implementation Context

<Brief context about architecture, patterns, and constraints>

### Tasks

* [ ] 1.0 - <Parent task description>
  - [ ] 1.1 - <Sub-task>
  - [ ] 1.2 - <Sub-task>
* [ ] 2.0 - <Parent task description>
  - [ ] 2.1 - <Sub-task>
  - [ ] 2.2 - <Sub-task>
```

Rules:
- Tasks follow the order of Gherkin scenarios where applicable
- Each sub-task is small enough to be completed and verified in one interaction
- Test tasks follow TDD red-green-refactor — no need to spell out TDD steps as sub-tasks (the development workflow enforces them)
- Parent tasks require user approval before starting
- Sub-tasks proceed without separate approval unless the agent encounters ambiguity
- Checkboxes are marked `[x]` immediately upon completion, in the same interaction

### 2.4 CONTEXT — Context Bundle

The curated set of files, schemas, docs, and prior VOS artifacts the AI agent needs. Assembled by the intent engineer.

```markdown
### Context Bundle

**Code:**
- `src/billing/invoice-service.ts` — existing invoice logic
- `src/billing/types.ts` — billing domain types
- `tests/billing/invoice.test.ts` — existing test patterns

**Schemas:**
- `docs/api/billing-api.yaml` — OpenAPI spec for billing endpoints

**Prior VOSs:**
- VOS #10 — Billing domain setup (shipped)
- VOS #11 — Invoice data model (shipped)

**Architecture:**
- Serverless-first, Lambda handlers, DynamoDB
- See `docs/architecture-decisions/adr-003-billing.md`
```

Rules:
- Context is curated, not scraped. The intent engineer selects what the agent needs.
- Include only what is relevant. More context is not better context — it is noise.
- Reference prior VOSs that established patterns the current VOS should follow.
- Include architecture decision records when the VOS touches architectural boundaries.

### 2.5 OUTCOME — Outcome Hypothesis

The measurable business or user impact this VOS is expected to produce.

```markdown
### Outcome Hypothesis

**Metric:** Monthly invoice processing time
**Current:** 4.2 hours average (manual process)
**Expected:** < 15 minutes (automated)
**Measurement:** Time delta between invoice trigger event and delivery confirmation in production logs
**Observation window:** 30 days post-ship
```

Rules:
- Not every VOS moves a business KPI — infrastructure and refactoring VOSs may state technical metrics (latency, error rate, deployment time)
- The hypothesis must be falsifiable — state what you expect to change and how you will observe it
- If you cannot state any expected outcome, question whether the VOS delivers user-observable value

## 3. VOS Lifecycle Tracking

### 3.1 Directory Structure

VOSs are tracked in a `.vos/` directory at the project root:

```
.vos/
├── todo/           — Drafted and contracted VOSs ready for the queue
├── in-progress/    — VOSs currently being generated
├── shipped/        — Completed and deployed VOSs
└── <number>.last   — Tracks the last VOS number (e.g., 14.last)
```

### 3.2 State Transitions

1. **Drafted → Contracted:** VOS moves from draft to `todo/` when all five sections are complete and reviewed
2. **Contracted → Queued → Generating:** VOS moves to `in-progress/` when an agent picks it up
3. **Generating → Verifying → Shipped:** VOS moves to `shipped/` when all acceptance contract scenarios pass and the code is deployed

Each state transition is committed to git before proceeding.

### 3.3 Numbering

The `.vos/<number>.last` file tracks the last VOS number. When creating a new VOS, increment by one and rename the file. The first VOS is `1`, so the initial file is `0.last`.

## 4. Optional Sections

These H2 sections are added when relevant:

- **CONSTRAINTS** — Hard constraints (regulatory, technical, organizational)
- **DESIGN DECISIONS** — Key decisions, rationale, and trade-offs
- **ALTERNATIVES CONSIDERED** — Other approaches and why they were rejected
- **ARCHITECTURE** — Architecture diagrams or decisions specific to this VOS
- **CHALLENGES & SOLUTIONS** — Post-implementation: what went wrong and how it was resolved

## 5. Quality Checklist

Before a VOS enters `todo/`:

- [ ] Intent statement is one paragraph, clear, and specific
- [ ] Acceptance contract has Gherkin scenarios for primary path, error paths, and edge cases
- [ ] Context bundle references only relevant files and prior VOSs
- [ ] Task plan has numbered tasks with checkboxes, ordered by dependency
- [ ] Outcome hypothesis states a measurable expectation with observation method
- [ ] VOS number is correct and `.last` file is updated
- [ ] Document is committed to git
