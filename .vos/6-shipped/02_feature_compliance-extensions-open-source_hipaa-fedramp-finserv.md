# 2 - Feature (compliance-extensions-open-source): HIPAA + FedRAMP + Financial Services rule docs, Apache 2.0

## WHY: Intent Statement
The compliance-at-generation-time argument in the book (Chapter 11) doesn't hold up unless real, citable rule packs are open and usable. This VOS moves the rule documents into `extensions/compliance/` under Apache 2.0: HIPAA (7 rules, 45 CFR Part 164), FedRAMP (8 rules, NIST 800-53), Financial Services (7 rules, PCI-DSS + SOX + SOC 2). Format is plain Markdown with a stable structure so a team can roll its own runner from the docs. The runner + banking extension stay commercial (Catalyst). Also aligned the pipeline framing across the book + methodology in the same commit.

## WHAT: Acceptance Contract
```gherkin
Feature: Open-source compliance rule packs

  Scenario: Three rule packs live under extensions/compliance/
    Given the repo
    Then extensions/compliance/hipaa/, fedramp/, financial-services/ all exist
    And each has README + numbered rule files
    And each rule cites a specific regulatory reference + verification criteria

  Scenario: Apache 2.0 covers rule content
    Given the LICENSE at the repo root
    Then rule documents are covered
    And attribution requirements are documented in NOTICE

  Scenario: Runner + Banking extension are NOT included
    Given the compliance-extensions README
    Then it discloses that the runner ships with Catalyst
    And that the fourth extension (Banking — GLBA + NYDFS + FFIEC + BSA/AML) is commercial-only

  Scenario: Pipeline reframe aligned with book
    Given the methodology docs
    Then "continuous loop" language is gone
    And "pipeline with watching layer" is the canonical framing
```

## HOW: Task Plan
Content-only ship. Do NOT include commercial runner code; do NOT include Banking rules.

- [x] 1.0 - HIPAA (7 rules)
- [x] 2.0 - FedRAMP (8 rules)
- [x] 3.0 - Financial Services (7 rules)
- [x] 4.0 - Pipeline reframe pass across methodology docs

## CONTEXT: Context Bundle
- `extensions/compliance/`
- Sister repo `catalyst`, VOS #3: extension source of truth (this VOS is the open-source subset)

## OUTCOME: Outcome Hypothesis
**Metric:** Extensions adopted by external regulated teams
**Current:** No baseline
**Expected:** External implementations roll their own runners from the docs
**Measurement:** Community PRs + external mentions
**Observation window:** Active.

## Lifecycle Trace
| Date | State | Trigger | Actor |
|------|-------|---------|-------|
| 2026-07-08 | SHIPPED (backfill) | Backfilled from commit `0f0ff54` (2026-04-30, "Open-source compliance rule documents; pipeline reframe to match book") | David + Claude |
