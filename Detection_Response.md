# Detection & Response Stack

## Overview

The SOC pipeline follows a five-stage flow: **collection → detection → orchestration → enrichment → case management**. Each stage was tested independently; end-to-end coverage across all scenarios is not yet fully measured (see [`LIMITATIONS.md`](./LIMITATIONS.md)).

## Wazuh (SIEM)

![Wazuh overview dashboard](./Detection_Response_images/wazuh_overview.png)

Wazuh centralizes log collection from Windows, Linux, and network sources, and applies both built-in and custom detection rules.

**Custom rules implemented:**

| Rule ID | Description | Category |
|---|---|---|
| 100001 | SSH authentication failure | Authentication / SSH |
| 100002 | Malicious activity: Mimikatz detected | Windows security |
| 100003 | Logon failure — unknown user or bad password | Authentication / Windows |
| 100500 | Dangerous command executed (e.g. `rm -rf`) | Command execution |
| 100600 | DNS query flagged as suspicious | DNS / suspicious activity |
| 100610 | Mail server authentication failure | Mail / brute force |
| Custom | Unauthorized access attempt to a restricted file share (Event ID 4663) | Access control |

![Wazuh custom rules catalog](./Detection_Response_images/wazuh_rules_catalog.png)

**Note:** A discrepancy was identified between the rule catalog (Mimikatz = 100002) and one Shuffle webhook integration referencing rule 100003. This is flagged as an open item in [`LIMITATIONS.md`](./LIMITATIONS.md) rather than presented as fully reconciled.

## Wazuh → Shuffle Integration

![Wazuh–Shuffle webhook integration](./Detection_Response_images/wazuh_shuffle_integration.png)

Wazuh forwards qualifying alerts to Shuffle via webhook, using a JSON alert format. Webhook identifiers are masked in the configuration screenshot to avoid exposing reusable endpoints.

## Shuffle (SOAR)

Shuffle receives Wazuh alerts and executes scenario-specific workflows: extracting indicators, querying enrichment sources, creating TheHive cases, and — where configured — triggering containment actions.

![Shuffle → TheHive alert creation, HTTP 201](./Detection_Response_images/shuffle_thehive_alert_success.png)

**Confirmed integration:** Shuffle → TheHive alert creation was tested and returned an HTTP 201 response with `success: true`. This confirms the API call succeeded; it does not by itself confirm incident resolution.

## TheHive (Incident Response)

![TheHive case list](./Detection_Response_images/thehive_case_list.png)

TheHive stores alerts and cases with structured context: description, key evidence, and recommended actions (e.g., password policy enforcement, MFA, standards references). Each case links back to the originating Wazuh alert.

![TheHive case detail](./Detection_Response_images/thehive_case_detail.png)

**Note on licensing:** The lab instance runs under a Platinum trial license. Licensing terms should be verified before assuming an entirely cost-free deployment.

## Cortex & VirusTotal (Enrichment)

Two separate enrichment paths were tested:

![Shuffle VirusTotal call, HTTP 200](./Detection_Response_images/shuffle_virustotal_success.png)

- **Direct VirusTotal call via Shuffle**: succeeded (HTTP 200, file report returned).

![Cortex analyzer job failure](./Detection_Response_images/cortex_job_failure.png)

- **Cortex analyzer job (VirusTotal_GetReport)**: failed in testing. The cause was not diagnosed in this lab cycle — the two paths are tracked separately rather than assumed to share the same root cause.

## MISP & Zabbix

Both are part of the target architecture (threat intelligence sharing and infrastructure monitoring, respectively). No dedicated test evidence was collected for these components in this lab cycle.

## Response Actions

The architecture includes automated response actions (firewall IP blocking, endpoint isolation, AD account disablement, analyst notification). A successful workflow execution or API call does not by itself confirm that a containment action was applied and verified on the target system — this distinction is maintained throughout the project's documentation.
