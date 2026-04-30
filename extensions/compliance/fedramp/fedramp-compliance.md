# FedRAMP Compliance Rules

## Overview

These rules enforce technical controls required for FedRAMP authorization, mapped to NIST SP 800-53 Rev 5 control families. FedRAMP defines three impact levels (Low, Moderate, High) with increasing control requirements. The enforcement level is determined by the user's opt-in selection.

These rules layer on top of the baseline security extension (SECURITY-01 through SECURITY-15). FedRAMP rules reference baseline rules where overlap exists and add federal-specific requirements.

**Enforcement**: At each applicable stage, the model MUST verify compliance with these rules before presenting the stage completion message to the user.

### Blocking FedRAMP Finding Behavior

A **blocking FedRAMP finding** means:
1. The finding MUST be listed in the stage completion message under a "FedRAMP Findings" section with the FEDRAMP rule ID, description, and applicable impact level
2. The stage MUST NOT present the "Continue to Next Stage" option until all blocking findings are resolved
3. The model MUST present only the "Request Changes" option with a clear explanation of what needs to change
4. The finding MUST be logged in `aidlc-docs/audit.md` with the FEDRAMP rule ID, description, and stage context

### Impact Level Enforcement

Rules are tagged with their minimum applicable impact level. A rule tagged `[Moderate+]` applies to Moderate and High. A rule tagged `[High]` applies only to High.

| Tag | Applies To |
|---|---|
| `[All]` | Low, Moderate, High |
| `[Moderate+]` | Moderate, High |
| `[High]` | High only |

### Verification Criteria Format

Verification items are plain bullet points describing compliance checks.

---

## Rule FEDRAMP-01: FedRAMP Boundary Definition `[All]`

**Rule**: The system MUST define an explicit authorization boundary that identifies every component, service, data flow, and interconnection within scope for FedRAMP authorization:

- **System boundary diagram**: A network-level diagram showing all components, their trust zones, and data flows between them
- **Component inventory**: Every software component, cloud service, and third-party integration within the boundary, with version numbers
- **Data flow catalog**: Every data flow that crosses the boundary (ingress, egress, and interconnections with external systems), with protocol, port, and encryption status
- **Leveraged authorizations**: Cloud services that hold their own FedRAMP authorization (e.g., AWS GovCloud services) MUST be documented as leveraged authorizations with their authorization ID

**Verification**:
- A system boundary diagram exists in the design artifacts
- A component inventory lists every service with version and FedRAMP authorization status
- All data flows crossing the boundary are documented with protocol and encryption details
- Leveraged cloud service authorizations are referenced by authorization ID

---

## Rule FEDRAMP-02: FIPS 140-2 Validated Cryptography `[All]`

**Rule**: All cryptographic operations MUST use FIPS 140-2 (or 140-3) validated modules. This extends SECURITY-01 and HIPAA-04 with federal-specific requirements:

- **FIPS endpoints**: AWS services MUST be accessed via FIPS endpoints where available (e.g., `kms-fips.us-east-1.amazonaws.com`)
- **TLS configuration**: TLS implementations MUST use FIPS-approved cipher suites. Non-FIPS cipher suites MUST be explicitly disabled
- **Key management**: Encryption keys MUST be managed through a FIPS 140-2 Level 2+ validated HSM (e.g., AWS KMS in GovCloud, or CloudHSM)
- **Hashing**: Password hashing and integrity checks MUST use FIPS-approved algorithms (SHA-256+, PBKDF2, or bcrypt with FIPS-validated implementation)

**Verification**:
- All AWS service calls use FIPS endpoints where available
- TLS configuration explicitly disables non-FIPS cipher suites
- KMS or CloudHSM is used for all key management operations
- No non-FIPS cryptographic algorithms are used in the application

---

## Rule FEDRAMP-03: Continuous Monitoring Infrastructure `[All]`

**Rule**: The system MUST implement continuous monitoring capabilities aligned with the FedRAMP Continuous Monitoring Strategy Guide:

