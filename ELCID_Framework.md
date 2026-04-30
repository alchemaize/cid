# ELCID

### Enterprise Layer for Continuous Intent Delivery

**The development framework for the AI-native era.**

*Version 2.0 — Alchemaize, Inc.*

---

## Preamble: Why ELCID Exists

Every dominant software development framework of the last 25 years was designed around a single, unspoken constraint: **human coding throughput**. Waterfall managed it with sequencing. Agile managed it with iteration. Scrum managed it with timeboxes. Scaled frameworks managed it with hierarchy. Every ceremony, every role, every artifact in those frameworks exists to coordinate the variable, expensive, error-prone process of humans typing code into editors.

That constraint is gone.

With Kiro, Claude Code, Cursor, Copilot, and the next wave of agentic development platforms, implementation throughput has increased by one to two orders of magnitude. The bottleneck has moved decisively to two new places:

1. **The clarity and completeness of intent** going into the AI.
2. **The speed and rigor of verification** coming out.

The middle — the actual writing of code — is now a near-zero-cost operation. Frameworks that continue to optimize for the old middle are not just inefficient; they are actively harmful, because they impose human-paced coordination overhead on a system that no longer has a human-paced bottleneck.

ELCID is built for the new constraint. It is the first development framework designed from a blank sheet for the assumption that AI does the implementation, humans do the intent and the verification, and the organization's job is to scale that pipeline without breaking it.

---

## The Stack

ELCID is the top layer of a three-layer stack. Each layer has a clear job and a clear boundary.

| Layer | Name | Job | Audience |
|---|---|---|---|
| **Enablement** | **CATALYST** | Alchemaize's services, products, and expertise that help organizations adopt CID and scale with ELCID. Consulting, compliance extensions, evaluator, workshops, and packaged methodology. | Engineering leaders, transformation sponsors |
| **Practice** | **CID — Continuous Intent Delivery** | The team-level methodology. How a pod of humans + AI agents builds software day to day. | Individual contributors, team leads |
| **Governance** | **ELCID — Enterprise Layer for Continuous Intent Delivery** | The organizational framework. How an enterprise funds, governs, and scales CID across many streams. | Executives, portfolio leads, transformation offices |

The relationship is deliberate: **CID is how teams build. ELCID is how enterprises scale it. CATALYST is how Alchemaize helps you get there.**

A team can practice CID without ELCID. An enterprise cannot scale CID without ELCID — they will revert to Jira-and-standups muscle memory within a quarter. ELCID is the scaffolding that prevents regression.

CID and ELCID are open methodologies. They work with any AI coding tool, any cloud provider, any programming language. CATALYST is Alchemaize's commercial offering — the consulting engagements, compliance extensions, evaluator framework, workshops, and packaged methodology that make adoption faster and compliance enforcement automatic. You can practice CID without CATALYST. CATALYST makes it dramatically easier.

---

## Part One: First Principles

ELCID rests on seven principles. These are not aspirations; they are load-bearing constraints. Every practice, role, and artifact in the framework derives from them.

**1. Intent is the artifact. Code is the exhaust.**
The thing humans author, version, review, and own is the intent specification. Code is generated from it and verified against it. If the spec is right and the verification is rigorous, the code is incidental.

**2. Verification is the trust anchor.**
Speed without verification is just faster debt accumulation. Every intent ships with an executable acceptance contract that runs before, during, and after generation. No verification, no merge. No exceptions. Verification is not downstream of development — it is upstream. The contract exists before the first line of code is generated.

**3. Flow over cadence.**
Sprints exist to synchronize humans. When implementation is continuous, synchronization is overhead. Work flows from intent to production as fast as verification permits. The unit of time is the cycle, not the sprint.

**4. Fund streams, not projects.**
Projects assume a fixed scope and a fixed end. Intent streams assume a persistent outcome and a continuous flow of intents toward it. Money attaches to outcomes, not deliverables.

**5. Three roles, not thirty.**
Every role that exists in ELCID earns its existence by doing work that AI cannot. If a role's primary function is coordination, status reporting, or translation between humans, it is overhead and must be eliminated.

