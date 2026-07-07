# TRACE — Threat Response & Case Evidence System

A single-file, offline-first incident response case management system for DFIR and cyber threat intelligence work. Everything runs in your browser — no server, no install, no data leaving your machine.

TRACE helps incident responders manage the full lifecycle of a security incident: evidence and chain of custody, indicators of compromise with MITRE ATT&CK tagging, response actions, regulatory notifications, an accurate incident timeline, and structured closure — all aligned to the NIST incident response lifecycle.

![TRACE dashboard](docs/trace-dashboard.png)

---

## Contents

- [Why TRACE](#why-trace)
- [Quick start](#quick-start)
- [The sample case](#the-sample-case)
- [Feature overview](#feature-overview)
- [Data & privacy](#data--privacy)
- [Saving your work](#saving-your-work)
- [Working as a team](#working-as-a-team)
- [Exports & integrations](#exports--integrations)
- [Themes](#themes)
- [Browser support](#browser-support)
- [File structure](#file-structure)
- [FAQ](#faq)
- [Disclaimer](#disclaimer)

---

## Why TRACE

Most incident response tooling is either a heavyweight platform that needs infrastructure and onboarding, or a scattering of spreadsheets and documents that fall out of sync. TRACE sits in between: a single HTML file you can drop onto a workstation, an air-gapped analysis box, or a USB stick, and start logging an incident in seconds.

Because it is one self-contained file with no back end, it is well suited to:

- Sensitive incidents where data must not leave the local machine
- Air-gapped or restricted forensic environments
- Tabletop exercises and training
- Small teams who want structure without standing up a platform

It captures the artefacts that matter for a defensible investigation — evidence integrity hashes, chain of custody, IOC provenance, notification deadlines, and analyst attribution — and lets you export them in the formats downstream tools expect.

---

## Quick start

1. **Download** `ir-cms.html`.
2. **Open it** in a modern browser (Chrome or Edge recommended — see [Browser support](#browser-support)).
3. Click **New case** and complete the short intake wizard, **or** load the [sample case](#the-sample-case) to explore a fully worked example.
4. Click **? Guide** (top right) at any time for an in-app walkthrough.

That's it. There is nothing to install and no account to create.

> **Tip:** For the safest setup, at the start of each incident click **Connect workspace** to link a file on disk (continuous auto-save) and **Set name** to enable analyst attribution. See [Saving your work](#saving-your-work).

---

## The sample case

`trace-sample-case.json` is a comprehensive, realistic worked example: a **BlackCat/ALPHV ransomware incident** on a finance file server (`IR-2026-014`). It is designed to exercise every feature of the system, so it is useful both for evaluating TRACE and for training.

**To load it:** open TRACE, click **Load file** in the sidebar, and select `trace-sample-case.json`.

The sample includes:

- A full incident narrative and complete governance block (incident commander, legal, DPO, insurer, external IR retainer, business impact, reputational risk)
- **5 evidence items** with SHA-256 integrity hashes and **4 chain-of-custody entries** (acquire → transfer → analyse)
- **7 IOCs** across multiple types (IPv4, account, hashes, domain, filename) covering five MITRE ATT&CK tactics, each with an observed-at time so the timeline is forensically accurate
- **6 response actions**, including two recurring tasks (a twice-daily executive briefing and daily leak-site monitoring) with completed instances logged
- **6 notifications** — five sent, one pending — spanning regulator, internal, legal, and insurance obligations with real deadlines
- A severity escalation, three root causes with remediations, and a partially completed closure checklist
- Two analysts stamped throughout, so attribution and the merge workflow have realistic data

Loading the sample **merges** it alongside any cases you already have — it will not overwrite your existing work.

---

## Feature overview

TRACE is organised into tabs, aligned to the phases of an incident.

**Dashboard** — an at-a-glance view of the active case: key metrics (evidence, time elapsed, actions, IOCs, notifications, closure progress), a clickable timestamp strip for setting detection/report/containment/resolution times, open action and notification summaries, recent activity, and a mini incident timeline.

**Incident details** — core incident metadata, narrative, affected systems, and severity/status.

**Evidence** — the evidence register. Log each item with type, integrity hash, source, and who collected it and when.

**Custody** — chain-of-custody entries linked to evidence items, recording each handling event (acquired, transferred, analysed, returned) for a defensible audit trail.

**IOCs** — indicators of compromise with inline **MITRE ATT&CK** technique lookup (search by what you observed — e.g. "powershell", "lsass", "scheduled task"). Each IOC records an **observed-at** time (when it was seen, distinct from when it was logged) so the timeline reflects reality. IOCs can be viewed as a flat list or grouped **by host**.

**Actions** — response tasks with owner, priority, due date, and status. Supports **recurring** actions (e.g. twice-daily briefings) with per-instance completion tracking.

**Timeline** — a unified incident timeline combining evidence, custody, IOCs, actions, escalations, notifications, and phase milestones (detected/reported/contained/resolved). Filter by type; switch between list and visual views.

**More** menu — governance context, severity log, notifications tracker, closure checklist, printable report, and an IR reference.

### Governance & compliance

- **Notifications tracker** with deadline calculation from detection time, distinguishing regulatory obligations (ICO/UK GDPR, NIS2, DORA, and others) from internal and advisory notifications, with sent/pending status
- **Closure checklist** covering containment, evidence, IOCs, recovery, actions, and notifications
- NIST incident response lifecycle alignment surfaced throughout

---

## Data & privacy

TRACE runs **entirely in your browser**. There is no server and no telemetry — nothing you enter is transmitted anywhere. Your data lives in two places:

1. **Browser storage** — TRACE automatically mirrors your work to local browser storage on every change, so an accidental tab close or crash will not lose data.
2. **A workspace file** (optional but recommended) — a real `.json` file on your disk that TRACE keeps continuously up to date. See below.

---

## Saving your work

There are two ways to persist a case, and they serve different purposes.

### Connect workspace (recommended)

Links TRACE to a `.json` file on your computer and **auto-saves to it on every change**. Within a second or two of any edit, the file on disk is current — so a crash or reboot costs you nothing.

- Click **Connect workspace** → **Create new** to choose a location and filename, or **Open existing** to resume auto-saving into a file you made earlier.
- A green indicator confirms "Auto-saving to *[file]*" once connected.
- After a full browser restart, click **Connect → Open** once to re-link the file (browsers drop the file handle when closed). Between launches you remain protected by browser storage.

### Save to file

A one-time snapshot export you trigger manually — useful for handing someone a copy, archiving a point-in-time state, or moving a case between machines.

**In short:** use **Connect workspace** for day-to-day durability; use **Save to file** for snapshots and sharing.

---

## Working as a team

TRACE supports multi-analyst incidents without needing a shared server.

### Analyst identity

Click **Set name** to record your name and pick an avatar colour. Every record you add — evidence, IOCs, actions, custody — is **stamped** with who added it and when, so on a shared case you can see who logged what.

### Merge analyst copy

For team incidents where analysts can't all edit the same file live, each analyst works on their own copy of a case. One person then clicks **Merge analyst copy** and selects a colleague's file. TRACE **combines** the two copies of the case intelligently:

- New evidence, IOCs, actions, and other records from their copy are **added** to yours
- Where both analysts edited the same record, the **most recently edited version wins**
- Completed recurring tasks, closure checklist items, and "notification sent" flags are **combined** — if either analyst did it, it counts

Nothing is ever overwritten destructively; everyone's work is preserved. The merge is safe to run repeatedly (it will not create duplicates) and accumulates correctly across multiple analysts. You receive a summary of what was added and updated.

---

## Exports & integrations

TRACE exports case data in the formats downstream tools expect. Export buttons are in the top bar and the IOC tab.

| Export | Format | Use |
|---|---|---|
| **STIX 2.1** | JSON bundle | Share indicators and observed data with threat-intel platforms |
| **ATT&CK Navigator** | Navigator layer JSON | Visualise tagged techniques on the MITRE ATT&CK matrix (current ATT&CK version) |
| **CSV** | Single spreadsheet | All artefacts (evidence, custody, IOCs, actions, escalations, notifications, root causes) flattened into one CSV for spreadsheets or SIEM import |
| **Report** | Printable HTML | A formatted incident report for stakeholders |
| **JSON** | TRACE workspace | Full-fidelity backup, sharing, and the basis for the merge workflow |

The CSV export uses proper quoting/escaping and a UTF-8 byte-order mark so it opens cleanly in Excel. The ATT&CK Navigator layer and STIX exports use standard STIX tactic shortnames so techniques land in the correct columns.

---

## Themes

Cycle the **Theme** button through three options; your choice is remembered:

- **Midnight** *(default)* — dark navy security-console aesthetic
- **Daybreak** — a warm, muted light theme (parchment, heritage green/amber/blue)
- **Console** — a green-on-black terminal look

---

## Browser support

TRACE works in any modern browser, but a few features rely on the **File System Access API**:

- **Connect workspace** (continuous auto-save to a disk file) requires a **Chromium-based browser** (Chrome or Edge).
- In browsers without this API (e.g. Firefox, Safari), TRACE still works fully — it falls back to browser storage plus manual **Save to file** / **Load file**. The **Connect workspace** option is hidden where unsupported.

Everything else — all tabs, exports, themes, merge, and the sample case — works everywhere.

---

## File structure

| File | Description |
|---|---|
| `ir-cms.html` | The complete application — a single self-contained HTML file. This is all you need to run TRACE. |
| `trace-sample-case.json` | A comprehensive worked example (ransomware incident) to load and explore. |
| `README.md` | This guide. |

The application is a single file with no build step and no external dependencies beyond a web font loaded from Google Fonts (it degrades gracefully to system fonts offline).

---

## FAQ

**Do I need to install anything or run a server?**
No. Open `ir-cms.html` in a browser and you're running.

**Is my data sent anywhere?**
No. Everything stays in your browser and, if you connect a workspace, in a file on your own disk.

**Will loading the sample case overwrite my work?**
No — loading and merging combine cases; they never destructively replace your existing cases.

**Can several people work on one incident?**
Yes. Each analyst keeps their own copy and one person merges the others in with **Merge analyst copy**. Records are attributed to whoever added them.

**What happens to my workspace connection after I close the browser?**
Browser storage keeps your data safe between launches. To resume live auto-save to your disk file, click **Connect → Open** once after restarting.

**Which browser should I use?**
Chrome or Edge for the full experience (including workspace auto-save). Others work with manual save/load.

---

## Disclaimer

TRACE is a case-management and record-keeping aid. It does not provide legal advice. Regulatory notification deadlines and obligations shown in the tool are aids to tracking and must be verified against the applicable regulations and your organisation's legal counsel for any real incident. You are responsible for the security and handling of the case data you enter and export.
