# CID/ELCID: The Complete Methodology for AI-Native Software Development

## Document Purpose

This is the comprehensive reference for the CID/ELCID methodology — the first software development framework designed from scratch for the assumption that AI does the implementation, humans do the intent and the verification, and the organization's job is to scale that pipeline without breaking it.

This document is structured for knowledge retrieval. It covers the full system: philosophy, team-level practice, enterprise governance, roles, metrics, artifacts, ceremonies, adoption path, compliance, and anti-patterns.

---

## What Is CID/ELCID?

CID/ELCID is a three-layer system for building software in the AI-native era:

- **CID (Continuous Intent Delivery)** is the team-level practice. It defines how a pod of humans and AI agents builds software through intent specification, adaptive generation, and continuous verification.
- **ELCID (Enterprise Layer for Continuous Intent Delivery)** is the governance framework. It defines how an enterprise funds, governs, and scales CID across many streams.
- **Catalyst** is Alchemaize's commercial enablement offering — consulting, compliance extensions, evaluator, workshops, and packaged methodology that make CID/ELCID adoption faster.

The relationship: CID is how teams build. ELCID is how enterprises scale it. Catalyst is how Alchemaize helps you get there.

CID and ELCID are open, vendor-agnostic methodologies. They work with any AI coding tool (Kiro, Claude Code, Cursor, Amazon Q Developer, GitHub Copilot, Cline), any cloud provider, any programming language.

---

## Why CID/ELCID Exists

Every dominant software development framework of the last 25 years — Waterfall, RUP, XP, Scrum, Kanban, SAFe, LeSS, Nexus — was designed around a single unspoken constraint: human coding throughput. Every ceremony, role, and artifact in those frameworks exists to coordinate the variable, expensive, error-prone process of humans typing code.

That constraint is gone. AI coding tools have increased implementation throughput by one to two orders of magnitude (10-40x). The bottleneck has moved permanently to two new places:

1. **Intent clarity** — how precisely humans can specify what to build
2. **Verification rigor** — how fast and thoroughly humans can prove the output is correct

The middle — the actual writing of code — is now a near-zero-cost operation. Frameworks that continue to optimize for the old middle are not just inefficient; they are actively harmful, because they impose human-paced coordination overhead on a system that no longer has a human-paced bottleneck.

CID/ELCID is built for the new constraint.

---

## The Seven First Principles

These are load-bearing constraints, not aspirations. Every practice, role, and artifact derives from them.

1. **Intent is the artifact. Code is the exhaust.** The thing humans author, version, review, and own is the intent specification. Code is generated from it and verified against it.

2. **Verification is the trust anchor.** Speed without verification is just faster debt accumulation. Every intent ships with an executable acceptance contract that runs before, during, and after generation. No verification, no merge. No exceptions.

3. **Flow over cadence.** Sprints exist to synchronize humans. When implementation is continuous, synchronization is overhead. Work flows from intent to production as fast as verification permits.

4. **Fund streams, not projects.** Projects assume fixed scope and a fixed end. Intent streams assume a persistent outcome and a continuous flow of intents toward it.

5. **Three roles, not thirty.** Every role earns its existence by doing work that AI cannot. If a role's primary function is coordination, status reporting, or translation between humans, it is overhead.

6. **Measure outcomes, not activity.** Velocity, story points, burndown, and capacity utilization are activity metrics from the human-bottleneck era. CID/ELCID measures four things: cycle time, verification pass rate, outcome KPI delta, and cost per shipped intent.

7. **Ceremony is a tax. Pay it only when it returns more than it costs.** CID/ELCID has exactly one recurring meeting at the enterprise layer and one at the team layer.

---

## Part One: CID — The Team-Level Practice

### The Atomic Unit: The Verifiable Outcome Slice (VOS)

CID does not have stories, tickets, tasks, or epics. It has Verifiable Outcome Slices.

A VOS is the smallest piece of user-observable value that can be independently specified, generated, verified, and shipped. Every VOS is a self-contained markdown document with five required sections:

1. **WHY — Intent Statement**: The motivation. What business problem this solves, who benefits, how it connects to the stream's strategic intent. One paragraph maximum. If you cannot state the intent in one paragraph, the slice is too large.

2. **WHAT — Acceptance Contract**: An executable specification that defines "done." Written in Gherkin (Given/When/Then) before generation begins. The contract is the single source of truth for verification. It is not a description of tests to write later — it is the test specification itself, authored by humans, executed by machines.