**6. Measure outcomes, not activity.**
Velocity, story points, burndown, and capacity utilization are activity metrics from the human-bottleneck era. ELCID measures four things: cycle time, verification pass rate, outcome KPI delta, and cost per shipped intent. Nothing else rolls up.

**7. Ceremony is a tax. Pay it only when it returns more than it costs.**
ELCID has exactly one recurring meeting at the enterprise layer and one at the team layer. Everything else is asynchronous, dashboarded, or eliminated.

---

## Part Two: CID — The Team-Level Practice

CID is what a pod actually does, hour by hour. It is the layer that produces software. This section defines the concepts. The detailed operational methodology — the workflow rules, the VOS specification, the development protocols — lives in the `cid/` directory and is designed to be consumed directly by AI agents and human practitioners alike.

### The Atomic Unit: The Verifiable Outcome Slice (VOS)

CID does not have stories, tickets, tasks, or epics. It has **Verifiable Outcome Slices**.

A VOS is the smallest piece of user-observable value that can be independently specified, generated, verified, and shipped. Every VOS is a self-contained document with five components:


1. **Intent Statement (WHY)** — The motivation. Why this change matters, what business problem it solves, and how it connects to the stream's strategic intent. One paragraph maximum. If you cannot state the intent in one paragraph, the slice is too large.

2. **Acceptance Contract (WHAT)** — An executable specification that defines what "done" means. Written in Gherkin or structured test scenarios before generation begins. The contract is the single source of truth for verification. It is not a description of tests to write later — it is the test specification itself, authored by humans, executed by machines.

3. **Context Bundle** — The curated set of code, docs, schemas, architectural decisions, and prior VOS artifacts the AI agent needs to execute against. Assembled by the intent engineer, not scraped by the agent. Context curation is a skill — the quality of the bundle directly determines the quality of the output.

4. **Task Plan (HOW)** — The step-by-step implementation plan. Tasks and sub-tasks, ordered by dependency, each small enough to be executed and verified independently. The plan is proposed by the AI agent, reviewed and approved by the intent engineer before execution begins. Tasks follow a strict protocol: one sub-task at a time, user approval between parent tasks, git discipline at every checkpoint.

5. **Outcome Hypothesis** — The measurable business or user impact this slice is expected to produce, and how it will be observed in production. Not every VOS moves a KPI — but every VOS must state what it expects to change and how that change will be measured.

If any of the five is missing, the VOS is not ready for the queue. This is the only gate.

### The CID Pipeline

CID operates as a pipeline with feedback edges. Every VOS cycles through five stages that feed back into each other:

```
    ┌──────────┐    ┌──────────┐    ┌────────────┐    ┌──────────────┐    ┌─────────────┐
    │  INTENT  │───▶│ CONTEXT  │───▶│ GENERATION │───▶│ VERIFICATION │───▶│ OBSERVATION │
    └──────────┘    └──────────┘    └────────────┘    └──────────────┘    └─────────────┘
         ▲                                                                       │
         │                                                                       │
         └─────────────────────── feedback edges ────────────────────────────┘
```

- **Intent** — A human authors the VOS: the WHY, the WHAT (Gherkin acceptance contract), and the outcome hypothesis.
- **Context** — The intent engineer curates the context bundle: code, schemas, prior VOSs, architectural decisions. Context quality is the single largest determinant of generation quality.
- **Generation** — AI agents execute the task plan against the acceptance contract. The adaptive workflow (inception, construction, operations) runs inside this stage, scaling depth to complexity.
- **Verification** — The acceptance contract runs. TDD cycles, static analysis, security scans, compliance checks. Nothing advances without passing.
- **Observation** — The shipped VOS is instrumented in production against its outcome hypothesis. Real data closes the pipeline — confirming the hypothesis or seeding the next intent.

The pipeline is the unit of work. An observation can author a new intent. A verification failure can refine the context. A generation result can prompt a revision to the intent itself. This is not waterfall with AI in the middle — it is a learning system that gets better with every cycle.

### The VOS Lifecycle States

Every VOS passes through six states within the pipeline. There are no others.

```
DRAFTED → CONTRACTED → QUEUED → GENERATING → VERIFYING → SHIPPED
```

