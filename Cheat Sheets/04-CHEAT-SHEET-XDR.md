# SC-200 cheat sheet 1/5 — Microsoft Defender XDR

*One page. Facts snapshot 2026-11; verify current behaviour on Microsoft Learn before the exam.*

## Portal, incidents, and identity of things

| Item | What to remember |
|---|---|
| What XDR is | The correlation layer over Defender for Endpoint, Identity, Office 365, Cloud Apps (and Cloud). It joins alerts that share entities into **one incident**. |
| Alert vs incident | An **alert** is one detection. An **incident** groups related alerts and carries status, owner, classification, and the attack story. |
| Queue default | Shows incidents from the **last 30 days**, newest first. Filters: status, severity, assignment, service source, tags, classification. |
| Incident tabs | **Attack story** (timeline and evidence) · **Alerts** · **Entities** · **Evidence and response** · **Summary** · **Similar incidents**. |
| Lifecycle fields | **Status** (Active / Resolved) · **Owner** · **Classification** (true positive, false positive, informational, unknown) · **Determination** and reason. |
| Attack disruption | Automatic containment from high-fidelity signals, for example disabling a compromised account or containing a device **before** an analyst acts. |
| Unified RBAC | One role model across the Defender workloads: **Security Reader < Security Operator < Security Administrator**. Configured once in the Defender portal. |
| Threat analytics | Curated intelligence on tracked actors and campaigns, your exposure to their techniques, and related incidents. |
| Exposure management | Continuous attack-surface, exposure score, choke points, and attack paths **before** an incident occurs. |
| Security Copilot | Embedded assistance: incident summary, impact analysis, guided response, query drafting. It drafts; the analyst approves. |

## Advanced hunting and custom detections

| Item | What to remember |
|---|---|
| Data window | Up to **30 days** of raw data in the XDR schema (endpoint, email, identity, cloud app tables). |
| Versus Sentinel | Need 90 days, custom tables, or workspace joins? Query in **Sentinel**, not advanced hunting. |
| Query language | KQL, same syntax as Sentinel, different table set. `Device*`, `Email*`, `Identity*`, `CloudApp*`, `AlertInfo`, `AlertEvidence`. |
| Custom detection | Hunting query + schedule + optional **response action** (isolate device, mark user compromised, quarantine file, collect package). |
| Schedule options | Every **1, 3, 12, or 24 hours**; a new rule first checks the past 30 days. |
| Limits | Up to 500 custom detections per tenant; each run is capped in time and results. |
| Indicators | Custom **file hash / IP / URL / certificate** indicators with **allow**, **alert**, or **block**. Blocked hashes stop execution tenant-wide. |

## Defender for Endpoint

| Item | What to remember |
|---|---|
| Coverage | Device inventory: onboarding status, OS, risk, exposure, vulnerabilities. Default view is alerts in the last 30 days. |
| Discovery | **Device discovery** finds unmanaged devices from network telemetry; standard or basic discovery mode. |
| Isolation | **Full** blocks all network traffic except the Defender service; **selective** allows only listed destinations. Device stays manageable and live-response capable. |
| Live response | Interactive remote shell: `dir`, `cd`, `getfile`, `putfile`, `run`, `analyze`, `remediate`, `library` scripts. Session is audited. |
| Investigation package | Zipped forensic snapshot: processes, network, autoruns, scheduled tasks, registry, event logs. Collect **before** rebuild. |
| ASR rules | Behaviour blocking for Office child processes, script/download chains, LSASS credential theft, USB untrusted executables. **Audit** first, then **Block**. |
| Tamper protection | Blocks local and GPO/registry attempts to disable antivirus, real-time protection, cloud protection, behaviour monitoring. |
| Device groups | Group devices by tag/OS/domain and set the **automation level**: full remediation, semi (approval), or none. |
| AIR | Automated investigation reviews alerts, collects evidence, and proposes or performs remediation per the group's automation level; approvals land in **Action center**. |
| TVM | Threat and vulnerability management prioritises by **exposure + exploitability + threat intelligence**, not by CVE age. |
| Other controls | Web content filtering, network protection, EDR in block mode, controlled folder access, device control. |

