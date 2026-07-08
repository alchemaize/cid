# VOS Specification

A Verifiable Outcome Slice (VOS) is a self-contained document that describes a single unit of user-observable value. It is the atomic unit of work in CID. A VOS is an intent-as-artifact: the thing humans author, review, version, and argue over. Code is the predictable output that falls out when the VOS is right.

Every VOS is a markdown file with one H1 header as the title, followed by five required H2 sections. A VOS describes exactly one unit of user-observable value: not a sprint's worth, not a theme, not an epic. If the WHY takes longer than a paragraph to state, the slice is too large and you split it. A two-page VOS is usually two VOSes.

## 1. VOS Title and Numbering

### 1.1 Title Format

```
# <number> - <type> (<domain>): <intent>
```

Types: `Feature`, `Enhancement`, `Bug`, `Refactor`, `Infrastructure`, `Security`.

### 1.2 File Naming

```
<number>_<type>_<domain>_<intent-dashed>.md
```

## 1.3 Sizing: One Verifiable Outcome

A VOS is one verifiable outcome, not a feature area. Before authoring, apply three tests. If any fails, split the slice:

1. **One-paragraph WHY.** If the intent needs more than a paragraph to state, it is more than one slice.
2. **Single-behavior WHAT.** If the acceptance contract holds scenarios for several independent behaviors (for example CRUD *and* a status workflow *and* a conversion *and* a routing rule), each behavior is its own VOS.
3. **Would it ship alone?** If the pieces were, or would be, shipped, verified, or superseded independently, they are separate VOSes.

The unit is an outcome, not a task. CRUD is one outcome; do not split it into create/read/update/delete. A status workflow, a record conversion, and an assignment rule are three outcomes, not one "management" VOS. When two candidate slices always ship together and are never verified apart, they are one VOS; when they can move through the lifecycle on their own, they are two.

## 1.4 The Backfill Trap

Backfilling a finished product tends to produce epics. Building incrementally keeps a VOS small automatically, because each ship is a single verifiable outcome and the act of shipping enforces the size. The end-state view of a mature domain has lost that signal: a domain that was actually built as a dozen ships over months collapses into one tempting "domain" VOS.

When backfilling, decompose each domain to outcome level using the tests in 1.3, exactly as you would size new work. You are not reconstructing the precise history, and you should not invent a ship sequence that never happened (see the Lifecycle Trace rule in 2.6); you are recording the shipped outcomes at the granularity you would author them at today. The reliable tell that a backfilled VOS is too big is a WHAT whose scenarios span several unrelated behaviors. Split it until each VOS carries one.

## 2. The Five Required Sections

Every VOS has five sections in order: WHY, WHAT, HOW, CONTEXT, OUTCOME. Each section has a **default role owner** on the pod, shown in the table below. The mapping says which of the three roles is accountable for the section, not which named person does the work. One human can wear more than one hat: a solo operator is Intent Engineer, AI Orchestrator, and Verification Owner all at once. Who actually authored and moved each stage is recorded per VOS in the Lifecycle Trace (Section 2.6), not fixed by the role table. Each section is short. Length budgets below are guidance, not hard limits, but a section that runs long is usually a signal the slice is too big.

| Section | Answers | Default role owner | Budget |
|---------|---------|--------------------|--------|
| WHY | The business or user outcome this slice serves | Intent Engineer | 2 to 4 lines |
| WHAT | The behavior, as an acceptance contract (Gherkin) | Intent Engineer + Verification Owner | 10 to 20 lines |
| HOW | Technical constraints and approach, then a task plan; not an implementation spec | AI Orchestrator | approach in 3 to 5 lines |
| CONTEXT | The minimum set of files and references the generator needs | Intent Engineer | 4 to 15 files, each with a stated reason |
| OUTCOME | The measurable change in a user or business metric the slice predicts | Intent Engineer | 1 to 3 lines |

### 2.1 WHY: Intent Statement

Owner: Intent Engineer. Two to four lines. The WHY answers three questions: what business problem the slice solves, who benefits and how, and how the slice connects to the stream's strategic intent. If you cannot state the intent in one paragraph, the slice is too large: split it. A WHY that could not survive a skeptical reader asking "so what" is not done.

Optional subsections:
- **Depends On** — VOS numbers this one requires to be shipped first
- **Enables** — VOS numbers that depend on this one
- **Supersedes** — VOS number this one replaces as the current reference for a feature domain (see Section 5). When present, update `CURRENT.md` to point to this VOS instead of the superseded one.

