# Known Limitations & Roadmap

This project is a lab prototype. This document distinguishes what was **validated by test**, what remains **partial**, and what is **planned but not yet demonstrated** — so the scope is clear to anyone reviewing this work.

## Open Issues

| Issue | Description | Root cause (hypothesis) |
|---|---|---|
| MidPoint → AD provisioning | New users created in MidPoint are not consistently propagated to Active Directory | DN mapping script targets a fixed OU (IT) regardless of the user's actual department |
| SSO end-to-end (Keycloak → MidPoint) | Redirect to Keycloak and AD authentication work, but MidPoint does not resolve the returned identity to a local user | Mismatch between the OIDC `preferred_username` claim and MidPoint's `name` attribute |
| Joiner email notification (SMTP) | Welcome email is generated correctly but not delivered | TLS handshake failure between the Docker container's Java runtime and Gmail SMTP (likely missing root CA in the Java truststore) |
| Cortex enrichment (VirusTotal) | Analyzer job fails in Cortex, even though a direct VirusTotal call via Shuffle succeeds | Not yet diagnosed — the two integration paths must be treated separately |
| Rule/webhook correspondence (Wazuh ↔ Shuffle) | One integration block references rule ID 100003 while the Mimikatz rule in the catalog is 100002 | Possible drift between the rule catalog and the webhook configuration — needs reconciliation |

## What Was Validated by Test

- Active Directory OU restructuring (11 OUs, 15 operational users, 7 groups)
- MidPoint inbound synchronization from AD (18 accounts, 17 OUs imported via LDAPS)
- MidPoint Live Sync (DirSync-based delta synchronization)
- Keycloak LDAPS federation and AD-based authentication
- Mover scenario: AD group/OU changes propagate to MidPoint and Keycloak
- Leaver scenario: disabling an AD account blocks Keycloak authentication (tested on new login attempts only — active sessions not tested)
- AD security hardening: PingCastle score reduced from 38 to 20 (documented before/after comparison)
- Shuffle → TheHive alert creation (HTTP 201 confirmed)
- Direct VirusTotal enrichment call via Shuffle (HTTP 200 confirmed)

## What Is Illustrated but Not Fully Measured

- Wazuh detection rules and severity scoring — functional, but detection precision/recall not benchmarked against labeled test events
- SOC workflows (SSH brute-force, Finance share access, Mimikatz, firewall block correlation) — demonstrate the automation pipeline, but end-to-end response actions (e.g., automatic IP blocking, account disablement) are not confirmed as executed on target systems
- AI-assisted alert analysis (LLM-based interpretation) — described in the architecture, not benchmarked

## Roadmap

| Priority | Task |
|---|---|
| P0 | Rotate lab secrets, remove `Allow Untrusted SSL/TLS`, restrict overly broad service account permissions (e.g., `midpoint.bind` Full Control) |
| P1 | Fix outbound provisioning (department-aware DN mapping), resolve OIDC username matching, fix SMTP TLS trust chain, diagnose Cortex failure |
| P1 | Reconcile Wazuh rule IDs with Shuffle webhook configuration; document a single source of truth for versions (MidPoint, Keycloak) |
| P2 | Test Leaver scenario against active sessions; verify automated containment actions actually execute on target systems |
| P2 | Define and measure MTTD / MTTR against labeled test events (not just qualitative "near real-time" claims) |
| P3 | Extend to ransomware / cloud / mobile banking scenarios once the core pipeline is fully reliable |

## Compliance Note

This project's controls contribute to objectives found in ISO/IEC 27001, PCI DSS, and SWIFT CSP, but this is **not a compliance audit or certification**. No real financial data, cardholder data, or production credentials are used anywhere in this lab.
