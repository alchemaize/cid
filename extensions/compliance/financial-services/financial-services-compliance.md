# Financial Services Compliance Rules

## Overview

These rules enforce technical controls required for financial services applications, covering PCI-DSS v4.0 (payment card data), SOX Section 404 (financial reporting integrity), and SOC 2 Type II (trust services criteria). The enforcement scope is determined by the user's opt-in selection.

These rules layer on top of the baseline security extension (SECURITY-01 through SECURITY-15). Financial services rules reference baseline rules where overlap exists and add sector-specific requirements.

**Enforcement**: At each applicable stage, the model MUST verify compliance with these rules before presenting the stage completion message to the user.

### Blocking Finding Behavior

A **blocking financial services finding** means:
1. The finding MUST be listed in the stage completion message under a "Financial Services Findings" section with the FINSVC rule ID, description, and applicable scope (PCI-DSS / SOX / SOC2)
2. The stage MUST NOT present the "Continue to Next Stage" option until all blocking findings are resolved
3. The model MUST present only the "Request Changes" option with a clear explanation of what needs to change
4. The finding MUST be logged in `aidlc-docs/audit.md` with the FINSVC rule ID, description, and stage context

### Scope Tags

| Tag | Applies When |
|---|---|
| `[PCI]` | User selected A or C (PCI-DSS scope) |
| `[SOX]` | User selected B or C (SOX/SOC2 scope) |
| `[ALL]` | Applies to all financial services selections (A, B, or C) |

### Verification Criteria Format

Verification items are plain bullet points describing compliance checks.

---

## Rule FINSVC-01: Sensitive Financial Data Classification `[ALL]`

**Rule**: The system MUST maintain an explicit inventory of sensitive financial data elements, classified by regulatory scope:

| Scope | Data Elements |
|---|---|
| PCI-DSS | Primary Account Number (PAN), cardholder name, expiration date, service code, full magnetic stripe data, CAV2/CVC2/CVV2/CID, PINs and PIN blocks |
| SOX | Financial transaction records, general ledger entries, journal entries, account balances, financial reporting data, audit evidence |
| SOC2 | Customer data subject to service commitments, system-generated data used in trust services criteria |

- **Data flow mapping**: Every sensitive data element MUST be traced through the system showing where it enters, is processed, is stored, and exits
- **Scope reduction**: The architecture SHOULD minimize the number of components that handle sensitive data. For PCI-DSS, this directly reduces the Cardholder Data Environment (CDE) scope

**Verification**:
- A sensitive data inventory exists listing every regulated data element
- Each element is classified by regulatory scope (PCI / SOX / SOC2)
- Data flow diagrams trace sensitive data through all system components
- The architecture minimizes the number of components handling sensitive data

---

## Rule FINSVC-02: Cardholder Data Protection `[PCI]`

**Rule**: Cardholder data MUST be protected per PCI-DSS v4.0 requirements:

- **PAN storage**: PAN MUST be rendered unreadable anywhere it is stored (encryption, truncation, tokenization, or one-way hashing). Display MUST mask PAN showing at most the first 6 and last 4 digits
- **Sensitive authentication data**: Full track data, CAV2/CVC2/CVV2/CID, and PINs MUST NOT be stored after authorization, even if encrypted
- **Tokenization**: Where possible, replace PAN with tokens for internal processing. Only the tokenization service should have access to the PAN-to-token mapping
- **Key management**: Encryption keys for cardholder data MUST have documented key management procedures including generation, distribution, storage, rotation (at least annually), and destruction
- **Network segmentation**: The Cardholder Data Environment (CDE) MUST be segmented from all other networks. Only systems with a business need access the CDE

**Verification**:
- PAN is rendered unreadable in all storage locations
- PAN display is masked (first 6 / last 4 maximum)
- No sensitive authentication data is stored post-authorization
- Tokenization is used where feasible to reduce CDE scope
- Key management procedures are documented with annual rotation
- CDE is network-segmented from non-CDE systems

---

## Rule FINSVC-03: Financial Data Integrity `[SOX]`

**Rule**: Systems handling financial reporting data MUST enforce data integrity controls per SOX Section 404:

- **Immutable audit trail**: Every create, update, or delete of financial data MUST be logged with: timestamp, user identity, before-value, after-value, and business justification. Financial records MUST NOT be physically deleted — use logical deletion with audit trail
- **Dual control for critical operations**: Financial transactions above a configurable threshold MUST require approval from a second authorized user before execution
- **Reconciliation support**: The system MUST support automated reconciliation between internal records and external sources (bank statements, payment processor reports, general ledger)
- **Period close controls**: The system MUST support period-close operations that prevent modification of financial data for closed periods without explicit override with audit trail
- **Calculation verification**: All financial calculations (interest, fees, taxes, totals) MUST be deterministic and reproducible. Floating-point arithmetic MUST NOT be used for monetary values — use decimal/fixed-point types

**Verification**:
- Every financial data mutation generates an immutable audit record with before/after values
- No physical deletion of financial records is possible through the application
- Dual control is enforced for transactions above the configured threshold
- Reconciliation endpoints or reports exist for external source comparison
- Period-close controls prevent modification of closed-period data
- Monetary calculations use decimal/fixed-point types, not floating-point

---

## Rule FINSVC-04: Change Management and Segregation of Duties `[ALL]`

**Rule**: All changes to financial systems MUST follow a controlled change management process:

- **Segregation of duties**: The person who writes code MUST NOT be the same person who approves it for production deployment. The person who deploys MUST NOT have direct database write access in production
- **Change approval**: Every change to production MUST have documented approval from an authorized approver before deployment
- **Change testing**: Every change MUST be tested in a non-production environment before production deployment. Test evidence MUST be retained
- **Emergency changes**: Emergency changes that bypass normal approval MUST be documented within 24 hours with retroactive approval and root cause analysis
- **Rollback capability**: Every deployment MUST have a documented and tested rollback procedure