### 2.2 WHAT: Acceptance Contract

Owner: Intent Engineer, with the Verification Owner as a required second reader. The WHAT is the acceptance contract, written in Gherkin, before any code exists. Every scenario must be independently verifiable.

Rules:
- At least one scenario for the primary success path
- Error and edge case scenarios required for user input, external data, or state transitions
- Specific enough for automated tests
- If not expressible in Gherkin (for example, pure infrastructure), use structured acceptance criteria with clear pass/fail checks

Most specifications fail here. A WHAT with only the happy path under-specifies the contract: the generated code will pass that one scenario and fail every case the scenario did not cover.

### 2.3 HOW: Task Plan

Owner: AI Orchestrator. The HOW names the technical approach at the level of what systems get touched and what patterns get reused, not how any individual function gets written. Keep the approach to a few lines. Its job is to keep the generation layer inside the architectural lanes the team already owns; negative instructions ("do not add new data-model tables") often matter more than positive ones. If the HOW contains pseudocode, it has drifted into the generation layer's work.

The approach is followed by the step-by-step task plan, proposed by the AI Orchestrator and approved by the Intent Engineer before execution.

```markdown
### Tasks
* [ ] 1.0 - <Parent task>
  - [ ] 1.1 - <Sub-task>
  - [ ] 1.2 - <Sub-task>
```

Rules:
- Tasks follow Gherkin scenario order where applicable
- Each sub-task completable in one interaction
- TDD red-green-refactor is implicit; no need to spell out TDD steps
- Parent tasks require user approval; sub-tasks proceed without separate approval
- Checkboxes marked `[x]` immediately upon completion
- **Every task and sub-task in the plan MUST be completed before the VOS can ship. No partial builds. If a task is written, it gets built. If it should not be built, remove it from the plan before starting, not after.**

### 2.4 CONTEXT: Context Bundle

Owner: Intent Engineer. A curated list of files, schemas, docs, and prior VOS artifacts the generation layer needs. Each entry carries a short stated reason for why the generator needs to see it.

Rules:
- Curated, not scraped: only what is relevant. The right number of files is almost always between four and fifteen. Under four and you are probably missing something. Over fifteen and you are including areas you needed to understand but the generator does not need to produce correct output.
- Reference prior VOSs that established patterns
- Include architecture decision records when touching architectural boundaries

### 2.5 OUTCOME: Outcome Hypothesis

Owner: Intent Engineer. One to three lines. A falsifiable prediction about the world, not an aspiration. An OUTCOME that cannot be falsified is a wish. State what you measure, the baseline, the target, how you observe it, and the window.

```markdown
**Metric:** <what you measure>
**Current:** <baseline>
**Expected:** <target>
**Measurement:** <how you observe it>
**Observation window:** <timeframe>
```

The VOS is not done when the code ships. It is done when the outcome has or has not happened.

### 2.6 Lifecycle Trace

A recommended trailing section that records who did what, and when, as the VOS moved through the six states. This is the provenance log. The role table above says which hat is accountable for a stage; the Lifecycle Trace names the actual human who wore it. For a solo operator the Actor column is the same name down the whole trip; for a full pod it shows the handoffs.

```markdown
## Lifecycle Trace

| Date | State | Trigger | Actor |
|------|-------|---------|-------|
| 2026-01-29 | DRAFTED | WHY + draft WHAT + OUTCOME written | Casey |
| 2026-01-30 | CONTRACTED | Contract reviewed and signed; HOW added | Casey |
| 2026-01-30 | QUEUED | Context bundle complete, capacity available | Casey |
| 2026-01-30 | GENERATING | Generation run begins | Casey |
| 2026-01-30 | VERIFYING | Candidate lands, harness runs | Casey |
| 2026-01-30 | SHIPPED | Harness passes, signed | Casey |
| 2026-02-01 to 28 | (post-ship) | OUTCOME measurement window | Casey |
```

Rules:
- One row per state entered, in order. Reverts (VERIFYING back to CONTRACTED or DRAFTED) add a row too; they are data, not shame.
- The Actor is the person, not the role. The role table (Section 2) is the default hat; the trace is the record of fact.
- A post-ship row for the OUTCOME measurement window is optional but useful.
- Backfilled VOSes (documenting already-shipped work) record a single honest row noting the backfill date and the real ship reference, rather than inventing a sequence that never happened.

## 3. The Seven First Principles

CID rests on seven numbered principles. Every rule in this spec traces back to one of them.

