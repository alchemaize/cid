# CID — Continuous Intent Delivery

**The development methodology for the AI-native era.**

CID is the first software development framework designed from a blank sheet for the assumption that AI does the implementation, humans do the intent and the verification, and the team's job is to scale that loop without breaking it.

## The Problem

Every framework your team uses — Scrum, SAFe, Kanban — was designed to coordinate human coding throughput. AI removed that bottleneck. The new bottleneck is **intent clarity** and **verification rigor**. CID optimizes for the new bottleneck.

## The CID Loop

```
INTENT → CONTEXT → GENERATION → VERIFICATION → OBSERVATION
  ▲                                                    │
  └──────────────── feedback loop ─────────────────────┘
```

- **Intent** — Write a Verifiable Outcome Slice (VOS): WHY, WHAT (Gherkin), CONTEXT, OUTCOME
- **Context** — Curate 5-15 files the AI needs (not the whole codebase)
- **Generation** — AI executes the task plan with TDD (test first, then code)
- **Verification** — Acceptance contract runs. All scenarios pass or it doesn't ship.
- **Observation** — Measure the outcome hypothesis in production

## Three Roles

| Role | Job | Replaces |
|------|-----|----------|
| **Intent Engineer** | Writes the WHY and WHAT. Curates context. | Product Owner, BA, most of PM |
| **AI Orchestrator** | Operates AI agents. Catches drift. Makes architectural calls. | Senior Engineer |
| **Verification Owner** | Designs acceptance contracts. Makes speed safe. | QA Lead (but upstream) |

Roles are functions, not headcount. A pod runs at 1-3 FTE.

## Quick Start

### 1. Install the workflow rules

**Kiro:**
```bash
cp -R cid/cid-rules/ <your-project>/.kiro/steering/cid-rules/
cp -R cid/cid-rule-details/ <your-project>/.kiro/cid-rule-details/
```

**Claude Code:**
```bash
cat cid/cid-rules/core-workflow.md >> <your-project>/CLAUDE.md
cp -R cid/cid-rule-details/ <your-project>/.cid-rule-details/
```

**Cursor:**
```bash
mkdir -p <your-project>/.cursor/rules
cp cid/cid-rules/core-workflow.md <your-project>/.cursor/rules/cid-workflow.mdc
cp -R cid/cid-rule-details/ <your-project>/.cid-rule-details/
```

### 2. Create the VOS directory

```bash
mkdir -p <your-project>/.vos/{todo,in-progress,shipped}
```

### 3. Write your first VOS

See `cid/vos-specification.md` for the format and `cid/vos-example.md` for a complete example.

### 4. Start building

Tell your AI agent: *"Using CID, build VOS #1 from .vos/todo/"*

## Repository Structure

```
├── ELCID_Framework.md              # Enterprise governance framework
├── CID_ELCID_Complete_Methodology.md  # Full methodology reference
├── cid/
│   ├── README.md                   # CID quick start
│   ├── vos-specification.md        # VOS document format
│   ├── vos-example.md              # Complete example VOS
│   ├── development-workflow.md     # TDD protocols
│   ├── cid-rules/
│   │   └── core-workflow.md        # Agent-consumable rules (install this)
│   └── cid-rule-details/           # Detailed phase rules
│       ├── common/                 # Process overview, session continuity
│       ├── inception/              # Workspace detection, requirements
│       └── construction/           # Code generation protocols
└── LICENSE
```

## Works With Any AI Tool

CID is vendor-agnostic. It works with Kiro, Claude Code, Cursor, Amazon Q Developer, GitHub Copilot, Cline, or any agent that supports project-level rules.

## The Results

A 2-FTE CID pod (3 people, partial allocation) built 35 production applications — ~1M lines of code — in 15 weeks. The equivalent traditional effort: ~31 developer-years. [Full case study →](https://alchemaize.ai/catalyst)

## ELCID — Enterprise Governance

ELCID scales CID across multiple teams with:
- Portfolio of Intents (replaces project roadmaps)
- Intent stream funding (replaces project budgets)
- Four metrics: cycle time, verification rate, outcome delta, cost per intent
- Three enterprise roles: Intent Portfolio Lead, Stream Architect, Verification Officer
- One ceremony: Monthly Intent Review (90 minutes)

See `ELCID_Framework.md` for the full enterprise framework.

## Catalyst

[Catalyst](https://alchemaize.ai/catalyst) is Alchemaize's commercial offering for CID/ELCID adoption — workshops, compliance extensions (HIPAA, FedRAMP, PCI-DSS), quality evaluator, and expert coaching. CID is open. Catalyst makes adoption faster.

## License

MIT — see LICENSE file.

---

*Built by [Alchemaize](https://alchemaize.ai)*