- **Drafted** — Intent statement exists. The WHY is clear. Not yet contracted.
- **Contracted** — Acceptance contract written in Gherkin. Context bundle assembled. Task plan approved. Outcome hypothesis stated. The VOS is fully specified.
- **Queued** — Accepted into a stream's flow. An agent will pick it up.
- **Generating** — AI agent is executing the task plan against the acceptance contract. Generation follows the adaptive workflow: the agent assesses complexity and executes the appropriate phases (inception through construction) as the work demands. Simple changes flow through minimal phases. Complex changes get full treatment. The workflow adapts to the work, not the other way around.
- **Verifying** — The acceptance contract is running. TDD red-green-refactor cycles execute for each task. Static analysis, security scans, and compliance checks run in parallel. Verification is not a gate at the end — it is continuous throughout generation.
- **Shipped** — All acceptance contract scenarios pass. Deployed to production. Outcome hypothesis instrumented for observation.

The state machine is rigid on purpose. VOS artifacts are version-controlled and tracked through status directories (`todo/`, `in-progress/`, `shipped/`), committed to git at every state transition. This creates an auditable trail of intent, decision, and outcome.

### The Adaptive Workflow Inside Generation

When a VOS enters the GENERATING state, the AI agent executes an adaptive workflow derived from three proven methodologies. The workflow has three phases:

**Inception Phase** — Determine what to build and why.
- Workspace detection (always) — assess the codebase state
- Reverse engineering (brownfield only) — analyze existing code
- Requirements validation (always, adaptive depth) — validate the VOS against the codebase
- Application design (conditional) — component and service design for complex changes
- Decomposition (conditional) — break complex VOS into sub-units if needed

**Construction Phase** — Determine how to build it, then build it.
- Functional design (conditional) — business logic design per unit
- Non-functional requirements (conditional) — performance, security, scalability
- Infrastructure design (conditional) — cloud resource mapping
- Code generation (always) — TDD-first, one task at a time, user approval between parent tasks
- Build and test (always) — full test suite, compliance checks, verification harness

**Operations Phase** — Deploy and observe.
- Deployment execution — ship to production
- Outcome instrumentation — wire up the hypothesis measurement
- Observation — monitor the KPI delta

The depth of each phase adapts to the complexity of the VOS:
- **Minimal** — Simple, clear change. Validate intent, generate code, verify, ship.
- **Standard** — Normal complexity. Full inception, standard construction, deploy.
- **Comprehensive** — Complex, high-risk. Full inception with reverse engineering, comprehensive construction with NFR analysis, staged deployment.

The agent determines the depth. The human approves it. This is the adaptive principle: the workflow adapts to the work, not the other way around.

### Verification-First Development

CID adopts a strict verification-first protocol for all code generation. This is not optional.

**TDD Red-Green-Refactor.** Every task in the task plan follows the cycle:
1. **Red** — Write the test. It must fail. The code under test does not exist yet or returns a placeholder.
2. **Green** — Write the minimal code to make the test pass. No more.
3. **Refactor** — Improve the code without changing behavior. The test still passes.

**Incremental Generation.** The agent works on one sub-task at a time. Each sub-task produces a small, verifiable change. The human reviews and approves before the next sub-task begins. This is not a suggestion — it is a protocol. Batch generation without incremental verification is the single fastest path to undetectable drift.

**Git Discipline.** Every completed parent task is committed. The commit message references the VOS number and task number. The working tree is clean before the next task begins. Uncommitted changes are a protocol violation.

**Acceptance Contract Execution.** After all tasks complete, the full acceptance contract runs. Every Gherkin scenario must pass. If any scenario fails, the VOS returns to GENERATING — it does not advance to SHIPPED with known failures.

### CID Roles

CID has three roles. That is the entire org chart at the team level. The roles are functions, not headcount — a pod can run at 1-3 FTE depending on allocation. One person can cover two roles on smaller projects. The constraint is that the three functions are covered, not that three people are assigned.

