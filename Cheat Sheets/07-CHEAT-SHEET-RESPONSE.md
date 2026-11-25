# SC-200 cheat sheet 4/5 — Investigate and respond

*One page. Facts snapshot 2026-11; verify current behaviour on Microsoft Learn before the exam.*

## Incident response phases

| Phase | What happens | SC-200 tools |
|---|---|---|
| Prepare | Playbooks, detection coverage, access tiers, communication plan. | Automation rules, incident tasks, Sentinel content hub. |
| Detect and triage | Validate, classify, assign, scope the affected entities. | Incident queue, attack story, entity pages, UEBA. |
| Contain | Stop spread across **every** compromised path: endpoint, identity, app, network. | Device isolation, session revocation, password reset, indicator block, playbook. |
| Eradicate | Remove malware, persistence, web shells, rogue accounts, vulnerable entry points. | Live response, AIR, ASR rules, patching. |
| Recover | Restore service, verify clean, monitor for reinfection. | Backups, tamper protection, watchful rules. |
| Lessons learned | Timeline, root cause, detection gaps, control changes. | Rule tuning, new detections, playbook changes. |

## Triage and scoping

| Item | What to remember |
|---|---|
| First question | Is this real, and how far has it spread? Scope before you contain. |
| Scope by campaign | Identify every recipient, click, device, and account touched — not just the first report. |
| Correlate entities | Users, hosts, IPs, mailboxes, apps, and cloud resources appear in the attack story and graph. |
| Identify the entry point | Phishing, exposed service, valid credentials, or a supply chain — recovery fails without this. |
| Business context | Asset value, data classification, regulatory obligation, and blast radius of the response itself. |
| Severity discipline | High severity is for urgent, high-impact items; over-grading destroys queue meaning. |

## Containment actions by workload

| Workload | Actions |
|---|---|
| Endpoint | **Full isolation** · **selective isolation** (allow list) · **restrict app execution** · **live response** · **antivirus scan** · **collect investigation package** · **initiate automated investigation**. |
| Identity | Disable account, revoke refresh tokens/sessions, reset password, require re-registration of MFA, remove device compliance, block sign-in. |
| Email | Soft delete or hard delete messages, move to quarantine, block sender/URL, purge campaign from all mailboxes, disable forwarding rules. |
| Cloud app | Revoke OAuth consent, disable the enterprise app, remove delegated permissions, session policy block. |
| Network | Block IP/URL/hash indicator, close the exposed port, restrict with Conditional Access or firewall policy. |
| Domain (AD) | Reset the account, reset **KRBTGT twice** after a Golden Ticket, rotate service account passwords, remove rogue DCs. |

## Evidence and forensics

| Item | What to remember |
|---|---|
| Order of volatility | CPU/cache and memory → running processes and network connections → disk → remote logs → backups. |
| Chain of custody | Who collected what, when, from where, hash, storage location, and every transfer or access. |
| Hashing | Hash on collection and re-verify at analysis; a broken chain can invalidate evidence. |
| Investigation package | Defender for Endpoint forensic snapshot — collect **before** reimaging or wiping. |
| Timeline | Normalise every source to one time zone (**UTC**) and order chronologically before drawing conclusions. |
| Log sources | Sign-in and audit logs (identity), mailbox audit (email), endpoint tables (device), firewall/proxy (network), cloud app audit (SaaS). |
| Retention | Analytics 30 days–2 years; long-term up to 12 years via search job or restore when the investigation spans months. |
| Legal hold | Block sign-in but **hold** the mailbox and files; never delete when a tribunal or regulator may need them. |

## Attack patterns

| Pattern | Signature | Response |
|---|---|---|
| Password spray | One or few passwords against **many** accounts, low attempt count each. | Block source, require MFA, reset any successes, review sign-in risk. |
| Brute force | Many passwords against **one** account. | Lockout, block IP, reset, check for success after failures. |
| Credential stuffing | Replayed breached user/password pairs across sites. | Force reset, check breach exposure, enable MFA. |
| Pass-the-Hash | NTLM hash reused; no password needed. | Reset the account, reset machine account, hunt for LSASS access. |
| Pass-the-Ticket | Valid Kerberos ticket reused. | Revoke tickets, reset account, review delegation. |
| Kerberoasting | Burst of service-ticket requests, often RC4, cracked offline. | Rotate service account passwords to long managed secrets, review SPNs. |
| Golden Ticket | Forged TGT signed with KRBTGT; long lifetime. | Reset KRBTGT **twice**, reset privileged accounts, rebuild trust. |
| Business email compromise | Hidden inbox rule, forwarding, payment or payroll change requests. | Remove rules, revoke sessions, reset password, purge messages, notify finance. |
| Illicit OAuth consent | App granted mail or file permissions, often from a phishing lure. | Revoke consent, disable app, remove rules, reset credentials. |
| Ransomware | Mass file rename/encryption, shadow copies deleted, ransom note. | Isolate, disable accounts, preserve evidence, find entry point, restore from clean backup. |
| Command and control | Regular small beacons to a rare domain or IP. | Isolate, block indicator, identify the spawning process. |
| Insider data theft | Off-hours mass download, personal cloud upload, departing employee. | Preserve evidence, involve HR and legal, DLP and Cloud Apps evidence, litigation hold. |
| Impossible travel | Two distant sign-ins in a short window. | Rule out VPN/proxy egress before declaring compromise. |

## MITRE ATT&CK tactics

| Tactic | Means |
|---|---|
| Reconnaissance | Gathering information before attacking. |
| Resource Development | Building infrastructure, accounts, and capabilities. |
| Initial Access | Getting the first foothold (phishing, exposed service, valid accounts). |
| Execution | Running attacker code (PowerShell, scripts, macros). |
| Persistence | Surviving restarts and credential changes (run keys, tasks, services). |
| Privilege Escalation | Gaining higher rights on the same host. |
| Defense Evasion | Avoiding detection (disabling logging, tampering, masquerading). |
| Credential Access | Stealing secrets (LSASS, Kerberoasting, keylogging). |
| Discovery | Enumerating users, groups, shares, and systems inside the environment. |
| Lateral Movement | Moving from one host to another. |
| Collection | Gathering data for exfiltration. |
| Command and Control | Communicating with compromised systems. |
| Exfiltration | Getting data out. |
| Impact | Destructive or disruptive action (encryption, wipers, DoS). |

## After-action work

| Item | What to remember |
|---|---|
| Measure | Time to detect, time to contain, scope accuracy, and percentage of incidents auto-triaged. |
| Close the gap | Every missed behaviour becomes a new or tuned analytics rule, tested before enabling. |
| Tune safely | Add exclusions with justification; never disable a rule wholesale to silence noise. |
| Document | Incident summary = timeline + affected entities + root cause + actions + residual risk. |
| Communicate | A defined plan with named roles, cadence, and escalation to legal and regulators. |

**Exam tip:** exam scenarios reward *sequence* — validate, scope, contain, eradicate, recover, review. Answer choices that skip a step are usually wrong.

*Source map: Investigate and respond to security incidents, Perform threat hunting in Microsoft Sentinel, Defender workload modules.*
