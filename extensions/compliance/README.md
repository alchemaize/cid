# Compliance Extensions

Three regulatory extensions referenced in *Continuous Intent Delivery*, Chapter 11. Each extension is a numbered set of rules. Each rule is mapped to a specific regulatory citation and paired with verification criteria stated precisely enough for an automated checker to run.

| Extension | Rules | Coverage |
|---|---|---|
| HIPAA | 7 | 45 CFR Part 164 (Security Rule technical safeguards), Administrative, Physical, and Technical Safeguard categories |
| FedRAMP | 8 | NIST 800-53 boundary, FIPS-validated cryptography, continuous monitoring, access control, audit, configuration management, contingency planning |
| Financial Services | 7 | SOX-relevant logging, PCI-DSS cardholder data protection, segregation of duties, vulnerability management, retention, third-party risk |

## What is in this directory

This directory carries the **rule documents**: the canonical specification for each rule, including the regulatory citation, the verification criteria, and the activation pattern (which of the five lifecycle stages the rule fires at, per Chapter 11).

The rule documents are the substance of the extensions. They describe what the methodology enforces and why. A team can read the HIPAA rule document and know exactly which CFR sections each rule traces to, what the audit log must contain, and what the encryption requirement is. The rules are the contract between the methodology and the regulation.

## What is not in this directory

The **runner** — the small command-line tool that loads the rules into an active stream and enforces them at generation time — ships separately as part of the CATALYST commercial package. The rules are useless without a runner, but a runner is also straightforward to build from the rule documents in this directory if you want to roll your own. The format is intentionally simple Markdown with a stable structure: rule ID, citation, requirement, verification list, enforcement default.

## Using the rules

A pod opts in to an extension at the stream-funding decision (Chapter 9). Once opted in, the rule document is loaded by the workflow and consulted at five activation stages described in Chapter 11. Findings block at requirements validation, at generation time, at verification, at handoff, and at the post-deploy audit sweep.

## Banking extension

A fourth extension covering banking-specific regulations (GLBA, NYDFS Cybersecurity Program, FFIEC, BSA/AML, Regulation E) ships with CATALYST and is not currently part of this open release. The structure of the banking rules matches the structure of the three rules here. If demand exists for an open banking-rule set, the rules will move into this directory in a future release.

## Authoring a custom extension

Custom extensions follow the same Markdown structure as the three included here. Use the HIPAA file as the template. The rule numbering scheme is `EXTENSION-NN`, citations are inline, and verification items are plain bullet points. A pod can author a custom extension in roughly an afternoon if a control mapping already exists for the regulation.

## License

These rule documents are released under the same MIT license as the rest of `cid` (see the repository root). Use them, fork them, modify them, ship them as part of your own systems. Attribution is appreciated; not required.