3. **HOW — Task Plan**: Step-by-step implementation plan. Tasks and sub-tasks, ordered by dependency, each small enough to be executed and verified independently. Proposed by the AI agent, reviewed and approved by the intent engineer before execution begins.

4. **CONTEXT — Bundle**: The curated set of code, docs, schemas, architectural decisions, and prior VOS artifacts the AI agent needs. Assembled by the intent engineer, not scraped by the agent. Context curation is a skill — the quality of the bundle directly determines the quality of the output.

5. **OUTCOME — Hypothesis**: The measurable business or user impact this slice is expected to produce, and how it will be observed in production. Not every VOS moves a KPI — but every VOS must state what it expects to change.

If any of the five sections is missing, the VOS is not ready for the queue. This is the only gate.

### VOS Lifecycle States

Every VOS passes through six states:

```
DRAFTED → CONTRACTED → QUEUED → GENERATING → VERIFYING → SHIPPED
```

- **Drafted**: Intent statement exists. WHY is clear. Not yet contracted.
- **Contracted**: All five sections complete. Acceptance contract written. Context bundle assembled. Task plan approved.
- **Queued**: Accepted into a stream's flow. An agent will pick it up.
- **Generating**: AI agent is executing the task plan against the acceptance contract.
- **Verifying**: Acceptance contract is running. TDD cycles executing. Compliance checks running.
- **Shipped**: All scenarios pass. Deployed to production. Outcome hypothesis instrumented.

VOS documents are version-controlled in a `.vos/` directory with `todo/`, `in-progress/`, and `shipped/` subdirectories. Every state transition is committed to git.

### The CID Pipeline

CID operates as a pipeline with feedback edges:

```
INTENT → CONTEXT → GENERATION → VERIFICATION → OBSERVATION → (back to INTENT)
```

- **Intent**: A human authors the VOS — the WHY, the WHAT (Gherkin acceptance contract), and the outcome hypothesis.
- **Context**: The intent engineer curates the context bundle. Context quality is the single largest determinant of generation quality.
- **Generation**: AI agents execute the task plan against the acceptance contract. The adaptive workflow runs inside this stage.
- **Verification**: The acceptance contract runs. TDD cycles, static analysis, security scans, compliance checks.
- **Observation**: The shipped VOS is instrumented in production against its outcome hypothesis. Real data closes the pipeline.

The pipeline is the unit of work. An observation can author a new intent. A verification failure can refine the context. A generation result can prompt a revision to the intent itself.

### The Adaptive Workflow Inside Generation

When a VOS enters GENERATING, the AI agent executes an adaptive workflow with three phases:

**Inception Phase** — Determine what to build and why:
- Workspace detection (always) — assess the codebase state
- Reverse engineering (brownfield only) — analyze existing code
- Requirements validation (always, adaptive depth)
- Application design (conditional) — for complex changes
- Decomposition (conditional) — break complex VOS into sub-units

**Construction Phase** — Determine how to build it, then build it:
- Functional design (conditional) — business logic design
- Non-functional requirements (conditional) — performance, security, scalability
- Infrastructure design (conditional) — cloud resource mapping
- Code generation (always) — TDD-first, one task at a time, user approval between parent tasks
- Build and test (always) — full test suite, compliance checks

**Operations Phase** — Deploy and observe:
- Deployment execution
- Outcome instrumentation
- Observation

The depth of each phase adapts to the complexity of the VOS:
- **Minimal**: Simple, clear change. Validate intent, generate code, verify, ship.
- **Standard**: Normal complexity. Full inception, standard construction, deploy.
- **Comprehensive**: Complex, high-risk. Full inception with reverse engineering, comprehensive construction with NFR analysis, staged deployment.

The agent determines the depth. The human approves it.

### Verification-First Development

CID mandates a strict verification-first protocol. This is not optional.

**TDD Red-Green-Refactor**: Every task follows the cycle:
1. RED — Write the test. It must fail. The code under test does not exist yet.
2. GREEN — Write the minimal code to make the test pass. No more.
3. REFACTOR — Improve the code without changing behavior. The test still passes.

**Incremental Generation**: The agent works on one sub-task at a time. Each sub-task produces a small, verifiable change. The human reviews and approves before the next sub-task begins.

