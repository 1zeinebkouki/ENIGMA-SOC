# Compliance Alignment

This document maps the project's technical controls to recognized security frameworks relevant to banking environments. This is a **contribution analysis**, not a compliance audit or certification. No real financial data, cardholder data, or SWIFT transactions are processed anywhere in this lab.

## ISO/IEC 27001 — Information Security Management

| Control area | How this project contributes | What would still be needed for certification |
|---|---|---|
| Access control | Wazuh monitors authentication events and flags abnormal login behavior | Formal risk assessment, defined scope, documented policies, Statement of Applicability |
| Logging & monitoring | Centralized log collection and correlation via Wazuh across endpoints and servers | Retention policy, log integrity controls, periodic review process |
| Incident management | Structured detection, response, and case tracking via Shuffle and TheHive | Defined roles/responsibilities, escalation procedures, post-incident review process |

The rule numbering (e.g. A.9, A.12.4, A.16) referenced in early project materials corresponds to the 2013 edition structure and has not been re-mapped to ISO/IEC 27001:2022. Any formal alignment claim should reference the current edition's control set.

## PCI DSS — Payment Card Industry Data Security Standard

| Requirement area | How this project contributes | What would still be needed |
|---|---|---|
| Requirement 10 (logging) | Wazuh tracks and logs access to monitored systems | Defined cardholder data environment (CDE) scope — none exists in this lab |
| Requirement 11 (security testing) | Simulated attack scenarios validate detection paths | Formal penetration testing program, vulnerability scanning cadence |
| Requirement 12 (security policy) | Documented incident response workflow via TheHive | Written information security policy, personnel security awareness program |

This project does not process, store, or transmit real cardholder data. Any PCI DSS reference describes conceptual alignment of monitoring capabilities, not scope-in compliance.

## SWIFT Customer Security Programme (CSP)

| Objective | How this project contributes |
|---|---|
| Monitoring and logging of user activity on critical systems | Wazuh centralized logging |
| Detection of anomalous or unauthorized access attempts | Custom Wazuh rules (e.g. brute-force detection, unauthorized file share access) |
| Incident response capability with traceability | TheHive case management with linked evidence |

No SWIFT messaging infrastructure, real financial transactions, or production banking systems are part of this lab. This mapping illustrates how SOC tooling *could* support SWIFT CSP objectives in a real deployment — it is not an assessment against the Customer Security Controls Framework (CSCF).

## MITRE ATT&CK Mapping

| Technique | Scenario |
|---|---|
| T1110 — Brute Force | Repeated failed login attempts (SSH / Windows authentication) |
| T1078 — Valid Accounts | Successful login following brute-force attempts |
| T1003 — OS Credential Dumping | Mimikatz detection rule (defensive detection only — no credential extraction performed) |

Technique tags shown in TheHive case descriptions come from the lab's rule configuration and should be independently verified against raw event data before being used to justify a detection accuracy claim.

## Summary

Standards references throughout this project describe **conceptual alignment** between the SOC/IAM controls implemented and objectives found in these frameworks. They are documented here for transparency about design intent — not as evidence of certification, audit completion, or regulatory compliance. See [`LIMITATIONS.md`](./LIMITATIONS.md) for the full scope of what remains unvalidated.
