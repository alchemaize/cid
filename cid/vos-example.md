# 12 - Feature (Billing-Invoices): Add recurring invoice generation

## WHY

The billing team currently generates invoices manually at the end of each month, a process that takes 4+ hours and is error-prone. Automating recurring invoice generation eliminates manual work, reduces billing errors, and ensures customers receive invoices on time. This VOS supports the "Billing Automation" strategic intent by delivering the core generation capability that all downstream billing features depend on.

### Depends On
- VOS #10 — Billing domain setup (shipped)
- VOS #11 — Invoice data model and storage (shipped)

### Enables
- VOS #15 — Invoice email delivery
- VOS #18 — Payment reconciliation

## WHAT

```gherkin
Feature: Recurring Invoice Generation

  As a billing administrator
  I want invoices to be generated automatically on a recurring schedule
  So that customers receive accurate invoices without manual intervention

  Background:
    Given the billing system is configured
    And a customer "Acme Corp" exists with an active subscription
    And the subscription has a monthly billing cycle starting on the 1st

  Scenario: Generate invoice on schedule
    When  the billing cycle date arrives for "Acme Corp"
    Then  an invoice is generated with the correct line items
    And   the invoice total matches the subscription amount
    And   the invoice status is "pending"
    And   the invoice is stored in the billing database

  Scenario: Generate invoice with prorated charges
    Given "Acme Corp" upgraded their plan mid-cycle on the 15th
    When  the billing cycle date arrives
    Then  the invoice includes a prorated charge for the upgrade
    And   the prorated amount is calculated based on remaining days in the cycle

  Scenario: Skip generation for paused subscriptions
    Given "Acme Corp" has paused their subscription
    When  the billing cycle date arrives
    Then  no invoice is generated
    And   a "skipped: subscription paused" event is logged

  Scenario: Handle generation failure gracefully
    Given the billing database is temporarily unavailable
    When  the billing cycle date arrives
    Then  the generation attempt is retried 3 times with exponential backoff
    And   if all retries fail, an alert is sent to the billing team
    And   the failed generation is queued for manual review

  Scenario: Prevent duplicate invoices
    Given an invoice for the current billing cycle already exists for "Acme Corp"
    When  the generation process runs again (e.g., retry or duplicate trigger)
    Then  no duplicate invoice is created
    And   a "skipped: invoice exists" event is logged
```

## HOW

### Implementation Context

The billing domain follows a serverless architecture with Lambda handlers, DynamoDB for storage, and EventBridge for scheduling. The invoice data model from VOS #11 defines the `Invoice` and `LineItem` types. All billing logic lives in `src/billing/` following the service pattern established in VOS #10.

TDD-first with red-green-refactor for all tasks. Tests use the existing DynamoDB local setup from the billing test fixtures.

### Tasks

* [ ] 1.0 - Create the recurring invoice generation service
  - [ ] 1.1 - Create `src/billing/services/generate-recurring-invoice.ts` with the public function signature returning a placeholder
  - [ ] 1.2 - Write test: generate invoice on schedule (primary success path) — expect RED
  - [ ] 1.3 - Implement minimal logic to make the test pass — expect GREEN
  - [ ] 1.4 - Refactor if opportunities exist
* [ ] 2.0 - Add proration calculation
  - [ ] 2.1 - Write test: generate invoice with prorated charges — expect RED
  - [ ] 2.2 - Implement proration logic — expect GREEN
  - [ ] 2.3 - Refactor: extract proration into `src/billing/utils/prorate.ts` if logic is non-trivial
* [ ] 3.0 - Add subscription status checks
  - [ ] 3.1 - Write test: skip generation for paused subscriptions — expect RED
  - [ ] 3.2 - Implement subscription status guard — expect GREEN
* [ ] 4.0 - Add error handling and retry logic
  - [ ] 4.1 - Write test: handle generation failure with retries — expect RED
  - [ ] 4.2 - Implement retry with exponential backoff — expect GREEN
  - [ ] 4.3 - Write test: alert on exhausted retries — expect RED
  - [ ] 4.4 - Implement alert dispatch — expect GREEN
* [ ] 5.0 - Add idempotency guard
  - [ ] 5.1 - Write test: prevent duplicate invoices — expect RED
  - [ ] 5.2 - Implement duplicate detection — expect GREEN
* [ ] 6.0 - Create the EventBridge scheduled Lambda handler
  - [ ] 6.1 - Create `src/billing/handlers/recurring-invoice-handler.ts` that invokes the generation service
  - [ ] 6.2 - Write integration test: handler processes a batch of due subscriptions
  - [ ] 6.3 - Add EventBridge rule configuration to CDK/IaC

## CONTEXT

### Context Bundle

**Code:**
- `src/billing/types.ts` — Invoice, LineItem, Subscription types from VOS #11
- `src/billing/services/` — existing service patterns from VOS #10
- `src/billing/repositories/invoice-repository.ts` — DynamoDB operations from VOS #11
- `tests/billing/fixtures/` — test fixtures and DynamoDB local setup

**Schemas:**
- `docs/api/billing-api.yaml` — OpenAPI spec (invoice endpoints)

**Prior VOSs:**
- VOS #10 — Billing domain setup: established service patterns, DynamoDB table design
- VOS #11 — Invoice data model: Invoice/LineItem types, repository layer

**Architecture:**
- Lambda + EventBridge for scheduling
- DynamoDB single-table design (see VOS #10 shipped docs)
- Idempotency via conditional writes (DynamoDB condition expressions)

## OUTCOME

### Outcome Hypothesis

**Metric:** Monthly invoice processing time
**Current:** 4.2 hours average (manual process, measured over last 6 months)
**Expected:** < 15 minutes (automated generation + manual review of exceptions only)
**Measurement:** Time delta between EventBridge trigger and last invoice write confirmation in CloudWatch logs
**Observation window:** 30 days post-ship, measured across 2 billing cycles
