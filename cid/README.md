# CID — Continuous Intent Delivery

The team-level practice methodology for AI-native software development.

CID is the operational layer of the ELCID framework. It defines how a pod of humans and AI agents builds software through intent specification, adaptive generation, and continuous verification.

## Quick Start

1. Copy the CID workflow rules into your AI coding tool's rules directory
2. Copy the VOS specification and example into your project
3. Start your first VOS

### Tool Setup

| Tool | Rules Location | Rule Details Location |
|------|---------------|----------------------|
| Kiro | `.kiro/steering/cid-rules/` | `.kiro/cid-rule-details/` |
| Claude Code | `CLAUDE.md` (append core-workflow.md) | `.cid-rule-details/` |
| Cursor | `.cursor/rules/cid-workflow.mdc` | `.cid-rule-details/` |
| Amazon Q Developer | `.amazonq/rules/cid-rules/` | `.amazonq/cid-rule-details/` |
| GitHub Copilot | `.github/copilot-instructions.md` | `.cid-rule-details/` |
| Cline | `.clinerules/` | `.cid-rule-details/` |

### Kiro Setup (Reference)

```bash
# From your project root
cp -R <catalyst-path>/cid/cid-rules/ .kiro/steering/cid-rules/
cp -R <catalyst-path>/cid/cid-rule-details/ .kiro/cid-rule-details/
```

### Other Tools

```bash
# Claude Code
cat <catalyst-path>/cid/cid-rules/core-workflow.md >> ./CLAUDE.md
cp -R <catalyst-path>/cid/cid-rule-details/ .cid-rule-details/

# Cursor
mkdir -p .cursor/rules
cp <catalyst-path>/cid/cid-rules/core-workflow.md .cursor/rules/cid-workflow.mdc
cp -R <catalyst-path>/cid/cid-rule-details/ .cid-rule-details/
```

## Contents

| File | Purpose |
|------|---------|
| `core-workflow.md` | The master workflow rules consumed by AI agents |
| `vos-specification.md` | The VOS document format specification |
| `vos-example.md` | A complete example VOS |
| `development-workflow.md` | TDD-first development protocols |
| `cid-rule-details/` | Detailed phase rules (loaded on demand by the workflow) |

## Key Concepts

- **VOS (Verifiable Outcome Slice)** — The atomic unit of work. Contains WHY, WHAT, HOW, context, and outcome hypothesis.
- **Adaptive Workflow** — Three phases (Inception, Construction, Operations) that execute only when they add value.
- **Verification-First** — Acceptance contracts before generation. TDD throughout. No exceptions.
- **Incremental Generation** — One sub-task at a time. User approval between parent tasks. Git discipline at every checkpoint.
- **Compliance Extensions** — Modular, opt-in rule sets for industry-specific requirements.

## Vendor Agnostic

CID works with any AI coding tool, any cloud provider, any programming language. The methodology is expressed as markdown rules that any agent can consume. The only requirement is that the tool supports project-level rules or steering files.

## Relationship to ELCID

CID is the team-level practice. ELCID is the enterprise governance layer that scales CID across many streams. CATALYST is Alchemaize's commercial offering — the consulting, extensions, evaluator, and workshops that make CID adoption faster. A team can practice CID without ELCID or CATALYST.