| # | Principle | What it replaces |
|---|-----------|------------------|
| 1 | Intent is the artifact. Code is the exhaust. | Code as the durable record |
| 2 | Verification is the trust anchor. | Downstream QA as a safety net |
| 3 | Flow over cadence. | Sprints, release trains, calendar-driven delivery |
| 4 | Fund streams, not projects. | Project-based budgeting and fixed-scope plans |
| 5 | Three roles, not thirty. | The expanded role chart of SAFe and its descendants |
| 6 | Measure outcomes, not activity. | Velocity, story points, ceremony attendance |
| 7 | Ceremony is a tax. | Standups, sprint reviews, retrospectives, PI planning |

The three pod roles referenced throughout this spec are the Intent Engineer, the AI Orchestrator, and the Verification Owner (principle 5).

## 4. VOS Lifecycle: Six States

A VOS is state-tracked through a six-state lifecycle. The lifecycle is a directed graph with two reverse edges, not a cycle. A VOS enters at DRAFTED, moves forward, and exits the graph when the Verification Owner signs at SHIPPED. SHIPPED is terminal and immutable.

```
DRAFTED → CONTRACTED → QUEUED → GENERATING → VERIFYING → SHIPPED
```

The states are visible: they live in the filesystem. A VOS in a given state sits in a given folder. Every state transition is committed to git. The filesystem and git history are the ticketing system and the audit log.

- **DRAFTED** — WHY and a rough WHAT exist; CONTEXT not curated; HOW not specified. The AI Orchestrator has not reviewed and the Verification Owner has not signed.
- **CONTRACTED** — The acceptance contract is signed by the Verification Owner and is now binding. HOW is written, CONTEXT is bundled, OUTCOME is stated. Ready for generation.
- **QUEUED** — Scheduled into the pod's active work queue, awaiting generator capacity. Not all contracted VOSes are in the queue at once; queueing is how the pod controls work-in-progress.
- **GENERATING** — The generation layer is producing code and tests against the contract. The AI Orchestrator is watching.
- **VERIFYING** — Generation is complete. The acceptance contract is being run and the Verification Owner is reviewing. Blocking findings are raised here.
- **SHIPPED** — The code is in production. The VOS is now immutable and part of the durable record. The OUTCOME measurement window begins.

### 4.1 State Machine

Seven transitions: five forward, two reverse. A VOS never reverts states silently. Reverts are data, not shame.

| From | To | Trigger | Owner of the transition |
|------|-----|---------|-------------------------|
| (new) | DRAFTED | Intent Engineer writes WHY, WHAT, HOW, CONTEXT, OUTCOME | Intent Engineer |
| DRAFTED | CONTRACTED | Acceptance contract reviewed, all scenarios verifiable, contract signed | Verification Owner |
| CONTRACTED | QUEUED | Context bundle complete, stream capacity available | AI Orchestrator |
| QUEUED | GENERATING | Orchestrator starts the generation run against the contract | AI Orchestrator |
| GENERATING | VERIFYING | Generation run produces a candidate, harness run begins | AI Orchestrator |
| VERIFYING | SHIPPED | Harness passes, compliance extensions clear, Verification Owner signs | Verification Owner |
| VERIFYING | CONTRACTED | Contract gap: the failing scenario was not specified well enough to regenerate against | Verification Owner, back to Intent Engineer |
| VERIFYING | DRAFTED | Intent gap: the VOS was mis-scoped; the intent did not survive contact with the work | Verification Owner, back to Intent Engineer |

The reverse edges are what make verification real. A VERIFYING-to-CONTRACTED revert is a contract failure: the contract gets rewritten before the pod tries again. A VERIFYING-to-DRAFTED revert is an intent failure: the pod steps back and rethinks what it was trying to accomplish. Both are normal, and both must be inexpensive to recover from. If sending work backward feels costly, teams quietly stop doing it and verification decays into a rubber stamp.

### 4.2 Folder Mapping

The six state folders are numbered `1` through `6` so the pipeline reads in order on disk and anyone can follow a VOS along its trip at a glance. `superseded/` is unnumbered because it sits off the linear flow (see Section 5).

```
.vos/
├── 1-drafted/      — DRAFTED: authored, not yet contracted
├── 2-contracted/   — CONTRACTED: contract signed, ready for the queue
├── 3-queued/       — QUEUED: scheduled, awaiting generator capacity
├── 4-generating/   — GENERATING: code and tests being produced
├── 5-verifying/    — VERIFYING: harness running, Verification Owner reviewing
├── 6-shipped/      — SHIPPED: in production, immutable (+ changelogs)
├── superseded/     — Historical VOSs replaced by newer ones (+ changelogs); see Section 5
├── CURRENT.md      — Living index mapping each domain to its current VOS
└── <number>.last   — Tracks the last VOS number
```