**Git Discipline**: Every completed parent task is committed. The commit message references the VOS number and task number. The working tree is clean before the next task begins.

**Acceptance Contract Execution**: After all tasks complete, the full acceptance contract runs. Every Gherkin scenario must pass. If any scenario fails, the VOS returns to GENERATING.

### CID Roles

CID has three roles. That is the entire team org chart.

**Intent Engineer** (replaces: Product Owner, Business Analyst, most of Product Manager)
- Authors the WHY and WHAT of every VOS
- Writes acceptance contracts in Gherkin
- Curates context bundles
- Reviews and approves task plans
- Accepts or rejects verified output
- Measured on: intent acceptance rate (first-pass verification) and outcome hypothesis accuracy
- One strong intent engineer can feed 2-3 concurrent agent streams

**AI Orchestrator** (replaces: most of Software Engineer)
- Operates the agentic platform
- Selects models, tunes context, manages the adaptive workflow
- Intervenes when generation stalls or drifts
- Handles the percentage of work that requires human implementation
- Architects the systems the agents build into
- Reviews task plans for technical feasibility

**Verification Owner** (new role — no Agile equivalent)
- Designs and maintains acceptance contracts, verification harness, compliance extensions, rollback infrastructure
- Most senior technical role on the pod
- Reports outside the delivery chain to preserve independence
- This is the role that makes speed safe

### CID Cadence

One recurring meeting. Thirty minutes. Weekly.

**The Pipeline Review**: The pod looks at four numbers from the past seven days:
1. Intent cycle time
2. First-pass verification rate
3. In-production incident rate
4. Outcome hypothesis accuracy

If any is trending wrong, the pod identifies the cause and adjusts. That is the entire ceremony footprint.

No standups. No retrospectives separate from the Pipeline Review. No sprint planning. The queue is always open.

### CID Artifacts

Three persistent artifacts. Total.

1. **The VOS Repository** — Version-controlled store of all VOS documents, organized by status. The source of truth for what the organization is building, has built, and why.
2. **The Verification Harness** — The executable system that runs every acceptance contract. Owned by the Verification Owner.
3. **The Pipeline Dashboard** — Real-time view of the four team metrics plus VOS state.

No Jira. No Confluence. No sprint boards. No burndown charts.


---

## Part Two: ELCID — The Enterprise Governance Layer

ELCID is what makes CID work at scale. A single pod can practice CID with discipline. A 500-person engineering organization cannot — not without scaffolding that protects intent quality, allocates funding to outcomes, and gives executives a coherent view.

The design constraint: everything the legacy scaling frameworks do, with one-tenth the surface area.

### The Portfolio of Intents

The top-level artifact. A curated, prioritized set of strategic outcomes the organization is funding. Not epics. Not initiatives. Not OKRs cascaded through six layers. Strategic intents, each with:

- A measurable business outcome
- A funded intent stream pointed at it
- An accountable Intent Portfolio Lead
- A live read on the four enterprise metrics

The portfolio is reviewed monthly. Strategic intents enter when funded, exit when their outcome is achieved or the bet is abandoned. No fixed roadmap horizon.

### Intent Streams

A persistent, funded flow of related VOSs pointed at a single strategic intent. Streams are the unit of organization in ELCID — the analog to SAFe release trains, but without the train.

A stream consists of:
- One or more CID pods (each with the three CID roles)
- A standing budget, refunded quarterly based on outcome metrics
- A Stream Architect who owns technical coherence across the pods
- A direct line into the Portfolio of Intents

You do not fund "the new billing system." You fund "the billing modernization stream" with a quarterly budget and an outcome hypothesis. The stream produces VOSs continuously until the outcome is achieved or funding is withdrawn.

### Lean Intent Funding

ELCID inverts how budget attaches to work. Each stream has:

- **Baseline allocation**: Set quarterly, sufficient to fund the pod and platform consumption
- **Outcome multiplier**: Additional funding triggered by demonstrated outcome KPI movement
- **Sunset trigger**: Automatic defunding if outcome metrics fail to move after a defined window

This connects engineering spend directly to business outcome in a feedback loop short enough that the CFO can actually see it.

### ELCID Roles

Three enterprise roles. Same discipline as CID.

**Intent Portfolio Lead** (replaces: VP of Product, Head of Program Management, most of the PMO)
- Owns the Portfolio of Intents
- Decides what the organization is and is not building
- Allocates funding across streams
- Accountable for the four enterprise metrics

