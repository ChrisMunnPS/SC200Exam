# SC-200 cheat sheet 2/5 — Microsoft Sentinel

*One page. Facts snapshot 2026-11; verify current behaviour on Microsoft Learn before the exam.*

## Platform and roles

| Item | What to remember |
|---|---|
| What Sentinel is | Cloud-native **SIEM** with **SOAR** built in, running on a Log Analytics workspace. |
| Data boundary | The **workspace** holds the logs; Sentinel adds connectors, rules, incidents, and automation on top of it. |
| Multiple workspaces | **Workspace manager** for many workspaces you own; **Azure Lighthouse** for cross-tenant (MSSP) delegation. |
| Cross-workspace query | `workspace('name').Table` — needs read access to the other workspace. |
| Roles | **Sentinel Reader** (view) · **Sentinel Responder** (manage incidents) · **Sentinel Contributor** (author content) · **Playbook Operator** (run playbooks) · **Sentinel Automation Contributor** (lets Sentinel attach playbooks to rules). |
| Key tables | `SigninLogs`, `AuditLogs`, `SecurityEvent`, `SecurityAlert`, `SecurityIncident`, `OfficeActivity`, `CommonSecurityLog`, `Syslog`, `Heartbeat`, `ThreatIntelligenceIndicator`, `BehaviorAnalytics`, `Watchlist`. |

## Connectors and ingestion

| Item | What to remember |
|---|---|
| Content hub | Installs **solutions**: connectors, rule templates, workbooks, hunting queries, parsers, playbooks. |
| Azure Activity | Subscription control-plane operations (resource deletion, role assignment changes). |
| Microsoft Entra ID | `SigninLogs` (interactive and non-interactive sign-ins) and `AuditLogs` (directory changes). |
| Microsoft 365 | `OfficeActivity` — Exchange, SharePoint, OneDrive, Teams audit events. |
| Defender XDR | Streams alerts and incidents. For integrated products, **disable Microsoft incident creation rules** so one attack does not create two incidents. |
| CEF / Syslog | Via the **Azure Monitor Agent** on a Linux forwarder; then normalise with ASIM parsers. |
| Logs Ingestion API | For sources with no connector: data collection endpoint + **DCR** + custom table. |
| TAXII | Pulls STIX indicators from a provider into `ThreatIntelligenceIndicator`. |
| DCR purpose | Selects what is collected, transforms/filters it, and names the destination table — filter **before** ingestion to cut cost. |
| ASIM | Normalisation layer: one schema, one parser per source, so one query works across many vendors. |

## Retention, plans, and cost

| Item | What to remember |
|---|---|
| Analytics retention | **30 days to 2 years (730 days)**; fully queryable by rules and hunting. |
| Total retention | Mirrors data to the **data lake** for up to **12 years**; by default equals analytics retention. |
| Interaction with long-term | Older data is reached with **search jobs**, **restore**, or data lake queries — not always-on analytics rules. |
| Table plans | **Analytics** (real-time detection), **Basic** (cheap, 30-day single-table queries for troubleshooting), **Auxiliary** (low-touch verbose/audit data). |
| Commitment tiers | Discounted ingestion at a committed daily volume; unused commitment is still billed. |
| Cost levers | DCR filtering, table plan choice, retention split, commitment tiers, dropping noisy verbose tables. |

## Analytics rules

| Item | What to remember |
|---|---|
| Scheduled | Your KQL + **frequency** + **lookback**. Lookback must cover frequency **plus** expected ingestion delay. |
| NRT | Near real time, minimal latency (roughly per-minute), simple query, no custom schedule. |
| Fusion | Microsoft's multistage correlation engine; **enabled by default**, no query to author. |
| Microsoft Security | Creates incidents from alerts raised by connected Microsoft security products. |
| ML behavior analytics | Microsoft-trained models for anomalous behaviour; no query. |
| Anomaly | Informational anomaly alerts from ML models; tunable, not automatic incidents. |
| Threat intelligence | Matches ingested indicators against telemetry. |
| Entity mapping | Maps result columns to **Account, Host, IP, File, Process, CloudApplication, DNS, URL** — this is what makes pivots and the graph work. |
| Custom details | Named key/value fields surfaced on the alert for context. |
| Alert grouping | Combines alerts from one rule into one incident, by time window and/or entity. |
| Suppression | Stops the query running for a period **after** an alert; reduces alert volume. |
| Incident creation | Can be turned off per rule, leaving alerts in `SecurityAlert` for query-based triage. |
| Metadata | Severity, tactics and techniques (MITRE ATT&CK), description, and entity mappings drive triage and coverage reporting. |

## Automation

| Item | What to remember |
|---|---|
| Automation rule | No-code triage: conditions on incident/alert, then **assign owner, change status/severity, add tags, run playbook**. Runs in **order**, can **stop processing**. |
| Playbook | An **Azure Logic App** triggered by an incident or alert; calls connectors (Teams, ServiceNow, email, Graph, HTTP). |
| Playbook permissions | Sentinel runs incident-trigger playbooks under a service identity that needs rights on the playbook's resource group; **Sentinel Automation Contributor** attaches playbooks to rules. |
| Trigger types | **When an incident is created** · **when an alert is created** · manual run from the incident. |
| Design pattern | Automation rule decides *who/what/when*; the playbook performs the external actions. |

## Hunting and investigation

| Item | What to remember |
|---|---|
| Hunting loop | Hypothesis → query → **findings** (bookmarks, entities) → investigation → promote to detection. |
| Bookmark | Preserves a query and its exact results; tag it, add to an incident, investigate in the graph. |
| Livestream | Interactive continuous query that surfaces new matches as they arrive. |
| Search job | Asynchronous sweep over long-term/archived data; results land in a new table. |
| Restore | Brings a chosen time window of archived data back to the analytics tier for full KQL. |
| Entity page | Consolidated alerts, incidents, activity, and UEBA insights for one user, host, or IP. |
| Investigation graph | Entity-centric pivoting across alerts, events, and bookmarks. |
| MITRE page | Coverage of active rules and hunting queries by tactic and technique, with gap analysis. |
| Bookmarks vs watchlists | Bookmarks are **evidence**; watchlists are **reference lists**. |

## UEBA, watchlists, workbooks, and intel

| Item | What to remember |
|---|---|
| UEBA | Enabled per workspace; needs identity plus activity data. Builds behaviour profiles, peer-group comparison, and anomaly scores. |
| `BehaviorAnalytics` | Table holding UEBA-derived activity and anomaly records; entity pages surface the insights. |
| Watchlists | Imported reference lists (VIPs, scanners, high-value assets) used in queries via `_GetWatchlist('name')` or the `Watchlist` table. |
| Threat intelligence | Indicators in `ThreatIntelligenceIndicator`; match telemetry with a `join`, or use the TI matching rule type. |
| Workbooks | Interactive reports built on queries, with **parameters** for time range and filters. |
| Incident tasks | Checklist items that record and assign response steps on an incident. |

**Exam tip:** choose the rule type by *what triggers it* — your query (scheduled/NRT), Microsoft correlation (Fusion), a connected product (Microsoft Security), or a model (ML/anomaly).

*Source map: Configure your Microsoft Sentinel environment, Connect logs to Microsoft Sentinel, Create detections and perform investigations, Perform threat hunting.*
