# operation-iron-watch-01
Tactical Blue Team campaign conducted within Sentinel SOC Lab. This operation simulates controlled reconnaissance and attack scenarios against soc-core, validating Snort detection capabilities, log integrity, and early-stage correlation workflows. Objective: Establish reliable detection foundations before SIEM integration.


Tactical Blue-Team Campaign

## Mission

Operation Iron Watch 01 establishes the foundational defensive monitoring framework of Sentinel SOC Lab.

The objective is to validate:

- Network visibility
- Snort-based detection capability
- Log ingestion reliability
- Manual correlation discipline

This campaign does not focus on complex exploitation.
It focuses on defensive readiness, detection validation, and structured analysis methodology.

---

## Scope

Assets involved:

- soc-core (Ubuntu Server 22.04 – Detection Node)
- redforge-02 (Isolated Internal Attacker – Host-Only)
- redforge-01 (External Exposure Attacker – Bridged LAN)

Detection Stack:

- Snort IDS
- rsyslog ingestion
- auth.log monitoring
- Manual correlation analysis

---

## Tactical Philosophy

Iron Watch represents:

- Persistent monitoring
- Controlled testing
- Clean detection resets before every scenario
- Evidence-based validation

No noise.
No uncontrolled attack behavior.
Structured execution only.
