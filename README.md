# 🔐 Operation Iron Watch 01
## Foundational SOC — Snort IDS & Manual Correlation

[![Status](https://img.shields.io/badge/status-complete-brightgreen)](https://github.com/cyberlandji/operation-iron-watch-01)
[![Lab Series](https://img.shields.io/badge/series-Operation%20Iron%20Watch-blue)](https://github.com/cyberlandji)
[![Focus](https://img.shields.io/badge/focus-Snort%20IDS%20%26%20Detection%20Baseline-teal)](https://github.com/cyberlandji/operation-iron-watch-01)

---

## 📌 Overview

Operation Iron Watch 01 establishes the **foundational detection baseline** of the Iron Watch lab series.

Before introducing a SIEM, before automating correlation, before adding complexity — this operation answers a fundamental question: *can we reliably detect early-stage attacker behavior using only Snort IDS and manual log analysis?*

The environment was intentionally kept **noise-free and controlled** to clearly distinguish attacker signals from background activity. No benign traffic interference. No complex correlation. Just clean, observable attacker behavior against a known baseline.

---

## 🎯 Objectives

- Validate network visibility on the SOC detection node
- Validate Snort-based detection against real attacker activity
- Build manual correlation discipline before SIEM integration
- Establish a clean detection baseline for future operations

---

## 🖥️ Lab Infrastructure

| Host | Role | Network |
|------|------|---------|
| `soc-core` | Ubuntu Server 22.04 — detection node | Host-Only |
| `redforge-02` | Isolated attacker VM | Host-Only + NAT (updates only) |

> Both VMs run on the same physical host. NAT was used exclusively for system updates — never for attack traffic.

---

## 🛠️ Detection Stack

| Tool | Role |
|------|------|
| Snort IDS | Primary detection — network-level alerts |
| auth.log | Host-level authentication event monitoring |
| Manual correlation | Analyst-driven — no SIEM in this operation |

---

## 🔍 Observed Activity

All attacker activity observed during IW01 was limited to early-stage reconnaissance and failed initial access. No compromise occurred.

| Activity | Description |
|----------|-------------|
| ICMP Host Discovery | Ping used to confirm target availability before further probing |
| TCP Connect Scan (-sT) | Full TCP handshake scan — indicates low-privilege attacker (no raw socket access) |
| SSH Authentication Attempts | Repeated failed login attempts — no credentials obtained, no access granted |

> The absence of stealth techniques (no `-sS` scan, no sudo usage) is itself an indicator — consistent with a low-sophistication or opportunistic attacker profile.

---

## 🗺️ MITRE ATT&CK Mapping

| Technique | Tactic | Status |
|-----------|--------|--------|
| Active Scanning | Reconnaissance | ✅ Observed |
| Network Service Discovery | Reconnaissance | ✅ Observed |
| Valid Accounts (SSH) | Initial Access | ⚠️ Attempted — Failed |

**Techniques NOT observed:** Execution, Persistence, Privilege Escalation, Lateral Movement, Defense Evasion, Command & Control.

---

## 🔗 Attack Flow

```
[redforge-02]
     │
     │  ICMP Echo Request → confirm host availability
     ▼
[soc-core]
     │
     │  TCP Connect Scan (-sT) → service enumeration
     ▼
[Service Discovery]
     │
     │  SSH Authentication Attempts → failed, no access granted
     ▼
[Access Denied — Attack Chain Terminated]
```

---

## 📂 Structure

```
operation-iron-watch-01/
├── 01-architecture/        # Network topology & VM modes
├── 02-assets/              # VM settings and baseline notes
├── 03-campaign-scenario/   # SOC node configs, detection, correlation
├── 04-evidences/           # Logs and screenshots
├── 05-lessons-learned/     # Lessons per phase
└── README.md
```

---

## 🏁 Key Takeaway

IW01 proves that **early-stage attacker behavior is detectable** even without a SIEM — if the right tools are in place and the analyst knows what to look for. Manual correlation is slow but it forces deep understanding of what each log entry actually means.

> The limitation IW01 exposes: manual correlation does not scale. The next step is a SIEM.

---

## 🔗 Iron Watch Series

| Episode | Focus | Status |
|---------|-------|--------|
| **Iron Watch 01** | **Foundational SOC — Snort IDS, manual correlation** | ✅ Complete |
| [Iron Watch 02](https://github.com/cyberlandji/operation-iron-watch-02) | Graylog SIEM — web enumeration detection, real SSH compromise | ✅ Complete |
| [Iron Watch 03](https://github.com/cyberlandji/operation-iron-watch-03) | DMZ hardening, log pipeline, DDoS detection suite | 🔄 In Progress |
| Iron Watch 04 | Attack validation — Kali recon & initial access against IW03 | 🔜 Planned |

---

## 👤 Author

**cyberlandji** — Blue Team Practitioner | ISC2 CC | CompTIA Security+ (in progress)

Portfolio: [cyberlandji.com](https://cyberlandji.com) · GitHub: [github.com/cyberlandji](https://github.com/cyberlandji)

