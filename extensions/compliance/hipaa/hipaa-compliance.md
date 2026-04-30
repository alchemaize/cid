# HIPAA Compliance Rules

## Overview

These rules enforce technical safeguards required by the HIPAA Security Rule (45 CFR Part 164, Subpart C) for systems that store, process, or transmit Protected Health Information (PHI). They map to the Administrative, Physical, and Technical Safeguard categories defined in the Security Rule.

These rules layer on top of the baseline security extension (SECURITY-01 through SECURITY-15). If the baseline security extension is also enabled, both rule sets are enforced. HIPAA rules reference baseline rules where overlap exists rather than duplicating requirements.

**Enforcement**: At each applicable stage, the model MUST verify compliance with these rules before presenting the stage completion message to the user.

### Blocking HIPAA Finding Behavior

A **blocking HIPAA finding** means:
1. The finding MUST be listed in the stage completion message under a "HIPAA Findings" section with the HIPAA rule ID and description
2. The stage MUST NOT present the "Continue to Next Stage" option until all blocking findings are resolved
3. The model MUST present only the "Request Changes" option with a clear explanation of what needs to change
4. The finding MUST be logged in `aidlc-docs/audit.md` with the HIPAA rule ID, description, and stage context

If a HIPAA rule is not applicable to the current unit (e.g., HIPAA-05 when no external integrations exist), mark it as **N/A** in the compliance summary — this is not a blocking finding.

### Default Enforcement

All rules in this document are **blocking** by default.

### Partial Enforcement Mode

If the user selected **Partial** enforcement during opt-in, only rules HIPAA-01, HIPAA-02, HIPAA-03, and HIPAA-04 are enforced. All other rules are treated as advisory (non-blocking). Log the enforcement mode in `aidlc-docs/aidlc-state.md` under `## Extension Configuration`.

### Verification Criteria Format

Verification items are plain bullet points describing compliance checks. Each item should be evaluated as compliant or non-compliant during review.

---

## Rule HIPAA-01: PHI Data Classification and Inventory

**Rule**: Every system that handles PHI MUST maintain an explicit data inventory identifying which data elements constitute PHI. The 18 HIPAA identifiers MUST be cataloged:

| Category | Identifiers |
|---|---|
| Direct identifiers | Names, Social Security numbers, medical record numbers, health plan beneficiary numbers, account numbers, certificate/license numbers, vehicle identifiers, device identifiers, biometric identifiers, full-face photographs |
| Quasi-identifiers | Dates (birth, admission, discharge, death), geographic data smaller than state, telephone numbers, fax numbers, email addresses, URLs, IP addresses |

The data inventory MUST be documented in the functional design artifacts and referenced during code generation.

**Verification**:
- A PHI data inventory exists in the design artifacts listing every PHI element the system handles
- Each PHI element is classified by HIPAA identifier category
- Data flow diagrams show where PHI enters, is stored, is processed, and exits the system
- No PHI element is stored or transmitted without appearing in the inventory

---

## Rule HIPAA-02: Minimum Necessary Access

**Rule**: Access to PHI MUST be limited to the minimum necessary to accomplish the intended purpose. This extends SECURITY-06 (Least Privilege) and SECURITY-08 (Application-Level Access Control) with PHI-specific requirements:

- **Role-based PHI access**: Define explicit roles that determine which PHI fields each user type can view, create, or modify
- **Field-level access control**: API responses MUST filter PHI fields based on the requesting user's role — do not return full PHI records when only a subset is needed
- **Purpose limitation**: Each API endpoint or function that accesses PHI MUST document its purpose and the specific PHI fields it requires
- **No bulk PHI export without authorization**: Endpoints that return collections of PHI records MUST enforce pagination limits and require explicit authorization beyond standard authentication

**Verification**:
- Role definitions include PHI field-level access specifications
- API responses filter PHI fields based on the caller's role
- No endpoint returns more PHI fields than its documented purpose requires
- Bulk PHI access endpoints have explicit authorization checks and pagination limits
- Purpose documentation exists for every function that reads or writes PHI

