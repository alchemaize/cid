# 1 - Feature (open-methodology): CID + ELCID open-source methodology release

## WHY: Intent Statement
CID's argument only lands if the methodology is public + copyable. This VOS ships the open-source distillation of the practice: `cid/` (vos-specification.md, vos-example.md, development-workflow.md, cid-rules/core-workflow.md, cid-rule-details/), `ELCID_Framework.md` for enterprise governance, `CID_ELCID_Complete_Methodology.md` as the consolidated reference, README with quick-start install for Kiro / Claude Code / Cursor / Q Developer / Copilot / Cline, ERRATA for book corrections. Sister to the internal catalyst repo (VOS #2 there); the two must stay conceptually aligned.

## WHAT: Acceptance Contract
```gherkin
Feature: Open CID methodology

  Scenario: cid/ contains the practice surface
    Given a git clone of alchemaize/cid-public
    Then cid/vos-specification.md defines the VOS document format
    And cid/vos-example.md ships a complete example VOS
    And cid/development-workflow.md documents TDD protocols
    And cid/cid-rules/core-workflow.md is the agent-consumable file
    And cid/cid-rule-details/ has common/, inception/, construction/ subdirs

  Scenario: ELCID_Framework.md at root
    Given the repo root
    Then ELCID_Framework.md exists
    And documents Portfolio of Intents + intent stream funding + four metrics + three enterprise roles + Monthly Intent Review

  Scenario: Quick-start install for multiple tools
    Given the README
    Then install commands for Kiro (.kiro/steering/), Claude Code (CLAUDE.md), and Cursor (.cursor/rules/) are all present
    And each is a plain cp / cat command

  Scenario: ERRATA for book corrections
    Given a discovered error in the book
    Then ERRATA.md documents it
    And a fix + affected pages are named
```

## HOW: Task Plan
Markdown source, no build system. Vendor-agnostic install pattern. Do NOT bake tool-specific syntax into the rule content; do NOT drift from the catalyst-side methodology (VOS #2 there).

- [x] 1.0 - cid/ practice files (vos-spec, vos-example, workflow, rules, rule-details)
- [x] 2.0 - ELCID_Framework.md
- [x] 3.0 - CID_ELCID_Complete_Methodology.md
- [x] 4.0 - README + ERRATA + first upload

## CONTEXT: Context Bundle
- `README.md`, `CID_ELCID_Complete_Methodology.md`, `ELCID_Framework.md`
- `cid/`
- `ERRATA.md`
- Sister repo `catalyst`, VOS #2 (methodology-cid-elcid): the internal counterpart

## OUTCOME: Outcome Hypothesis
**Metric:** Adoption reach (stars, forks, external PRs)
**Current:** Undefined
**Expected:** Reachable adoption across the AI-coding-tool ecosystem
**Measurement:** GitHub metrics
**Observation window:** Active.

## Lifecycle Trace
| Date | State | Trigger | Actor |
|------|-------|---------|-------|
| 2026-07-08 | SHIPPED (backfill) | Backfilled from commits `846aa82` (2026-04-09, "initial: CID/ELCID open methodology") + `2a4f3cc` (2026-04-09, "Add files via upload") | David + Claude |