**Stream Architect** (replaces: Solution Architect, Release Train Engineer, most of the Architecture Review Board)
- Owns technical coherence within a stream
- Makes architectural decisions that span multiple pods
- Ensures VOSs compose into a coherent system

**Verification Officer** (new role — no equivalent in traditional scaled frameworks)
- Owns verification rigor across the entire enterprise
- Sets standards for acceptance contracts, verification harness, compliance extensions, rollback infrastructure
- Reports independently of the delivery chain — typically to the CTO or CEO
- The institutional check that prevents speed from outrunning trustworthiness

### The Monthly Intent Review

One enterprise ceremony. Ninety minutes. Once a month. Every stream lead presents three things:

1. **Shipped**: What VOSs went to production, what outcome KPI movement they produced
2. **In flight**: What is currently in the queue, generating, or verifying
3. **Friction**: Where verification is pushing back, where intents are getting rejected, where the stream is blocked

No PI Planning. No quarterly big-room planning. No two-day off-sites. No certified facilitators.

### The Four Enterprise Metrics

ELCID rolls up exactly four numbers from every stream:

1. **Intent Cycle Time**: Median time from VOS contracted to shipped. The speed metric.
2. **First-Pass Verification Rate**: Percentage of generated VOSs that pass acceptance on first attempt. The intent quality metric.
3. **Outcome KPI Delta**: Measured movement in the business KPI tied to each strategic intent. The value metric.
4. **Cost per Shipped Intent**: Fully-loaded cost (people + platform) divided by VOSs shipped. The efficiency metric.

A CIO can read the entire engineering organization in thirty seconds.

---

## Part Three: CID/ELCID vs Traditional Frameworks — Comparison

### CID vs Scrum (Team Level)

| Dimension | Scrum | CID |
|-----------|-------|-----|
| Atomic unit | User Story (estimated in points) | VOS (defined by executable acceptance contract) |
| Cadence | 2-week sprints | Continuous flow, no sprints |
| Planning | Sprint Planning (2-4 hrs) | VOS enters queue when contracted |
| Daily ceremony | Standup (15 min) | None. Coordinate via VOS queue + dashboard. |
| Review | Sprint Review (1-2 hrs) | Acceptance contract passes = done |
| Retrospective | Sprint Retro (1-2 hrs) | Pipeline Review covers this (30 min/week) |
| Estimation | Story points | None. Measure actual cycle time. |
| Roles | Scrum Master, Product Owner, Developers | Intent Engineer, AI Orchestrator, Verification Owner |
| Verification | QA downstream of dev | Verification upstream — contract before code |
| Artifacts | Product Backlog, Sprint Backlog, Increment | VOS Repository, Verification Harness, Pipeline Dashboard |

### ELCID vs SAFe (Enterprise Level)

| Dimension | SAFe | ELCID |
|-----------|------|-------|
| Core assumption | Humans are the bottleneck — coordinate them | AI is the implementation layer — protect intent + verification |
| Cadence | 2-week sprints inside 10-week Program Increments | Continuous flow. No sprints. No PIs. |
| Planning ceremony | PI Planning: 2 days, every 10 weeks, all hands | Monthly Intent Review: 90 minutes |
| Team ceremonies | 5+ recurring meetings per sprint | Pipeline Review: 30 min/week |
| Defined roles | ~14 | 6 total (3 team + 3 enterprise) |
| Funding model | Project-gated by PI cycles | Stream-funded with outcome-based reallocation |
| Primary metrics | Velocity, predictability, capacity utilization | Cycle time, verification rate, outcome delta, cost/intent |
| Compliance | Bolted on via external audit | Built in via modular extensions at every phase |
| Time to first value | 6-18 months (typical rollout) | 4-8 weeks (one stream, one pod) |
| Documentation | Thousands of pages, certified curriculum | One framework document, one practice guide |

### Ceremony Overhead Comparison

For a 10-person team per 2-week sprint:

| Ceremony | Scrum Hours | CID Hours |
|----------|-------------|-----------|
| Daily standups | 10 hrs | 0 |
| Sprint planning | 8 hrs | 0 |
| Backlog refinement | 6 hrs | 0 |
| Sprint review | 4 hrs | 0 |
| Retrospective | 4 hrs | 0 |
| Pipeline Review | 0 | 1 hr |
| **Total** | **32 hrs** | **1 hr** |