A VOS moves from one numbered folder to the next as its state changes, and each move is a git commit. A pod may leave folders empty when it does not use a state (for example, a solo backfill of already-shipped work lands everything in `6-shipped/`), but the numbered structure always carries all six so a VOS has a home for whatever state it is in.

## 5. VOS Evolution

Shipped VOSs are immutable. They are the historical record of what was intended, built, and verified.

### 5.1 The Book Model: The Watching Layer

In the base CID model, a shipped VOS is terminal. Evolution happens through the **watching layer**: a parallel observation channel that measures whether each shipped VOS moved the metric it predicted. When observation surfaces actionable signal, that signal seeds a **new VOS, drafted fresh**, which takes its own one-way trip through the lifecycle. The watching layer does not feed the old VOS back through the pipeline. The old VOS is shipped and terminal.

### 5.2 Alchemaize Operational Extension: Supersession and Changelogs

Beyond the base model, Alchemaize adds an operational layer for tracking long-lived feature domains across many shipped VOSes. This extension never mutates a shipped VOS, so it stays consistent with the immutability rule in Section 5.1: it only moves immutable files between folders and maintains an index. Teams practicing base CID can ignore this section.

**Supersession is wholesale only.** A VOS is either fully superseded or it is not. There is no partial supersession.

- **Superseded** means: this VOS is no longer the authoritative reference for its domain; a newer VOS covers everything this one did and more.
- **NOT superseded** means: this VOS is still the authoritative reference for its domain, even if individual artifacts it delivered were later modified by other VOS work.

**Domain ownership, not file ownership.** Each VOS owns a domain. If VOS #8 (UX) modifies a file originally created by VOS #5 (API), that does not supersede #5. VOS #5 still owns the API domain. The change is logged in #8's changelog (because #8 did the work) and optionally cross-referenced in #5's changelog. Neither VOS is superseded.

**Consolidation supersedes within a domain only.** A consolidating VOS supersedes all prior VOS in the same domain. It never supersedes VOS in other domains, even if those VOS created files the consolidated domain now uses.

### 5.3 When to Create a New VOS vs. a Changelog Entry

**Full VOS** — new capabilities that do not exist yet:
- A new feature domain, integration, or screen
- Intent unclear enough that writing it down first saves time
- Significant architectural changes

**Changelog entry** — iterations on shipped features:
- Bug fixes, UX tweaks, polish
- "Make this feel better" iteration cycles
- Iterating 3 to 10 times to get something right
- Changes where you already know what the feature does

### 5.4 Changelog Convention

Each shipped VOS has a companion changelog at `.vos/6-shipped/<number>.changelog.md`.

```markdown
# Changelog: VOS #<number> — <title>

## <date> — <brief summary>
- What changed and why (one line per change)
- Files modified: `path/to/file.ts`
```

Rules:
- No Gherkin, no task plans, no ceremony: just dated entries
- Append after iteration sessions, bug fixes, or polish
- One entry per logical change, not per commit
- Include file paths so the PM tool can track what moved

### 5.5 Consolidation Trigger

When a changelog reaches **10+ entries** or a **major rework** fundamentally changes how the feature works:

1. Write a new VOS representing the current complete state
2. Add `Supersedes: #N` in the WHY section
3. Update `CURRENT.md` to point to the new VOS
4. Move the old VOS and its changelog to `superseded/`
5. The new VOS starts with a fresh empty changelog

### 5.6 CURRENT.md

`.vos/CURRENT.md` is the living index that maps each feature domain to its current VOS. It answers: what is the latest truth about feature X? This is the file a PM tool reads.

## 6. Quality Checklist

Before a VOS leaves DRAFTED for CONTRACTED:
- [ ] Intent statement is one paragraph, clear, specific, and survives "so what"
- [ ] Each of the five sections has its owner and stays within budget
- [ ] Acceptance contract has Gherkin scenarios for primary, error, and edge cases
- [ ] Context bundle references only relevant files and prior VOSs, each with a stated reason (4 to 15)
- [ ] Task plan has numbered tasks with checkboxes, ordered by dependency
- [ ] Outcome hypothesis states a falsifiable expectation with a measurement window
- [ ] VOS number is correct and the `.last` file is updated