**Intent Engineer** *(replaces: Product Owner, Business Analyst, most of Product Manager)*
Authors the WHY and WHAT of every VOS. Writes acceptance contracts in Gherkin. Curates context bundles. Reviews and approves task plans. Accepts or rejects verified output. This is the highest-leverage role in the system. One strong intent engineer can feed two to three concurrent agent streams.

Intent engineers are measured on two things: intent acceptance rate (how often their VOSs pass verification on first generation) and outcome hypothesis accuracy (how often the shipped slice produces the predicted business impact). Both metrics reward precision of thought, not volume of output.

**AI Orchestrator** *(replaces: most of Software Engineer)*
Operates the agentic platform. Selects models, tunes context, manages the adaptive workflow, intervenes when generation stalls or drifts, handles the percentage of work that requires human implementation, and architects the systems the agents build into. This role requires deep technical judgment — it is not "prompt engineer." It is the senior engineer of the new era. The orchestrator reviews task plans for technical feasibility, ensures the context bundle is complete, and makes the call on adaptive depth.

**Verification Owner** *(new role — no Agile equivalent)*
Designs and maintains the acceptance contracts, the verification harness, the compliance extensions, and the rollback infrastructure. This is the role that makes speed safe. In traditional Agile, QA is downstream of dev. In CID, verification is upstream — the contract exists before generation. The Verification Owner is the most senior technical role on the pod and reports outside the delivery chain to preserve independence.

### CID Cadence

One recurring meeting. Thirty minutes. Weekly.

**The Pipeline Review.** The pod looks at four numbers from the past seven days: intent cycle time, first-pass verification rate, in-production incident rate, and outcome hypothesis accuracy. If any of the four is trending wrong, the pod identifies the cause and adjusts. That is the entire ceremony footprint.

No standups. CID pods coordinate through the intent queue and the verification dashboard, both of which are continuously visible. If a pod finds itself wanting a standup, it is a signal that the queue or the dashboard is broken.

No retrospectives separate from the Pipeline Review. The metrics surface problems in real time.

No sprint planning. The queue is always open. VOSs enter when contracted; agents pick them up when capacity exists.

### CID Artifacts

Three persistent artifacts. Total.

1. **The VOS Repository** — Version-controlled store of all VOS documents, organized by status (`todo/`, `in-progress/`, `shipped/`). Each VOS contains its intent statement, acceptance contract, context bundle, task plan, and outcome hypothesis. The repository is diffable, reviewable, and auditable. It is the source of truth for what the organization is building, has built, and why.

2. **The Verification Harness** — The executable system that runs every acceptance contract. Includes the TDD framework, static analysis, security scanning, compliance extension checks, and observability instrumentation. Owned by the Verification Owner, treated as production infrastructure.

3. **The Pipeline Dashboard** — Real-time view of the four team metrics, plus the state of every VOS in flight. Visible to the pod, the stream, and the enterprise layer.

No Jira. No Confluence pages. No sprint boards. No burndown charts. No story point spreadsheets. If a team finds itself reaching for any of those, CID has not been adopted; it has been costumed.

### Compliance Extensions

CID supports a modular extension system for industry-specific compliance requirements. Extensions are blocking constraints — rules that must be satisfied at specific workflow phases before the VOS can advance.

Each extension consists of:
- A **rules file** with numbered, independently verifiable rules mapped to regulatory controls
- An **opt-in file** that presents a structured question during requirements validation

Extensions are loaded at workflow start but enforced only when opted in. This keeps context lean for teams that do not need them while providing hard enforcement for teams that do.

The extension system is designed to be authored by compliance specialists, not developers. Each rule has a clear statement, concrete verification criteria, and a regulatory reference mapping. Extensions layer on top of baseline security rules and can be partially enforced for incremental adoption.

CATALYST ships with extensions for HIPAA, FedRAMP, and Financial Services (PCI-DSS, SOX, SOC 2). Custom extensions follow the same format and can be authored for any regulatory framework.

---

## Part Three: ELCID — The Enterprise Layer

ELCID is what makes CID work at scale. A single pod can practice CID with discipline. A 500-person engineering organization cannot — not without scaffolding that protects intent quality, allocates funding to outcomes, and gives executives a coherent view of a system that no longer produces traditional artifacts. ELCID is that scaffolding.