---

## Rule HIPAA-03: PHI Audit Trail

**Rule**: Every access to PHI MUST be logged in an immutable audit trail. This extends SECURITY-03 (Application-Level Logging) and SECURITY-14 (Alerting and Monitoring) with PHI-specific requirements:

- **Access logging**: Every read, create, update, or delete of PHI MUST be logged with: timestamp, user identity, action performed, PHI fields accessed, patient/record identifier (without logging the PHI values themselves), and source IP/session
- **Immutability**: PHI audit logs MUST be stored in append-only or write-once storage. Application roles MUST NOT have permission to modify or delete audit logs
- **Retention**: PHI audit logs MUST be retained for a minimum of 6 years (HIPAA requirement)
- **Review capability**: The system MUST provide a mechanism to query audit logs by patient identifier, user, date range, and action type
- **Anomaly detection**: Alerts MUST be configured for unusual PHI access patterns: access outside business hours, bulk record access, access by users not in the patient's care team

**Verification**:
- Every PHI access operation generates an audit log entry
- Audit log entries include all required fields (timestamp, user, action, fields accessed, record ID, source)
- PHI values are NOT included in audit log entries (only record identifiers)
- Audit log storage is append-only with a 6-year retention policy
- Application IAM roles cannot delete or modify audit log storage
- Anomaly alerting is configured for at least 3 unusual access patterns

---

## Rule HIPAA-04: PHI Encryption Requirements

**Rule**: PHI MUST be encrypted at rest and in transit using NIST-approved algorithms. This extends SECURITY-01 (Encryption at Rest and in Transit) with specific requirements:

- **Encryption at rest**: AES-256 or equivalent. Key management MUST use a managed key service (e.g., AWS KMS) with automatic key rotation enabled
- **Encryption in transit**: TLS 1.2+ for all PHI transmission. TLS 1.0 and 1.1 MUST be explicitly disabled
- **Encryption key access**: IAM policies for encryption keys MUST follow minimum necessary — only services that directly encrypt/decrypt PHI should have key access
- **PHI in temporary storage**: PHI in caches, message queues, temporary files, and session storage MUST be encrypted. Temporary PHI MUST have automatic expiration/deletion
- **PHI in logs**: PHI MUST NOT appear in application logs, error messages, debug output, or monitoring dashboards. If PHI must be referenced in logs, use tokenized or hashed identifiers

**Verification**:
- All PHI data stores use AES-256 encryption with managed key rotation
- TLS 1.0 and 1.1 are explicitly disabled on all endpoints that handle PHI
- KMS key policies restrict access to only the services that need encryption/decryption
- Caches and queues containing PHI have encryption enabled and TTL-based expiration
- No PHI appears in application logs, error responses, or monitoring dashboards

---

## Rule HIPAA-05: Business Associate Agreement (BAA) Verification

**Rule**: Every third-party service, API, or cloud provider that processes, stores, or transmits PHI on behalf of the covered entity MUST have a Business Associate Agreement (BAA) in place. The system design MUST document:

- **BAA inventory**: A list of all third-party services that handle PHI, with BAA status
- **BAA-eligible services only**: The architecture MUST NOT route PHI through services that do not offer BAAs. For AWS, only BAA-eligible services may handle PHI (reference: AWS HIPAA Eligible Services)
- **Data flow validation**: Data flow diagrams MUST show that PHI never passes through a non-BAA-covered service

**Verification**:
- A BAA inventory exists listing every third-party service that handles PHI
- Each service in the inventory has BAA status documented (in place / pending / not available)
- No PHI data flow passes through a service without a BAA
- For AWS deployments, all services handling PHI are on the AWS HIPAA Eligible Services list
- Infrastructure design artifacts reference the BAA inventory

---

## Rule HIPAA-06: PHI Disposal and Retention

