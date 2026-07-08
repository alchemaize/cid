# CURRENT.md — cid-public VOS domain map

**Status:** Active. Public Apache 2.0 repo hosting the open-source subset of the CID + ELCID methodology, the three open compliance extensions (HIPAA / FedRAMP / Financial Services), and the ERRATA for the *Continuous Intent Delivery* book. The runner + quality evaluator + Banking extension stay commercial (Catalyst).

## Numbering

- Next VOS number: **4** (see [`3.last`](3.last))
- Highest shipped: **#3** (license-and-legal)

## Domain map (3 domains)

| Domain | Current | Type |
|--------|---------|------|
| open-methodology | [#1](6-shipped/01_feature_open-methodology_cid-elcid-initial-release.md) | Feature |
| compliance-extensions-open-source | [#2](6-shipped/02_feature_compliance-extensions-open-source_hipaa-fedramp-finserv.md) | Feature |
| license-and-legal | [#3](6-shipped/03_infrastructure_license-and-legal_apache-2-across-repo.md) | Infrastructure |

## Relationship to catalyst

- Catalyst VOS #2 (methodology-cid-elcid) is the internal source of truth. This repo's VOS #1 is the open-source distillation.
- Catalyst VOS #3 (compliance-extensions) is the internal source (includes Banking + the runner). This repo's VOS #2 is the open subset.
- ERRATA lives here because it's a public promise; catalyst can reference it.

## Not captured as VOSes

- **DS_Store gitignore churn** (`d006898` + `5f1b224`) is housekeeping.