---

## Part Four: Compliance Extensions

CID supports a modular extension system for industry-specific compliance requirements. Extensions are blocking constraints — rules that must be satisfied at specific workflow phases before the VOS can advance.

Each extension consists of:
- A rules file with numbered, independently verifiable rules mapped to regulatory controls
- An opt-in file that presents a structured question during requirements validation

Extensions are loaded at workflow start but enforced only when opted in.

### Available Extensions

| Extension | Rules | Regulatory Mapping |
|-----------|-------|--------------------|
| HIPAA | 7 | 45 CFR Part 164 (Security Rule) — PHI classification, minimum necessary access, audit trails, encryption, BAA verification |
| FedRAMP | 8 | NIST SP 800-53 Rev 5 — FIPS crypto, continuous monitoring, access control, configuration management (Low/Moderate/High) |
| Financial Services | 7 | PCI-DSS v4.0, SOX Section 404, SOC 2 TSC — cardholder data, financial integrity, trust criteria |

Custom extensions follow the same format and can be authored for any regulatory framework.

---

## Part Five: Adoption Path

CID/ELCID is not a transformation program. It is designed to be adopted one stream at a time, with measurable results inside a quarter.

### Phase 1: Single Stream (Weeks 1-4)
Pick one strategic intent. Stand up one CID pod with the three roles filled. Install the CID workflow rules into the team's AI coding tool. Create the VOS repository. Begin drafting and contracting VOSs. Ship the first VOS by end of week 2. Measure the four team metrics from day one.

### Phase 2: Stream Maturation (Weeks 5-12)
Tune acceptance contract patterns. Establish cycle time and first-pass verification baselines. Run the first three Pipeline Reviews. Begin instrumenting outcome hypotheses against real production data. Deploy the evaluator. Enable compliance extensions if applicable. By end of Phase 2, the stream should be shipping at 5-10x the pre-CID rate with equivalent or lower defect rates.

### Phase 3: Enterprise Layer (Weeks 13-24)
Stand up the Portfolio of Intents. Appoint the Intent Portfolio Lead and Verification Officer. Run the first Monthly Intent Review. Onboard the second and third streams. Begin reallocating funding based on outcome metrics.

### Phase 4: Scale (Months 7+)
Add streams as fast as the organization can fill the three roles with qualified people. The constraint on adoption is never tooling or process — it is the supply of strong intent engineers and verification owners.

No certification required. No multi-day training courses. Readable in one sitting, adoptable in one quarter, defensible in one board meeting.

---

## Part Six: Anti-Patterns

Every framework attracts costuming — adopting the vocabulary while preserving old behavior.

**Costume CID**: Renaming Jira tickets "intents" without executable acceptance contracts or a real verification harness. Diagnostic: if the team still has standups, it is not practicing CID.

**Spec-as-wishlist**: Vague intent statements instead of precise contracts. The agent generates plausible code that doesn't solve the actual problem. Diagnostic: low first-pass verification rate trending down.

**Verification-as-afterthought**: Verification Owner role filled by a junior tester. Acceptance contracts are weak. Speed accelerates, defect rate accelerates with it. Diagnostic: incident rate climbs as cycle time falls.

**Vibe coding**: Bypassing the VOS lifecycle to "just fix it in the file." Unspecified, uncontracted, unverified changes. Diagnostic: production incidents that trace back to changes with no VOS.

**Project-funding zombies**: Adopting intent stream language but continuing annual project budget cycles. Streams cannot reallocate based on outcome. Diagnostic: Lean Intent Funding exists on a slide but not in the general ledger.

**Ceremony creep**: Reintroducing weekly status meetings, planning meetings, retrospectives. Within a quarter, ELCID is quietly replaced with Scrum. Diagnostic: any recurring meeting that is not the Pipeline Review or Monthly Intent Review.

**Context neglect**: Treating the context bundle as optional. Agents scrape whatever they find. Output quality is inconsistent. Diagnostic: high variance in first-pass verification rate across similar VOSs.

**The Forty-Person PMO**: Asking the legacy scaling framework's transformation office to "run ELCID." ELCID's enterprise layer is three roles, not thirty. The PMO must be dissolved or repurposed. Diagnostic: an org chart with more than six ELCID roles.

---

## Part Seven: Role Mapping — From Traditional to CID/ELCID