**Rule**: The system MUST implement secure disposal of PHI when it is no longer needed, and enforce retention policies:

- **Retention policy**: Define and enforce a PHI retention period based on applicable state and federal requirements (minimum 6 years for HIPAA, longer if state law requires)
- **Secure deletion**: When PHI is deleted, it MUST be rendered unrecoverable. For databases, this means actual deletion (not soft-delete flags alone). For object storage, versioning must be managed so deleted PHI does not persist in prior versions
- **Backup disposal**: PHI in backups MUST be subject to the same retention and disposal policies. Backups containing PHI MUST be encrypted and have defined expiration
- **De-identification**: When PHI is needed for analytics or testing, it MUST be de-identified per the HIPAA Safe Harbor method (remove all 18 identifiers) or Expert Determination method

**Verification**:
- A PHI retention policy is documented with specific retention periods
- Deletion operations render PHI unrecoverable (not just soft-deleted)
- Object storage versioning is configured so deleted PHI versions expire
- Backup encryption and expiration policies are documented
- Test and analytics environments use de-identified data, not production PHI

---

## Rule HIPAA-07: Breach Notification Readiness

**Rule**: The system MUST include technical capabilities to support HIPAA breach notification requirements (45 CFR 164.400-414):

- **Breach detection**: Automated monitoring MUST detect potential PHI breaches (unauthorized access, data exfiltration, anomalous bulk access)
- **Breach scope assessment**: The audit trail (HIPAA-03) MUST support determining which PHI records were affected, which individuals are impacted, and the timeframe of the breach
- **Notification data**: The system MUST be able to generate a list of affected individuals and their contact information for notification purposes
- **Incident response integration**: The system MUST document how breach detection feeds into the organization's incident response process

**Verification**:
- Automated breach detection monitoring is configured
- Audit trail supports querying by date range, user, and record to scope a breach
- The system can generate affected-individual lists from audit data
- Incident response integration is documented in the operations artifacts

---

## Enforcement Integration

These rules apply to the following AI-DLC stages:

| Stage | Applicable Rules | Enforcement |
|---|---|---|
| Requirements Analysis | HIPAA-01, HIPAA-05 | PHI inventory and BAA inventory must be initiated |
| Functional Design | HIPAA-01, HIPAA-02, HIPAA-03 | Data classification, access control design, audit trail design |
| NFR Requirements | HIPAA-04, HIPAA-06 | Encryption and retention requirements |
| NFR Design | HIPAA-04, HIPAA-06 | Encryption implementation patterns, disposal mechanisms |
| Infrastructure Design | HIPAA-04, HIPAA-05 | BAA-eligible services only, encryption configuration |
| Code Generation | HIPAA-01 through HIPAA-07 | All rules verified in generated code |
| Build and Test | HIPAA-03, HIPAA-07 | Audit trail testing, breach detection testing |

At each applicable stage:
- Evaluate all HIPAA rule verification criteria against the artifacts produced
- Include a "HIPAA Compliance" section in the stage completion summary listing each rule as compliant, non-compliant, or N/A
- If any rule is non-compliant, this is a blocking HIPAA finding — follow the blocking finding behavior defined in the Overview

---

## Appendix: HIPAA Security Rule Reference Mapping

| HIPAA Rule | Security Rule Section | Safeguard Type |
|---|---|---|
| HIPAA-01 | §164.312(d)(1) — Person or Entity Authentication | Technical |
| HIPAA-02 | §164.502(b), §164.514(d) — Minimum Necessary | Administrative |
| HIPAA-03 | §164.312(b) — Audit Controls | Technical |
| HIPAA-04 | §164.312(a)(2)(iv), §164.312(e)(1) — Encryption | Technical |
| HIPAA-05 | §164.502(e), §164.504(e) — Business Associates | Administrative |
| HIPAA-06 | §164.530(j) — Retention, §164.310(d)(2) — Disposal | Administrative / Physical |
| HIPAA-07 | §164.400-414 — Breach Notification | Administrative |
