# Network Security — Change Communications Playbook

This document defines **when** and **where** the Network Security team should
send communications ("comms") for any change it makes (firewall rules, VPN,
segmentation, IDS/IPS, proxy/DNS, certificates, access policies, tooling, etc.).

> The flowchart is the source of truth for the decision flow. The tables below
> spell out timing, owners, and channels. Assumptions are listed at the bottom —
> edit them to match your org.

---

## 1. Decision flow — when & where to send comms

```mermaid
flowchart TD
    Start([Change identified by Network Security]) --> Q1{"Is this an<br/>emergency / security<br/>incident-driven change?"}

    %% Emergency path
    Q1 -- Yes --> EMG[/"Tier 0 - EMERGENCY"/]
    EMG --> EMG_BEFORE["BEFORE (ASAP, may be concurrent):<br/>• Notify Incident Commander + Security leadership<br/>• Open #sec-incident bridge"]
    EMG_BEFORE --> EMG_DURING["DURING:<br/>• Post to Status Page (if user-facing)<br/>• #network-security + #all-engineering<br/>• Email exec/stakeholder DL"]
    EMG_DURING --> EMG_AFTER["AFTER (within 24h):<br/>• Post-incident review notice<br/>• Email change summary to stakeholders"]
    EMG_AFTER --> Done

    %% Non-emergency
    Q1 -- No --> Q2{"Customer-facing or<br/>cross-team impact?<br/>(downtime, latency,<br/>blocked access)"}

    Q2 -- "High impact" --> MAJ[/"Tier 1 - MAJOR CHANGE"/]
    Q2 -- "Low / limited impact" --> Q3{"Any external or<br/>internal user visible<br/>effect at all?"}

    Q3 -- Yes --> STD[/"Tier 2 - STANDARD CHANGE"/]
    Q3 -- No --> INFO[/"Tier 3 - INFORMATIONAL / ROUTINE"/]

    %% Major path
    MAJ --> MAJ_CAB["1. Submit to CAB / Change Approval<br/>(get approval first)"]
    MAJ_CAB --> MAJ_BEFORE["BEFORE — >= 5 business days notice:<br/>• Email change-announce DL + affected teams<br/>• #network-security + #change-management<br/>• Add to Status Page maintenance calendar"]
    MAJ_BEFORE --> MAJ_REMIND["REMINDER — 24h & 1h before:<br/>• Slack reminder in impacted channels<br/>• Status Page 'scheduled maintenance'"]
    MAJ_REMIND --> MAJ_DURING["DURING:<br/>• Status Page -> In Progress<br/>• #change-management start/stop messages"]
    MAJ_DURING --> MAJ_AFTER["AFTER:<br/>• Status Page -> Resolved<br/>• Completion email to stakeholders"]
    MAJ_AFTER --> Done

    %% Standard path
    STD --> STD_BEFORE["BEFORE — >= 2 business days notice:<br/>• Email affected team(s) / owners<br/>• #network-security announcement"]
    STD_BEFORE --> STD_DURING["DURING:<br/>• Brief #network-security start/stop note"]
    STD_DURING --> STD_AFTER["AFTER:<br/>• Confirmation in #network-security<br/>• Update ticket / change record"]
    STD_AFTER --> Done

    %% Informational path
    INFO --> INFO_LOG["No proactive comms required:<br/>• Log in change record / ticket<br/>• Optional FYI in #network-security<br/>• Rolls up into weekly change digest"]
    INFO_LOG --> Done

    Done([Change recorded in change log / CMDB])

    classDef emg fill:#ffd6d6,stroke:#c00,color:#000;
    classDef maj fill:#ffe9c7,stroke:#d68000,color:#000;
    classDef std fill:#d6e8ff,stroke:#06c,color:#000;
    classDef info fill:#e2e2e2,stroke:#666,color:#000;
    class EMG,EMG_BEFORE,EMG_DURING,EMG_AFTER emg;
    class MAJ,MAJ_CAB,MAJ_BEFORE,MAJ_REMIND,MAJ_DURING,MAJ_AFTER maj;
    class STD,STD_BEFORE,STD_DURING,STD_AFTER std;
    class INFO,INFO_LOG info;
```

---

## 2. When to send — timing matrix

| Tier | Change type | Advance notice (BEFORE) | Reminders | During | After |
|------|-------------|-------------------------|-----------|--------|-------|
| **0 — Emergency** | Incident-driven, active threat, urgent patch | ASAP / concurrent | n/a | Real-time on bridge + Status Page | Post-incident review ≤ 24h |
| **1 — Major** | Downtime, broad/customer impact | ≥ 5 business days | 24h + 1h before | Start/stop + Status Page | Completion notice |
| **2 — Standard** | Limited/known impact, scheduled | ≥ 2 business days | Optional 1h before | Brief start/stop note | Confirmation in channel |
| **3 — Informational** | No user-visible impact, routine | None proactive | n/a | n/a | Weekly digest entry |

---

## 3. Where to send — channels & audiences

| Channel / destination | Tier 0 | Tier 1 | Tier 2 | Tier 3 |
|-----------------------|:------:|:------:|:------:|:------:|
| Security leadership / Incident Commander | ✅ | — | — | — |
| Exec & stakeholder email DL | ✅ | ✅ | — | — |
| `change-announce@` email DL | — | ✅ | — | — |
| Affected team owners (direct) | ✅ | ✅ | ✅ | — |
| Public/internal **Status Page** | ✅ (if user-facing) | ✅ | — | — |
| CAB / Change Approval Board | — | ✅ (approval) | log only | log only |
| Slack `#sec-incident` | ✅ | — | — | — |
| Slack `#network-security` | ✅ | ✅ | ✅ | optional FYI |
| Slack `#change-management` | — | ✅ | — | — |
| Slack `#all-engineering` | ✅ | — | — | — |
| Change log / CMDB / ticket | ✅ | ✅ | ✅ | ✅ |
| Weekly change digest | ✅ | ✅ | ✅ | ✅ |

---

## 4. Ownership (who sends)

| Tier | Drafts comms | Approves | Sends |
|------|--------------|----------|-------|
| 0 — Emergency | On-call Net Sec engineer | Incident Commander | IC / Comms lead |
| 1 — Major | Change owner | Net Sec manager + CAB | Change owner |
| 2 — Standard | Change owner | Net Sec lead | Change owner |
| 3 — Informational | Change owner | n/a | Automated digest |

---

## 5. Assumptions (edit to fit your org)

- Four change tiers (0–3) based on urgency × impact.
- Channels named (`#network-security`, `change-announce@`, Status Page, CAB,
  CMDB) are placeholders — swap for your real tools (Slack/Teams, ServiceNow,
  Jira, Statuspage/Atlassian, etc.).
- "Business days" notice windows are typical CAB defaults; tune to your SLAs.
- A central **change log / CMDB** entry is required for *every* tier, including
  emergencies (logged retroactively).