### Team Level

| Traditional Role | CID Equivalent | What Changes |
|-----------------|----------------|--------------|
| Product Owner | Intent Engineer | Writes executable acceptance contracts (Gherkin), not user stories. Curates context bundles. |
| Business Analyst | Intent Engineer | Same person. The BA and PO roles merge into one higher-leverage role. |
| Software Engineer | AI Orchestrator | Stops writing boilerplate. Starts operating AI platforms, reviewing output, making architectural decisions. |
| QA Engineer | Verification Owner | Moves upstream. Designs acceptance contracts before code exists, not test plans after. |
| Scrum Master | Eliminated | Coordination role for human-paced development. CID coordinates via the VOS queue and dashboard. |
| Tech Lead | AI Orchestrator or Verification Owner | Depending on strength — architecture-focused becomes Orchestrator, quality-focused becomes Verification Owner. |

### Enterprise Level

| Traditional Role | ELCID Equivalent | What Changes |
|-----------------|------------------|--------------|
| VP of Product | Intent Portfolio Lead | Manages Portfolio of Intents, not a product roadmap. Allocates funding to outcomes. |
| Release Train Engineer | Stream Architect | Technical coherence, not ceremony facilitation. |
| Solution Architect | Stream Architect | Same role, narrower scope (one stream, not the whole org). |
| PMO | Eliminated | Three enterprise roles replace the entire PMO function. |
| VP of Engineering | Verification Officer (partially) | The verification rigor function is extracted into an independent role. |

---

## Part Eight: Glossary

**Acceptance Contract**: The executable test specification that defines completion for a VOS. Written in Gherkin. Authored before generation. Run continuously.

**Adaptive Workflow**: The CID workflow engine that assesses complexity and executes only the phases that add value.

**AI Orchestrator**: The CID role that operates the AI platform, selects models, tunes context, and architects systems.

**Catalyst**: Alchemaize's commercial offering for CID/ELCID adoption — consulting, compliance extensions, evaluator, workshops, methodology.

**CID (Continuous Intent Delivery)**: The team-level practice of building software through intent specification, AI generation, and continuous verification.

**Compliance Extension**: A modular rule set enforcing industry-specific regulatory requirements at each workflow phase. Opt-in, blocking.

**Context Bundle**: The curated set of code, docs, schemas, and prior VOS artifacts provided to an AI agent for a given VOS.

**ELCID (Enterprise Layer for Continuous Intent Delivery)**: The governance, funding, and scaling framework that wraps CID at the organizational level.

**Evaluator**: A deployable quality scoring system that runs CID against test cases and measures output quality across AI models.

**Intent Engineer**: The CID role responsible for authoring intents and acceptance contracts. Highest-leverage role at the team layer.

**Intent Portfolio Lead**: The ELCID role responsible for the Portfolio of Intents and funding allocation.

**Intent Stream**: A persistent, funded flow of VOSs pointed at a single strategic intent.

**Lean Intent Funding**: The ELCID funding model — baseline allocation + outcome multiplier + sunset trigger.

**Pipeline Dashboard**: Real-time view of the four team metrics and VOS state.

**Pipeline Review**: The single weekly team ceremony. Thirty minutes.

**Monthly Intent Review**: The single monthly enterprise ceremony. Ninety minutes.

**Outcome Hypothesis**: The measurable impact a VOS is expected to produce, stated before shipping and verified after.

**Portfolio of Intents**: The curated set of strategic outcomes the enterprise is currently funding.

**Stream Architect**: The ELCID role responsible for technical coherence across pods within a stream.

**Verification Harness**: The executable infrastructure that runs every acceptance contract.

**Verification Officer**: The ELCID role responsible for verification rigor across the enterprise. Reports independently.

**Verification Owner**: The CID role responsible for the verification harness and acceptance contract design within a pod.

**Verification-First Development**: The CID protocol requiring TDD red-green-refactor on every task, acceptance contracts before generation, and git discipline at every checkpoint.

**VOS (Verifiable Outcome Slice)**: The atomic unit of work in CID. Contains WHY, WHAT, HOW, CONTEXT, and OUTCOME.

**VOS Repository**: Version-controlled store of all VOS documents, organized by lifecycle status.

---

*CID/ELCID is developed and maintained by Alchemaize, Inc. Version 2.0. (c) 2026 Alchemaize, Inc. All rights reserved.*