- **Vulnerability scanning**: Automated vulnerability scanning of all components at least monthly (Moderate+: weekly for internet-facing, monthly for internal)
- **Configuration scanning**: Automated configuration compliance scanning against CIS Benchmarks or DISA STIGs at least monthly
- **Log aggregation**: All security-relevant logs MUST be aggregated in a centralized SIEM or log management system with correlation capabilities
- **Incident detection**: Automated detection rules for at least: unauthorized access attempts, privilege escalation, data exfiltration indicators, malware indicators, and configuration drift
- **POA&M tracking**: The system MUST support tracking Plan of Action and Milestones for identified vulnerabilities and findings

**Verification**:
- Vulnerability scanning is configured and runs on the required schedule
- Configuration compliance scanning is configured against an approved benchmark
- Centralized log aggregation is implemented with defined retention
- Automated incident detection rules are configured for the required categories
- A mechanism exists for tracking remediation of findings (POA&M)

---

## Rule FEDRAMP-04: Access Control and Identity Management `[All]`

**Rule**: Access control MUST implement NIST AC (Access Control) family controls. This extends SECURITY-06, SECURITY-08, and SECURITY-12 with federal requirements:

- **Multi-factor authentication**: MFA MUST be enforced for all privileged access `[All]` and for all user access `[Moderate+]`
- **Session controls**: Sessions MUST lock after 15 minutes of inactivity `[Moderate+]` or 30 minutes `[Low]`. Sessions MUST terminate after 8 hours maximum regardless of activity `[High]`
- **Account management**: Automated account disabling after 90 days of inactivity `[Moderate+]`. Temporary/emergency accounts MUST auto-expire within 24 hours
- **Separation of duties**: Administrative functions MUST be separated from normal user functions. No single user should be able to both deploy code and approve deployments
- **Remote access**: All remote access MUST use encrypted channels with MFA. VPN or zero-trust network access is required for administrative access `[Moderate+]`

**Verification**:
- MFA is enforced at the required level (privileged for Low, all users for Moderate+)
- Session timeout is configured at the required threshold
- Automated account disabling is configured for inactive accounts
- Administrative and user roles are separated with no overlap
- Remote administrative access requires encrypted channels with MFA

---

## Rule FEDRAMP-05: Audit and Accountability `[All]`

**Rule**: Audit capabilities MUST implement NIST AU (Audit and Accountability) family controls. This extends SECURITY-03, SECURITY-14, and HIPAA-03:

- **Audit events**: The system MUST log at minimum: successful and failed authentication, privilege escalation, access to sensitive data, administrative actions, system startup/shutdown, and configuration changes
- **Audit record content**: Each audit record MUST include: timestamp (synchronized to authoritative time source), user identity, event type, event outcome (success/fail), source address, and affected resource
- **Time synchronization**: All system clocks MUST synchronize to an authoritative time source (e.g., NTP from NIST or AWS Time Sync Service) with drift detection
- **Audit storage**: Audit logs MUST be stored in a separate system from the application with write-once semantics. Retention: 1 year online, 3 years archived `[Moderate+]`
- **Audit review**: Automated audit log review MUST flag anomalies. Manual review of flagged events MUST be documented as a process `[Moderate+]`

**Verification**:
- All required audit event types are logged
- Audit records contain all required fields
- Time synchronization is configured with drift alerting
- Audit storage is separate from the application with write-once semantics
- Retention meets the required threshold for the impact level
- Automated anomaly detection is configured on audit logs `[Moderate+]`

---

## Rule FEDRAMP-06: System and Communications Protection `[Moderate+]`

**Rule**: Network and communications MUST implement NIST SC (System and Communications Protection) family controls:

- **Network segmentation**: The system MUST implement network segmentation between trust zones (public, application, data). Security groups and NACLs MUST enforce zone boundaries
- **Denial of service protection**: Public-facing endpoints MUST have DDoS protection (e.g., AWS Shield Standard at minimum, Shield Advanced for High)
- **DNS security**: DNSSEC MUST be enabled for all public DNS zones `[High]`
- **Boundary protection**: All traffic crossing the authorization boundary MUST pass through a managed firewall or WAF with logging enabled
- **Cryptographic key establishment**: Key exchange MUST use FIPS-approved methods (ECDH with P-256+ or RSA 2048+)

**Verification**:
- Network segmentation exists between public, application, and data tiers
- DDoS protection is enabled on public-facing endpoints
- WAF is configured on all boundary-crossing traffic with logging
- DNSSEC is enabled for public zones `[High]`
- Key exchange uses FIPS-approved algorithms