The design constraint for ELCID was explicit: **everything the legacy scaling frameworks do, with one-tenth the surface area.**

### The Portfolio of Intents

The top-level artifact at the enterprise layer is the **Portfolio of Intents** — a curated, prioritized set of strategic outcomes the organization is funding. Not epics. Not initiatives. Not OKRs cascaded down through six layers. Strategic intents, each with:

- A measurable business outcome
- A funded intent stream pointed at it
- An accountable Intent Portfolio Lead
- A live read on the four enterprise metrics

The portfolio is reviewed monthly. Strategic intents enter when funded, exit when their outcome is achieved or when the organization decides the bet is wrong. There is no fixed roadmap horizon — the portfolio is a living document, not an annual exercise.

### Intent Streams

An **Intent Stream** is a persistent, funded flow of related VOSs pointed at a single strategic intent. Streams are the unit of organization in ELCID — the analog to traditional scaled Agile release trains, but without the train.

A stream consists of:

- One or more CID pods (each with the three CID roles)
- A standing budget, refunded quarterly based on outcome metrics
- A Stream Architect who owns technical coherence across the pods
- A direct line into the Portfolio of Intents

Streams are funded, not projects. You do not fund "the new billing system." You fund "the billing modernization stream" with a quarterly budget and an outcome hypothesis. The stream produces VOSs continuously until the outcome is achieved or the funding is withdrawn. This single shift — from project funding to stream funding — is the financial mechanism that makes continuous flow possible at the enterprise level.

### The Alignment Cadence

ELCID has exactly one recurring enterprise ceremony.

**The Monthly Intent Review.** Ninety minutes. Once a month. Every stream lead presents three things:

1. **Shipped** — what VOSs went to production in the past month, and what outcome KPI movement they produced.
2. **In flight** — what is currently in the queue, generating, or verifying.
3. **Friction** — where verification is pushing back, where intents are getting rejected, where the stream is blocked.

That is the entire ceremony. No PI Planning. No quarterly big-room planning. No two-day off-sites. No physical sticky notes. No certified facilitators. The Monthly Intent Review replaces all of it.

### Lean Intent Funding

ELCID inverts how budget attaches to work.

Each stream has:

- **A baseline allocation** — set quarterly, sufficient to fund the pod and the platform consumption
- **An outcome multiplier** — additional funding triggered by demonstrated outcome KPI movement
- **A sunset trigger** — automatic defunding if outcome metrics fail to move after a defined window

This mechanism connects engineering spend directly to business outcome, in a feedback loop short enough that the CFO can actually see it.

### ELCID Roles

Three. Same discipline as CID.

**Intent Portfolio Lead** *(replaces: VP of Product, Head of Program Management, most of the PMO)*
Owns the Portfolio of Intents. Decides what the organization is and is not building. Allocates funding across streams. Accountable for the four enterprise metrics rolling up across the portfolio.

**Stream Architect** *(replaces: Solution Architect, Release Train Engineer, most of the Architecture Review Board)*
Owns technical coherence within a stream. Makes the architectural decisions that span multiple pods. Ensures that the VOSs flowing through the stream compose into a coherent system rather than a pile of generated fragments.

**Verification Officer** *(new role — no equivalent in traditional scaled frameworks — this is the differentiator)*
Owns verification rigor across the entire enterprise. Sets the standards for acceptance contracts, the verification harness, the compliance extensions, and the rollback infrastructure. Reports independently of the delivery chain — typically to the CTO or directly to the CEO. The Verification Officer is to ELCID what the CFO is to financial reporting: the institutional check that prevents the speed of the system from outrunning its trustworthiness.

That is the entire enterprise layer org chart. Three roles.

### The Four Enterprise Metrics

ELCID rolls up exactly four numbers from every stream.

1. **Intent Cycle Time** — median time from a VOS being contracted to being shipped. The headline speed metric.
2. **First-Pass Verification Rate** — percentage of generated VOSs that pass the acceptance contract on first attempt. The headline intent quality metric.
3. **Outcome KPI Delta** — measured movement in the business KPI tied to each strategic intent. The headline value metric.
4. **Cost per Shipped Intent** — fully-loaded cost (people + platform consumption) divided by VOSs shipped. The headline efficiency metric.

