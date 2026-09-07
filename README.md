# Network & Identity Architecture

## Network Zones

The lab environment is segmented into four zones:

- **WAN** — external connectivity
- **DMZ** — public-facing services (Mail, DNS, Web, Proxy)
- **LAN** — internal banking infrastructure (Active Directory, employee workstations, storage)
- **SOC** — centralized monitoring, orchestration, and incident response tooling

Traffic between zones passes through firewall/segmentation devices (pfSense at the WAN/DMZ boundary, OPNsense for internal segmentation). The exact production firewall topology (single device vs. cascade) was not finalized in this lab — see [`LIMITATIONS.md`](../LIMITATIONS.md).

![ENIGMA Architecture](./ENIGMA_Architecture.png)

## SOC Components

| Layer | Tool | Role |
|---|---|---|
| Network monitoring | Zabbix | Infrastructure health monitoring |
| SIEM | Wazuh | Log collection, correlation, alerting |
| SOAR | Shuffle | Automated response orchestration |
| Threat intelligence | MISP | Indicator lookup and sharing |
| Incident response | TheHive | Case management, evidence tracking |
| Enrichment | Cortex | Observable analysis (e.g., VirusTotal) |

### SOC Event Flow

1. **Collection** — endpoints, servers, and firewalls send events to Wazuh
2. **Detection** — Wazuh rules assign a description, severity, and category
3. **Orchestration** — Shuffle receives the alert and runs a scenario-specific workflow
4. **Enrichment** — observables are submitted to MISP or Cortex, depending on the workflow
5. **Case management** — TheHive stores alerts, cases, and analysis context
6. **Response** — a notification or containment action may be triggered; execution on target systems should be verified independently of a successful API call (see [`LIMITATIONS.md`](../LIMITATIONS.md))

## IAM Components

| Component | Role |
|---|---|
| Active Directory | Identity store, authentication |
| MidPoint (IGA) | Identity governance, provisioning, RBAC |
| Keycloak | SSO / LDAPS federation |

Active Directory is the validated source feeding MidPoint (inbound sync) and Keycloak (LDAPS federation) in current tests. MidPoint's role as an authoritative source for outbound provisioning to AD is the target design, not yet fully validated — see [`LIMITATIONS.md`](../LIMITATIONS.md).
