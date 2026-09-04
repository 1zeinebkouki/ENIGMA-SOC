# ENIGMA — Banking SOC & IAM Platform

**Stopping cyber attacks before they shut down banks.**

ENIGMA is a two-pillar cybersecurity project simulating a full banking security environment: a **Security Operations Center (SOC)** for automated threat detection and incident response, and an **Identity & Access Management (IAM)** stack for centralized identity governance.

## Why ENIGMA?

When a bank goes offline for even one hour, the impact is severe: interrupted transactions, blocked customer payments, and long-term loss of trust. Traditional security is reactive, slow, and dependent on scarce skilled analysts. ENIGMA transforms banking security from **reactive defense** to **proactive protection**.

## Project Structure

| Folder | Description |
|---|---|
| [`architecture/`](./architecture) | Global network architecture (WAN/DMZ/LAN/SOC) |
| [`SOC/`](./SOC) | Detection & response stack, attack scenarios, compliance mapping |
| [`IAM/`](./IAM) | Identity architecture, JML lifecycle, AD security hardening |

## Solution Overview

**SOC Pillar** — Detection → Orchestration → Response
- **Wazuh** (SIEM/XDR): centralized log ingestion, correlation, alerting
- **Shuffle** (SOAR): automated, event-driven response workflows
- **TheHive / Cortex**: incident case management and enrichment
- **MISP**: threat intelligence sharing
- **pfSense / OPNsense**: perimeter defense and network segmentation
- AI-assisted alert interpretation (LLM) to reduce false positives and support analyst decisions

**IAM Pillar** — Provisioning → Governance → SSO
- **Active Directory**: identity store, bank-oriented OU hierarchy
- **MidPoint** (IGA): identity lifecycle automation, RBAC, reconciliation
- **Keycloak**: SSO / LDAPS federation with AD
- Full **Joiner–Mover–Leaver** automation
- **PingCastle**-driven AD security hardening (risk score reduced from 38/70 → 20)

## Key Results

| KPI | Before | After |
|---|---|---|
| MTTD | Manual detection delay | Near real-time (Wazuh + AI) |
| MTTR | Hours (manual) | Seconds–minutes (SOAR + Flask) |
| IAM Risk Score (PingCastle) | 38–70 / 100 | 20 / 100 |
| Incident handling | Reactive, manual | Automated, structured |

## Compliance Alignment
ISO/IEC 27001 · PCI DSS · SWIFT CSP · MITRE ATT&CK

> **Note:** This project is for educational and simulation purposes. No real financial data, cardholder data, or production credentials are used.

## License
MIT
