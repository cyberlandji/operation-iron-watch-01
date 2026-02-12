# Lab Topology — Operation Iron Watch 01

Sentinel SOC Lab — Hybrid Defensive Architecture

---

## 1. Overview

Operation Iron Watch 01 is built on a hybrid lab model combining:

- Isolated detection testing (Host-Only network)
- Realistic exposure simulation (Bridged LAN)

The objective is to validate detection capability under controlled and semi-realistic conditions.

---

## 2. Assets Involved

### 1️⃣ soc-core
Role: Detection Node  
OS: Ubuntu Server 22.04 (No GUI)  
Purpose:
- Snort IDS monitoring
- rsyslog log ingestion
- Manual correlation analysis

---

### 2️⃣ redforge-02
Role: Internal Controlled Attacker  
OS: Kali Linux  
Network: NAT + Host-Only  

Purpose:
- Reproducible detection testing
- Controlled ICMP, Nmap, SSH tests
- Low-noise validation of Snort rules

---

### 3️⃣ redforge-01
Role: External Exposure Attacker  
OS: Kali Linux  
Network: Bridged  

Purpose:
- Simulate real LAN attacker
- Compare detection visibility
- Validate monitoring under realistic conditions

---

## 3. Logical Topology Diagram (Conceptual)

          [ redforge-01 ]
            (Bridged LAN)
                   |
          =======================
                Real LAN
          =======================
                   |
              [ Host Machine ]
                   |
          -----------------------
          |                     |
    Host-Only Network     NAT (updates)
          |
     -----------------
     |               |
[ soc-core ]   [ redforge-02 ]


---

## 4. Monitoring Flow

Traffic Flow (Isolated Test):

redforge-02 → Host-Only → soc-core → Snort → Logs → Correlation

Traffic Flow (LAN Simulation):

redforge-01 → LAN → soc-core → Snort → Logs → Correlation

---

## 5. Detection Philosophy

Iron Watch 01 is not focused on exploitation.

It is focused on:

- Visibility
- Signal validation
- Baseline building
- Structured analysis

Every test scenario must begin from a clean detection state.


---

# Threat Model Overview — Operation Iron Watch 01

## 1. Objective

The threat model for Iron Watch 01 is intentionally limited.

This campaign does not simulate advanced persistent threats.

It focuses on validating defensive visibility against common attacker behaviors.

---

## 2. Threat Actor Profiles

### Internal Opportunistic Attacker
Source: redforge-02 (Host-Only)
Capabilities:
- ICMP probing
- Port scanning (Nmap)
- SSH connection attempts

Goal:
Test detection reliability in a controlled environment.

---

### LAN-Based Attacker
Source: redforge-01 (Bridged)
Capabilities:
- Enumeration
- Service discovery
- SSH access attempts

Goal:
Compare detection signals under realistic LAN exposure.

---

## 3. Assets at Risk

Primary asset:
- soc-core (Detection Node)

Exposed Services (when enabled):
- SSH
- Open test ports (if configured for detection validation)

---

## 4. Assumptions

- No internet-based attacker in this phase
- No malware deployment
- No lateral movement simulation
- No privilege escalation scenarios

---

## 5. Defensive Objective

Iron Watch 01 validates:

- Traffic visibility
- Snort alert triggering
- Log ingestion reliability
- Manual correlation accuracy

Only after validation is complete will more advanced scenarios be introduced.

