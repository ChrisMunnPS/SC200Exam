<div align="center">

# 🛡️ SC-200 Exam Simulator

### Microsoft Security Operations Analyst · 221 questions · 3 case studies · runs entirely in the browser

[![Live site](https://img.shields.io/badge/%E2%96%B6%20Take%20the%20exam-chrismunnps.github.io-0d9488?style=for-the-badge)](https://chrismunnps.github.io/exam-sim/sc-200/)
[![Questions](https://img.shields.io/badge/questions-221-2563eb?style=for-the-badge)](#-question-bank)
[![Pass mark](https://img.shields.io/badge/pass%20mark-700%20%2F%201000-7c3aed?style=for-the-badge)](#-scoring-and-review)
[![No account](https://img.shields.io/badge/signup-not%20required-16a34a?style=for-the-badge)](#-privacy)

**▶ Live exam: https://chrismunnps.github.io/exam-sim/sc-200/**

</div>

---

## 🚀 Executive summary

A free, full-length practice exam for **Exam SC-200: Microsoft Security Operations Analyst**, built from the Microsoft Learn SC-200 learning paths. It recreates the real sitting closely enough to be useful: a weighted 50-question draw, a 100-minute clock, scaled scoring against the 700 pass mark, three multi-question case studies, and eight architecture diagrams used by twenty-two items.

The bank is 221 questions split across Defender XDR, Microsoft Sentinel, detection engineering with KQL, and incident investigation and response — the four skills the exam actually tests, in the order the learning paths teach them.

Everything runs client-side. There is no account, no install, no server, no analytics, and no network call after the page loads — scores and review history stay in the visitor's own browser.

The same seven files power both the repository and the live site, so what you test locally is exactly what is published:

> **🌐 Live site: https://chrismunnps.github.io/exam-sim/sc-200/**
> Served by GitHub Pages from the portfolio repo's `exam-sim/sc-200/` folder — the same seven runtime files in this repository are the deployed build.

It is an independent study tool. It is not affiliated with, endorsed by, or connected to Microsoft.

---

## ✨ Why this one is different

🧭 **It behaves like the real exam.** Fifty questions drawn by domain weight (14 / 16 / 11 / 9), 120 seconds each, a running clock, flag-for-review, a question palette, and submit-on-timeout. No "practice mode" shortcuts unless you ask for them.

🧠 **Explanations teach, not just score.** Every item carries a rationale, one sentence on each wrong option, an exam tip, and the Microsoft Learn module and unit it came from.

🧩 **Three case studies.** Twenty-one questions hang off shared scenarios — Contoso Manufacturing's ransomware response, Fabrikam Financial's credential attack, and Tailwind Logistics' insider data theft — so you practise holding context across a case, the way the real exam does.

🖼️ **Architecture you can actually see.** Eight inline SVG diagrams cover the XDR correlation layer, the Sentinel ingestion pipeline, automation rules and playbooks, the hunting loop, the ATT&CK chain, Kerberos and NTLM credential abuse, the investigation graph, and the ransomware kill chain. Every diagram item also explains the figure in words, so it survives a broken image or a screen reader.

✅ **Multi-answer items are all-or-nothing.** Twenty-five "select two/three" questions are graded exactly as strictly as the exam grades them.

🔍 **Four domains, four drills.** Full 50, Defender XDR 14, Sentinel 16, KQL 11, Response 9, Quick 10, and a seven-question drill for each case.

🔁 **A spaced-review queue.** Wrong answers come back on a schedule instead of disappearing, so weak domains get revisited without re-running whole exams.

🌓 **Light and dark themes.** A top-bar switch, remembered per browser, following the OS until you choose. Printing always forces black-on-white.

⌨️ **Keyboard-first.** `A`–`D` to answer, arrows to navigate, `F` to flag, plus a visible focus ring and a skip link.

📴 **Genuinely offline.** Double-click the page and it works with the network unplugged. No bundler, no local server, no runtime dependencies.

---

## 🎯 Exam at a glance

| | |
|---|---|
| **Certification** | Microsoft Certified: Security Operations Analyst Associate |
| **Exam** | SC-200 — Microsoft Security Operations Analyst |
| **Questions in bank** | 221 (200 standalone + 21 case-study) |
| **Default attempt** | 50 questions, weighted by domain |
| **Time limit** | 120 seconds per question — 100 minutes for the default draw |
| **Passing score** | 700 / 1,000 (scaled from percentage correct) |
| **Review** | Every question after submission, with explanations and sources |
| **Prerequisites** | A browser. That is the whole list. |

---

## 📚 Question bank

| Domain | Questions | Weight | IDs |
|---|---:|---|---|
| Defender XDR | 60 | 25–30% | Q1–Q54, Q201–Q204, Q208, Q209 |
| Microsoft Sentinel | 70 | 30–35% | Q55–Q118, Q210–Q213, Q218, Q219 |
| Detection & KQL | 50 | 20–25% | Q119–Q165, Q214, Q220, Q221 |
| Investigation & response | 41 | 15–20% | Q166–Q200, Q205–Q207, Q215–Q217 |
| **Total** | **221** | | |

The weights are the midpoints of the official skills-measured ranges, re-cut onto the learning paths so each section is a clean drill boundary. Case-study items are counted inside their primary domain.

**Question mix:** 45% applied scenario, 30% multi-step analysis, 25% foundational recall · 11.3% multi-answer · 10.0% diagram-based · 22.6% KQL.

### 🗂️ Case studies

| Case | Questions | Scenarios |
|---|---|---|
| Contoso Manufacturing — ransomware response | Q201–Q207 | Attack-chain stage, Kerberoasting sensor, unified incident source, hash blocking, reinfection, tamper protection, family intelligence |
| Fabrikam Financial — credential attack | Q208–Q214 | OAuth consent after spray, lateral movement paths, required log sources, ASIM for VPN, board reporting, BEC automation, inbox-rule query |
| Tailwind Logistics — insider data theft | Q215–Q221 | Evidence preservation, exfiltration sources, mailbox hold, Cloud Apps connector, 18-month search job, hunting findings, mass-download threshold |

---

## 📄 Cheat sheets

Five one-page A4 sheets, written from the same course snapshot as the bank:

| Sheet | Covers |
|---|---|
| `04-CHEAT-SHEET-XDR.md` | Defender XDR, Endpoint, Identity, Office 365, Cloud Apps, Cloud |
| `05-CHEAT-SHEET-SENTINEL.md` | Workspace, connectors, retention, analytics rules, automation, hunting, UEBA |
| `06-CHEAT-SHEET-KQL.md` | Operators, aggregations, joins, table map, performance, patterns |
| `07-CHEAT-SHEET-RESPONSE.md` | Response phases, containment per workload, evidence, attack patterns, ATT&CK |
| `08-CHEAT-SHEET-EXAM-DAY.md` | Numbers, "which tool?", rule chooser, distractors, scenario words, pacing |

`09-CHEAT-SHEETS.html` bundles all five into a print pack — open it, press **Print all five sheets**, and each sheet lands on its own page with no reflow.

---

## 🖥️ Run it locally

**The short version:** download the repository and double-click `index.html`.

```powershell
git clone https://github.com/ChrisMunnPS/SC200Exam.git
```

```text
index.html        – page shell, theme bootstrap
exam.config.js    – pass mark, domains, weights, timer
questions.js      – 221 compiled questions (the whole bank)
diagrams.js       – 8 inline SVG diagrams
app.js            – exam engine
styles.css        – shared theme, light + dark
md-loader.js      – data bootstrap and zero-setup fallback
```

No `npm install`. No build step. No web server. The page opens from `file://`, reads the compiled bank through a plain `<script>` tag, and starts.

> 💡 **Why not Markdown at runtime?** A double-clicked page is not allowed to `fetch` other local files — that rule applies to every `file://` URL. Compiling the Markdown into `questions.js` once is what makes the offline copy work with no setup prompt.

If you only want to check the bank without opening the UI, run `node tools\build-questions.mjs --check` from the authoring kit (not included in this repository).

---

## 🧮 Scoring and review

📊 **Scaled score.** `round(correct / total * 1000)` — the same shape Microsoft uses in the score report. Thirty-five out of fifty is a pass.

🎆 **Passing feels like passing.** Clearing 700 adds a five-second fireworks overlay to the score report. It is skipped automatically when the operating system asks for reduced motion, and never appears in print.

🔍 **Review everything.** After submitting, filter by all / incorrect / flagged / unanswered. Each item shows your answer, the correct answer, the full explanation, the distractor notes, the exam tip, and the source module.

📈 **Section breakdown.** The score report charts all four domains, marks the 700 line on each bar, and links straight into the filtered review for that section.

📄 **Print or save.** "Print full review" and "Print score report" produce clean paper layouts with the light palette forced, so a dark-mode session still prints readable black text.

💾 **Resume safe.** An in-progress attempt is saved on every change. Close the tab, come back, and the clock and answers are where you left them.

---

## 🔐 Privacy

Everything is local. The simulator writes four keys to `localStorage` and nothing else:

| Key | Holds |
|---|---|
| `sc200-sim:attempt` | The in-progress attempt, for resume |
| `sc200-sim:settings` | Draw size, timer, instant-feedback preference |
| `sc200-sim:srs` | Spaced-review scheduling data |
| `sc200-sim:theme` | Light or dark choice |

No cookies, no requests to third parties, no telemetry. Clearing site data for the page resets the simulator completely.

---

## ♿ Accessibility

- Full keyboard operation, including `A`–`D` answer keys and arrow navigation.
- Semantic landmarks, a skip link, and a polite live region for toasts.
- Visible focus rings and readable contrast in both themes.
- `prefers-reduced-motion` disables the fireworks and shortens transitions.
- Diagrams are hidden from assistive technology; every diagram question restates the figure in prose.

---

## 🛠️ For maintainers


## ⚠️ Disclaimer

Unofficial study material built from publicly available Microsoft Learn content. Not affiliated with or endorsed by Microsoft. Exam objectives and product behaviour change; verify the current skills measured on Microsoft Learn before sitting the exam. Facts snapshot: **November 2026**.

<div align="center">

**[▶ Start practising](https://chrismunnps.github.io/exam-sim/sc-200/)** · [Microsoft Learn SC-200](https://learn.microsoft.com/en-us/credentials/certifications/security-operations-analyst/)

</div>