**Verification**:
- CI/CD pipeline enforces separate roles for code authoring, approval, and deployment
- Production deployments require documented approval (PR approval, change ticket, or equivalent)
- Non-production testing is required before production deployment
- Emergency change procedures are documented with retroactive approval requirements
- Rollback procedures exist and are tested for every deployment mechanism

---

## Rule FINSVC-05: Vulnerability Management `[ALL]`

**Rule**: Financial systems MUST implement rigorous vulnerability management:

- **Scanning frequency**: External vulnerability scans at least quarterly by an Approved Scanning Vendor (ASV) `[PCI]`. Internal scans at least quarterly `[ALL]`. After any significant change, rescan within 7 days
- **Penetration testing**: Annual penetration testing of the application and network `[PCI]`. Semi-annual for internet-facing applications `[PCI]`
- **Patch management**: Critical vulnerabilities MUST be patched within 30 days of discovery. High vulnerabilities within 60 days. All others within 90 days
- **Dependency scanning**: Automated dependency vulnerability scanning on every build. Known vulnerable dependencies MUST NOT be deployed to production
- **Web application security**: Public-facing web applications MUST be protected by a WAF or undergo application security review at least annually `[PCI]`

**Verification**:
- Vulnerability scanning schedule meets the required frequency
- Penetration testing is scheduled at the required frequency `[PCI]`
- Patch management SLAs are documented (30/60/90 days by severity)
- CI/CD pipeline includes dependency vulnerability scanning that blocks on known vulnerabilities
- WAF is configured on public-facing web applications `[PCI]`

---

## Rule FINSVC-06: Data Retention and Destruction `[ALL]`

**Rule**: Financial data MUST be retained and destroyed per regulatory requirements:

| Scope | Minimum Retention | Notes |
|---|---|---|
| PCI-DSS | Cardholder data: only as long as business need exists | Must have documented retention policy with quarterly review |
| SOX | Financial records: 7 years | Includes all supporting documentation and audit evidence |
| SOC2 | Per service commitments | Defined in customer agreements |

- **Automated retention enforcement**: Data retention policies MUST be enforced automatically (TTL, lifecycle policies, scheduled purge jobs) — not dependent on manual processes
- **Secure destruction**: When data reaches end-of-retention, it MUST be securely destroyed (cryptographic erasure, secure overwrite, or physical destruction for media)
- **Destruction certification**: Destruction events MUST be logged with timestamp, data category, destruction method, and responsible party

**Verification**:
- Data retention policies are documented with specific periods per data category
- Automated retention enforcement is configured (lifecycle policies, TTL, scheduled jobs)
- Secure destruction methods are defined and implemented
- Destruction events are logged in the audit trail

---

## Rule FINSVC-07: Third-Party Risk Management `[ALL]`

**Rule**: Every third-party service that processes, stores, or transmits regulated financial data MUST be assessed and monitored:

- **Vendor assessment**: Third-party services MUST have current SOC 2 Type II reports or equivalent attestation. For PCI scope, third parties MUST be PCI-DSS compliant or covered by the organization's PCI assessment
- **Contractual controls**: Agreements with third parties MUST include data protection requirements, breach notification obligations, and right-to-audit clauses
- **Ongoing monitoring**: Third-party compliance status MUST be reviewed at least annually
- **Fourth-party risk**: If a third party subcontracts data processing, the subcontractor MUST meet the same compliance requirements

**Verification**:
- A third-party inventory exists with compliance attestation status for each vendor
- Contractual agreements include required data protection and audit clauses
- Annual review schedule is documented for third-party compliance
- Fourth-party risk is addressed in vendor agreements

---

## Enforcement Integration

| Stage | Applicable Rules | Enforcement |
|---|---|---|
| Requirements Analysis | FINSVC-01, FINSVC-07 | Data classification and vendor inventory initiated |
| Functional Design | FINSVC-01, FINSVC-02, FINSVC-03 | Data handling design, integrity controls, tokenization |
| NFR Requirements | FINSVC-04, FINSVC-05, FINSVC-06 | Change management, vulnerability management, retention |
| NFR Design | FINSVC-02, FINSVC-03 | Encryption patterns, audit trail design, calculation patterns |
| Infrastructure Design | FINSVC-02, FINSVC-05 | CDE segmentation, WAF, scanning infrastructure |
| Code Generation | FINSVC-01 through FINSVC-07 | All rules verified in generated code |
| Build and Test | FINSVC-04, FINSVC-05 | Change management enforcement, vulnerability scanning |

---

## Appendix: Regulatory Reference Mapping

| FINSVC Rule | PCI-DSS v4.0 | SOX / COSO | SOC 2 TSC |
|---|---|---|---|
| FINSVC-01 | Req 3 (Protect Stored Data) | COSO Principle 11 | CC6.1 |
| FINSVC-02 | Req 3, 4 (Protect Stored/Transmitted Data) | — | — |
| FINSVC-03 | — | Section 404, COSO Principles 10-13 | CC8.1 |
| FINSVC-04 | Req 6.5 (Change Management) | COSO Principle 12 | CC8.1 |
| FINSVC-05 | Req 6.3, 11 (Vulnerability Management) | — | CC7.1 |
| FINSVC-06 | Req 3.1 (Data Retention) | Section 802 (7-year retention) | CC6.5 |
| FINSVC-07 | Req 12.8 (Service Providers) | COSO Principle 15 | CC9.2 |
