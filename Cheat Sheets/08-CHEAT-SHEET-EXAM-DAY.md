# SC-200 cheat sheet 5/5 — Exam-day decision map

*One page. Facts snapshot 2026-11; verify current behaviour on Microsoft Learn before the exam.*

## Numbers worth memorising

| Number | Belongs to |
|---|---|
| **30 days** | Defender XDR advanced hunting window · Defender incident queue default · Device inventory default · device timeline default. |
| **30 days – 2 years (730 days)** | Sentinel analytics retention range. |
| **Up to 12 years** | Sentinel total retention in the data lake. |
| **1, 3, 12, 24 hours** | Custom detection schedule options in Defender XDR. |
| **500** | Maximum custom detections per tenant; also a common "mass download" threshold in queries. |
| **0 – 100** | Cloud app risk score range. |
| **2** | KRBTGT resets required after a Golden Ticket. |
| **4** | Defender XDR domain sections in this kit's simulator: XDR · Sentinel · KQL · Response. |
| **700 / 1000** | Pass mark; scaled from raw percentage. |

## "Which tool?" — the fastest route

| If the question is about… | Answer |
|---|---|
| Correlating alerts from several Defender workloads | Microsoft Defender XDR |
| Raw process, file, network, or registry evidence on a device | Defender for Endpoint |
| Kerberos, NTLM, or on-premises AD abuse | Defender for Identity |
| Email delivery, links, attachments, or campaign purge | Defender for Office 365 |
| Shadow IT discovery, OAuth apps, or SaaS session control | Defender for Cloud Apps |
| Multi-cloud posture score and recommendations | Defender for Cloud |
| Long-term log storage, custom queries, and incident automation | Microsoft Sentinel |
| Pre-incident exposure and attack paths | Security Exposure Management |
| Tracked actor and campaign intelligence | Threat analytics |
| Natural-language incident summary | Microsoft Security Copilot |
| Audit retention and DLP for compliance evidence | Microsoft Purview |

## Rule type chooser

| Requirement | Choose |
|---|---|
| My own KQL on a schedule | Scheduled analytics rule |
| My own KQL at the lowest latency | NRT analytics rule |
| Correlate multistage signals with no query | Fusion |
| Incident from a connected Microsoft product alert | Microsoft Security rule |
| Learn normal behaviour and flag outliers | ML behavior analytics / Anomaly |
| Match known malicious indicators | Threat intelligence rule / TI matching |
| Turn a hunting query into automatic device isolation | XDR **custom detection** with a response action |
| Triage incidents with no code | Sentinel **automation rule** |
| Call Teams, ServiceNow, or an external API | Sentinel **playbook** (Logic App) |

## Common distractors

| Trap | Correction |
|---|---|
| "Defender for Endpoint correlates email and identity alerts" | It does not; Defender XDR is the correlation layer. |
| "Sentinel replaces Defender XDR correlation" | Sentinel aggregates; XDR correlates the workload signals first. |
| "Suppression reduces incident count" | Suppression reduces **alerts**; grouping reduces **incidents**. |
| "Grouping and suppression are the same" | Different features with different effects — read the verb in the stem. |
| "Alert grouping stops the query running" | Only suppression does that. |
| "Isolate then offboard the device" | Offboarding removes telemetry and evidence; never a containment step. |
| "Delete the account to contain a compromise" | Disable and revoke sessions; deletion destroys evidence and may be reversible only for 30 days. |
| "Restore from backup and put the server back" | Reinfection is almost certain until the entry point and credentials are fixed. |
| "A scan proves the device is clean" | It does not; collect evidence and rebuild. |
| "Enable every incident-creation rule for Defender products" | Duplicate incidents — disable Microsoft incident creation for integrated products. |
| "Increase retention to fix a slow query" | Retention changes storage, not query cost; bound the time range. |
| "NRT accepts any query complexity" | NRT is for simple, low-latency queries. |
| "Fusion uses your query" | Fusion does not accept custom queries. |
| "A watchlist stores detection logic" | Watchlists store reference data. |
| "Isolating a device blocks live response" | Full isolation still allows live response and the Defender service. |

## Scenario words and what they signal

| Wording | Points to |
|---|---|
| "without alerting the user" / "preserve evidence" | Insider case: preserve, involve HR and legal, litigation hold. |
| "must not pay the ransom" | Contain, preserve, find entry point, restore from clean backup. |
| "single sign-on to every app" / "permissions" | App governance, consent grant, Conditional Access. |
| "reports the email and entered credentials" | Identity incident: reset, revoke sessions, review rules and sign-ins. |
| "one attempt per account" | Password spray. |
| "many attempts on one account" | Brute force. |
| "hourly buckets" / "trend" | `bin(TimeGenerated, 1h)` + `render timechart`. |
| "top five accounts" | `summarize` then `top 5 by`. |
| "one row per device with the latest event" | `arg_max(TimeGenerated, *) by DeviceName`. |
| "reference list used in many queries" | Watchlist. |
| "look back six months cheaply" | Search job (or restore for a deep dive). |
| "same query across five vendors" | ASIM parsers. |
| "must fire in under a minute" | NRT rule. |
| "one incident per host, not per alert" | Alert grouping by entity. |

## Time and question strategy

| Item | Guidance |
|---|---|
| Pace | 50 questions in 100 minutes ≈ **2 minutes** each; case questions deserve slightly more. |
| Multi-answer | All-or-nothing. Read whether it says *Select two* or *Select three* and count your selections. |
| Long stems | Read the **last sentence** first; it names the decision. Then find the constraint (cost, latency, least privilege). |
| "Least effort" stems | Prefer the built-in feature over a custom query or script. |
| Two plausible answers | The one that satisfies the stated constraint wins; the other usually solves a different problem. |
| Flag and move | Never spend more than four minutes on one item; the review pass is where the flagged items get a second look. |
| Case studies | Read the requirements section twice — the answer usually hinges on one stated constraint. |
| Last five minutes | Confirm no blank answers; there is no penalty for a guess. |

## Final-week checklist

| Check | Why |
|---|---|
| Can you name the sensor placement for Defender for Identity? | It is the most common identity question. |
| Can you tell alert from incident, and grouping from suppression? | Both appear in every sitting. |
| Can you write `summarize … by bin()` and `arg_max` from memory? | They cover most KQL items. |
| Do you know which tables hold sign-ins, audit, mailbox, and endpoint events? | Table-choice questions are quick marks. |
| Can you order the incident response phases? | Scenario stems test sequence. |
| Do you know the automation-rule versus playbook split? | Recurring Sentinel question. |
| Have you drilled all three case studies in the simulator? | Context-holding is a skill, not knowledge. |

**Exam tip:** when two answers both work technically, choose the one that uses the **native** control and the **least** privilege — that is how the live exam is written.

*Source map: all SC-200 learning paths, distilled into decision rules.*