A CIO can read the entire engineering organization in thirty seconds.

---

## Part Four: CATALYST — The Enablement Brand

CATALYST is Alchemaize's commercial offering — the services, products, and expertise that make CID and ELCID adoption real. It is not a software platform you log into. It is the consulting engagements, compliance extensions, evaluator framework, workshops, and packaged methodology that compress months of self-guided adoption into weeks of expert-led enablement.

CATALYST is not required to practice CID — but it is the fastest path from "we read the framework" to "we are shipping VOSs." The same way an organization can adopt Terraform without HashiCorp but adopts faster with them, a team can practice CID with markdown files and discipline but adopts faster with CATALYST.

### What CATALYST Provides

**Workflow Engine.** The CID adaptive workflow, packaged as agent-consumable rules that work with any AI coding tool. The rules are markdown files that instruct the agent to follow the CID lifecycle, execute the adaptive phases, enforce verification-first development, and maintain git discipline. Setup is copying files into the tool's rules directory — no server, no SaaS, no vendor lock-in.

Supported tools:
- Kiro (steering files in `.kiro/steering/`)
- Claude Code (`CLAUDE.md`)
- Cursor (`.cursor/rules/`)
- Amazon Q Developer (`.amazonq/rules/`)
- GitHub Copilot (`.github/copilot-instructions.md`)
- Cline (`.clinerules/`)
- Any agent that supports project-level rules

**Compliance Extensions.** Pre-built, tested rule sets for HIPAA, FedRAMP, and Financial Services (PCI-DSS, SOX, SOC 2). Each extension integrates into the adaptive workflow as blocking constraints with opt-in control. Custom extensions follow the same format.

**Evaluator.** A deployable quality scoring system that runs CID against test cases, evaluates output across AI models, and generates comparison and trend reports. Deployed into the customer's own infrastructure. Runs on schedule. Measures whether CID is actually working, not whether people say it is.

**Readiness Assessment.** A structured intake questionnaire that maps customer context to CID input documents (Vision Document and Technical Environment Document). Converts business answers into the artifacts the adaptive workflow consumes.

**Expert Coaching.** CATALYST engagements are led by engineers who use CID daily on production applications — not consultants who learned the methodology from documentation. Workshop facilitation, extension development, evaluator interpretation, and multi-team training are all delivered by practitioners.

**Engagement Playbooks.** Internal guides for running CATALYST engagements: intake, discovery, workshop facilitation, extension development, evaluator operations. These encode the patterns that make CID adoption stick.

### Kiro as Reference Implementation

Kiro is the reference AI coding tool for CATALYST. The CID workflow rules are designed as Kiro steering files, and the VOS lifecycle maps directly to Kiro's spec-driven development model:

- VOS documents live in the project's `.kiro/specs/` or `.vos/` directory
- Kiro steering files enforce the adaptive workflow phases
- Kiro hooks automate verification (lint on save, test on task completion, compliance checks on VOS state transitions)
- Kiro's agent mode executes the task plan with incremental generation and user approval

Other tools work with the same rules in their native format. The methodology is the same; only the file paths change.

---

## Part Five: ELCID vs Traditional Scaled Frameworks — Head to Head

