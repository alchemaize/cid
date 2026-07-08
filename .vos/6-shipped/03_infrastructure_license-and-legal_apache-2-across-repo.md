# 3 - Infrastructure (license-and-legal): Apache 2.0 standardization + NOTICE + ERRATA

## WHY: Intent Statement
The methodology + rule docs are only credible open source if the license is unambiguous. This VOS standardizes on Apache 2.0 across the repo: single `LICENSE` file at the root, `NOTICE` for attribution requirements, README's License section pointing to both. ERRATA.md gets a top-level home so book corrections have a public place to land.

## WHAT: Acceptance Contract
```gherkin
Feature: Apache 2.0 + NOTICE + ERRATA

  Scenario: LICENSE at root
    Given the repo
    Then LICENSE exists at the root
    And is the full Apache 2.0 text

  Scenario: NOTICE documents attribution
    Given NOTICE
    Then attribution requirements for downstream users are documented

  Scenario: README makes the split clear
    Given the README License section
    Then it declares Apache 2.0 for the methodology + templates + examples + rule documents
    And declares that the runner + quality evaluator + Banking extension are NOT covered (commercial-only)

  Scenario: ERRATA at root
    Given ERRATA.md
    Then book corrections are recorded there
    And each entry references affected pages
```

## HOW: Task Plan
License consolidation + NOTICE + ERRATA. Do NOT dual-license; do NOT let a downstream user infer that the runner is Apache-licensed.

- [x] 1.0 - LICENSE (Apache 2.0)
- [x] 2.0 - NOTICE (attribution)
- [x] 3.0 - README License section clarifies open vs. commercial split
- [x] 4.0 - ERRATA.md at root

## CONTEXT: Context Bundle
- `LICENSE`, `NOTICE`, `ERRATA.md`
- `README.md`
- VOS #1 + #2: content this legal shell covers

## OUTCOME: Outcome Hypothesis
**Metric:** External adoption without licensing friction
**Current:** No baseline
**Expected:** 0 licensing-clarification issues opened by adopters
**Measurement:** GitHub issue traffic
**Observation window:** Active.

## Lifecycle Trace
| Date | State | Trigger | Actor |
|------|-------|---------|-------|
| 2026-07-08 | SHIPPED (backfill) | Backfilled from commit `13e2de5` (2026-04-30, "license: standardize on Apache 2.0 across the repo") | David + Claude |