---

## Rule FEDRAMP-07: Configuration Management `[Moderate+]`

**Rule**: System configuration MUST implement NIST CM (Configuration Management) family controls:

- **Baseline configuration**: A documented configuration baseline MUST exist for every component (OS, runtime, framework, cloud service settings). Infrastructure as Code is the preferred mechanism
- **Configuration change control**: All configuration changes MUST go through a change control process with approval, testing, and rollback capability
- **Least functionality**: Systems MUST be configured to provide only essential capabilities. Unnecessary ports, protocols, services, and functions MUST be disabled
- **Software restriction**: Only authorized software may be installed. Container images MUST be built from approved base images with no additional package installation at runtime
- **Configuration monitoring**: Automated configuration drift detection MUST alert when a component deviates from its baseline

**Verification**:
- Infrastructure as Code defines the configuration baseline for all components
- Change control process is documented and enforced (PR review, approval, testing)
- No unnecessary ports, protocols, or services are enabled
- Container images use approved base images with no runtime package installation
- Configuration drift detection is configured with alerting

---

## Rule FEDRAMP-08: Contingency Planning `[Moderate+]`

**Rule**: The system MUST implement NIST CP (Contingency Planning) family controls:

- **Backup strategy**: All data stores MUST have automated backups with defined RPO (Recovery Point Objective) and RTO (Recovery Time Objective). Backups MUST be encrypted and stored in a separate availability zone or region
- **Disaster recovery**: A documented DR procedure MUST exist with tested failover to a secondary region `[High]` or availability zone `[Moderate]`
- **Backup testing**: Backup restoration MUST be tested at least annually `[Moderate]` or quarterly `[High]`
- **Data integrity verification**: Backup integrity MUST be verified automatically (checksums or test restores)

**Verification**:
- Automated backups are configured for all data stores with defined RPO/RTO
- Backups are encrypted and stored in a separate AZ or region
- DR procedure is documented
- Backup restoration testing schedule is defined
- Backup integrity verification is automated

---

## Enforcement Integration

| Stage | Applicable Rules | Enforcement |
|---|---|---|
| Requirements Analysis | FEDRAMP-01 | Boundary definition must be initiated |
| Functional Design | FEDRAMP-04, FEDRAMP-05 | Access control and audit design |
| NFR Requirements | FEDRAMP-02, FEDRAMP-03, FEDRAMP-06, FEDRAMP-08 | Crypto, monitoring, network, DR requirements |
| NFR Design | FEDRAMP-02, FEDRAMP-06, FEDRAMP-07 | FIPS crypto implementation, network segmentation, config baseline |
| Infrastructure Design | FEDRAMP-01 through FEDRAMP-08 | All rules verified against infrastructure artifacts |
| Code Generation | FEDRAMP-02, FEDRAMP-04, FEDRAMP-05 | FIPS endpoints, access controls, audit logging in code |
| Build and Test | FEDRAMP-03, FEDRAMP-07 | Scanning configuration, drift detection, baseline verification |

---

## Appendix: NIST SP 800-53 Rev 5 Control Family Mapping

| FEDRAMP Rule | Primary Control Family | Key Controls |
|---|---|---|
| FEDRAMP-01 | CA (Assessment, Authorization, Monitoring) | CA-3, CA-9 |
| FEDRAMP-02 | SC (System and Communications Protection) | SC-12, SC-13 |
| FEDRAMP-03 | CA, RA (Risk Assessment), SI (System and Information Integrity) | CA-7, RA-5, SI-4 |
| FEDRAMP-04 | AC (Access Control), IA (Identification and Authentication) | AC-2, AC-3, AC-7, AC-11, AC-17, IA-2 |
| FEDRAMP-05 | AU (Audit and Accountability) | AU-2, AU-3, AU-6, AU-8, AU-9, AU-11 |
| FEDRAMP-06 | SC (System and Communications Protection) | SC-5, SC-7, SC-8, SC-20 |
| FEDRAMP-07 | CM (Configuration Management) | CM-2, CM-3, CM-6, CM-7, CM-11 |
| FEDRAMP-08 | CP (Contingency Planning) | CP-6, CP-9, CP-10 |