| Dimension | Traditional Scaled Agile | ELCID 2.0 |
|---|---|---|
| **Core assumption** | Humans are the implementation bottleneck; coordinate them with hierarchy. | AI is the implementation layer; protect intent quality and verification rigor. |
| **Atomic unit** | Story (estimated in points, scoped to a sprint) | Verifiable Outcome Slice (defined by an executable acceptance contract) |
| **Cadence** | 2-week sprints inside 10-week Program Increments | Continuous flow; no sprints, no PIs |
| **Planning ceremony** | PI Planning — 2 days, every 10 weeks, all hands | Monthly Intent Review — 90 minutes |
| **Team ceremonies** | Standup, Backlog Refinement, Sprint Planning, Sprint Review, Retrospective, System Demo, Scrum of Scrums | Pipeline Review — 30 minutes, weekly |
| **Number of defined roles** | ~14 | 6 total (3 team, 3 enterprise) |
| **Funding model** | Project or value stream, gated by PI cycles | Intent stream with continuous outcome-based reallocation |
| **Primary metrics** | Velocity, predictability, program predictability measure, feature completion | Cycle time, first-pass verification rate, outcome KPI delta, cost per shipped intent |
| **Verification model** | QA downstream of dev, manual test plans, acceptance at sprint review | Verification upstream — acceptance contract before generation, TDD throughout, continuous automated verification |
| **Documentation** | Thousands of pages, certified curriculum, multi-tier consulting industry | One framework document, one practice guide, one platform |
| **Compliance** | Bolted on via external audit processes | Built in via modular compliance extensions enforced at every workflow phase |
| **Time to first value** | 6–18 months (typical enterprise rollout) | 4–8 weeks (one stream, one pod, one intent portfolio) |
| **What it optimizes for** | Coordinating large numbers of human developers | Scaling AI-generated software safely |

---

## Part Six: Adoption Path

ELCID is not a transformation program. It is designed to be adopted one stream at a time, with measurable results inside a quarter.

### Phase 1: Single Stream (Weeks 1–4)
Pick one strategic intent. Stand up one CID pod with the three roles filled. Install the CID workflow rules into the team's AI coding tool. Create the VOS repository. Begin drafting and contracting VOSs. Ship the first VOS by end of week 2. Measure the four team metrics from day one.

### Phase 2: Stream Maturation (Weeks 5–12)
Tune the acceptance contract patterns. Establish the cycle time and first-pass verification baselines. Run the first three Pipeline Reviews. Begin instrumenting outcome hypotheses against real production data. Deploy the evaluator to measure output quality objectively. Enable compliance extensions if applicable. By the end of Phase 2, the stream should be shipping VOSs at 5–10x the pre-CID rate with equivalent or lower defect rates.

### Phase 3: Enterprise Layer (Weeks 13–24)
Stand up the Portfolio of Intents. Appoint the Intent Portfolio Lead and the Verification Officer. Run the first Monthly Intent Review with the original stream as the only participant. Onboard the second and third streams. Begin reallocating funding based on outcome metrics rather than project plans.

### Phase 4: Scale (Months 7+)
Add streams as fast as the organization can fill the three roles with qualified people. The constraint on ELCID adoption is never tooling or process — it is the supply of strong intent engineers and verification owners. Invest in growing both.

There is no "ELCID certification." There is no multi-day training course. The framework is designed to be readable in one sitting, adoptable in one quarter, and defensible in one board meeting.

---

## Part Seven: Anti-Patterns

Every framework attracts costuming — the practice of adopting the vocabulary while preserving the old behavior. ELCID has predictable failure modes.

**Costume CID.** A team renames its Jira tickets "intents" and declares CID adoption. Without executable acceptance contracts and a real verification harness, this is just Scrum with new labels. The diagnostic: if the team still has standups, it is not practicing CID.

**Spec-as-wishlist.** Intent statements written as vague aspirations rather than precise contracts. The agent generates plausible code that does not solve the actual problem. The verification harness catches it, but cycle time degrades because everything bounces. Diagnostic: low first-pass verification rate trending down.

**Verification-as-afterthought.** The Verification Owner role is filled by a junior tester or a part-time contractor. The acceptance contracts are weak. Speed accelerates, defect rate accelerates with it. Diagnostic: incident rate in production climbs as cycle time falls.

**Vibe coding.** Someone bypasses the VOS lifecycle and "just fixes it in the file." The change is unspecified, uncontracted, and unverified. It ships because no one is watching. It breaks something downstream because no acceptance contract covered it. Diagnostic: production incidents that trace back to changes with no VOS.

**Project-funding zombies.** The organization adopts the language of intent streams but continues to allocate budget on annual project cycles. Streams cannot reallocate, cannot scale up or down based on outcome, and revert to project behavior. Diagnostic: Lean Intent Funding exists on a slide but not in the general ledger.

**Ceremony creep.** A well-meaning manager reintroduces a weekly status meeting "just to stay aligned." Then a planning meeting. Then a retrospective. Within a quarter, ELCID has been quietly replaced with Scrum. Diagnostic: any recurring meeting that is not the Pipeline Review or the Monthly Intent Review.