## Defender for Identity

| Item | What to remember |
|---|---|
| Sensor placement | Domain controllers, plus **AD FS, AD CS, and Microsoft Entra Connect** servers. One sensor per server; no sensor means no visibility for that DC. |
| Telemetry | Kerberos, NTLM, DNS, and directory replication traffic — not endpoint processes. |
| Core detections | Pass-the-Hash, Pass-the-Ticket, **Kerberoasting**, Golden Ticket, overpass-the-hash, reconnaissance, DC sync, rogue DC, brute force, honeytoken use. |
| Honeytoken | A decoy account or resource; any authentication against it is high-signal. |
| Lateral movement paths | Shows how a compromised identity could reach **tier-0** assets through existing logon relationships; use it to break the path. |
| Versus Entra ID Protection | Identity Protection = cloud sign-in risk. Defender for Identity = on-premises AD credential abuse. |
| Response | Disable the account, reset the password **twice** for KRBTGT, remove sessions, rotate service account secrets. |

## Defender for Office 365

| Item | What to remember |
|---|---|
| Safe Links | Time-of-click URL evaluation and rewriting. Covers email, Teams, and supported Office clients. |
| Safe Attachments | Sandbox detonation; **Dynamic Delivery** sends the body while the attachment is still being analysed. |
| ZAP | Zero-hour auto purge removes or quarantines messages **after** delivery when a verdict changes to malware or high-confidence phish. |
| Threat Explorer | Email hunting by sender, recipient, subject, URL, file hash; powers **soft delete**, **move to quarantine**, and campaign views. |
| AIR | Automated investigation for email: entity analysis, cluster analysis, and a recommended action (purge, block sender, etc.). |
| Attack simulation | Controlled phishing campaigns plus automated training assignment for users who click or submit credentials. |
| Submissions | User-reported and admin-submitted messages land in the **Submissions** queue for review and purge. |

## Defender for Cloud Apps

| Item | What to remember |
|---|---|
| Cloud Discovery | Parses firewall/proxy/endpoint logs to find shadow IT, then matches apps to the catalog with a **risk score** (0–100). |
| Sanctioning | Mark discovered apps **sanctioned** or **unsanctioned** to record the decision and drive reporting and blocks. |
| Session policies | Real-time controls (block download, block upload, protect on download) that require **Conditional Access App Control**. |
| File policies | Scan files **at rest** in connected apps; match labels or sensitive info types; remove external sharing, quarantine, alert. |
| App governance | OAuth app inventory, permissions, publisher, consent grants; policies can alert, disable, or revoke an overprivileged app. |
| Anomaly detection | Built-in policies for impossible travel, unusual inbox rule, mass download, suspicious OAuth app, ransomware activity. |
| Connector types | **API connectors** (app audit and files) versus **Conditional Access App Control** (proxy, real-time session). |

## Defender for Cloud

| Item | What to remember |
|---|---|
| Purpose | Posture management (CSPM) plus workload protection across **Azure, AWS, GCP, and on-premises**. |
| Secure score | Weighted posture score with prioritised recommendations; not an incident or threat measure. |
| To Sentinel | Connect with the **Defender for Cloud connector** and choose which alert types stream into the workspace. |
| Cloud-native alerts | Control-plane and data-plane detections, attack-path analysis, and agentless scanning. |

**Exam tip:** when a question asks *where the correlation happened*, the answer is Defender XDR; when it asks *where the detection logic lives*, match the workload to its telemetry.

*Source map: Mitigate threats using Microsoft Defender XDR, Defender for Endpoint, Defender for Identity, Defender for Office 365, Defender for Cloud Apps, Defender for Cloud.*