**Context neglect.** The context bundle is treated as optional. Agents scrape whatever they find. Output quality is inconsistent. The intent engineer blames the model instead of the context. Diagnostic: high variance in first-pass verification rate across similar VOSs.

**The Forty-Person PMO.** The transformation office that was running a legacy scaling framework is asked to "run ELCID instead." It cannot. ELCID's enterprise layer is three roles, not thirty. The PMO must be dissolved or repurposed; it cannot be ported. Diagnostic: an org chart with more than six ELCID roles total.

---

## Part Eight: Glossary

**Acceptance Contract** — The executable test specification that defines completion for a VOS. Written in Gherkin or structured test scenarios. Authored before generation. Run continuously during and after.

**Adaptive Workflow** — The CID workflow engine that assesses complexity and executes only the phases that add value. Simple changes get minimal treatment; complex changes get comprehensive treatment.

**CATALYST** — Alchemaize's commercial offering for CID and ELCID adoption. Encompasses consulting engagements, compliance extensions, evaluator framework, readiness assessment, workshops, and packaged methodology.

**CID — Continuous Intent Delivery** — The team-level practice of building software through intent specification, AI generation, and continuous verification.

**Compliance Extension** — A modular rule set that enforces industry-specific regulatory requirements at each workflow phase. Opt-in, blocking, independently verifiable.

**Context Bundle** — The curated set of code, documentation, schemas, and prior VOS artifacts provided to an AI agent for a given VOS.

**ELCID — Enterprise Layer for Continuous Intent Delivery** — The governance, funding, and scaling framework that wraps CID at the organizational level.

**Evaluator** — A deployable quality scoring system that runs CID against test cases and measures output quality across AI models.

**Intent Engineer** — The CID role responsible for authoring intents and acceptance contracts. Highest-leverage role at the team layer.

**Intent Portfolio Lead** — The ELCID role responsible for the Portfolio of Intents and funding allocation across streams.

**Intent Stream** — A persistent, funded flow of VOSs pointed at a single strategic intent. The unit of organization at the enterprise layer.

**Pipeline Dashboard** — Real-time view of the four team metrics and the state of every VOS in flight.

**Pipeline Review** — The single weekly team ceremony in CID. Thirty minutes.

**Monthly Intent Review** — The single monthly enterprise ceremony in ELCID. Ninety minutes.

**Outcome Hypothesis** — The measurable business or user impact a VOS is expected to produce, stated before shipping and verified after.

**Portfolio of Intents** — The curated set of strategic outcomes the enterprise is currently funding.

**Stream Architect** — The ELCID role responsible for technical coherence across the pods within a stream.

**Task Plan** — The step-by-step implementation plan within a VOS. Tasks and sub-tasks, executed incrementally with verification at each step.

**Verifiable Outcome Slice (VOS)** — The atomic unit of work in CID. Contains intent statement (WHY), acceptance contract (WHAT), context bundle, task plan (HOW), and outcome hypothesis.

**Verification Harness** — The executable infrastructure that runs every acceptance contract, including TDD framework, static analysis, security scanning, and compliance checks.

**Verification Officer** — The ELCID role responsible for verification rigor across the enterprise. Reports independently of delivery.

**Verification Owner** — The CID role responsible for the verification harness and acceptance contract design within a pod.

**VOS Repository** — Version-controlled store of all VOS documents, organized by lifecycle status.

---

## Closing

The frameworks of the last quarter-century — Waterfall, RUP, XP, Scrum, Kanban, and the various scaled Agile frameworks — were all good answers to the question *how do humans coordinate to write software at scale?* They are no longer the right question.

The right question is *how do organizations harness AI implementation at scale without losing trust, coherence, or control of their outcomes?* ELCID is the first framework built to answer it.

**CID is how teams build. ELCID is how enterprises scale it. CATALYST is what they run it on.**

Conquer the backlog.

---

*ELCID is developed and maintained by Alchemaize, Inc. Version 2.0. © 2026 Alchemaize, Inc. All rights reserved.*
