# Network Modes — Operation Iron Watch 01

This document defines the role of each VirtualBox network mode used in the lab.

---

## 1. Host-Only Network

Purpose:
- Isolated lab environment
- No exposure to real LAN
- Controlled attack simulations

Used by:
- soc-core
- redforge-02

Characteristics:
- Private subnet (e.g., 192.168.56.0/24)
- No internet access unless combined with NAT
- Ideal for repeatable detection testing

Why it matters:
Host-Only ensures that detection validation is noise-free and controlled.

---

## 2. NAT (Network Address Translation)

Purpose:
- Internet access for updates and package installation

Used by:
- soc-core (optional)
- redforge-02

Characteristics:
- Outbound internet allowed
- No inbound exposure from LAN
- Does not expose services externally

Why it matters:
Keeps lab operational without introducing detection noise.

---

## 3. Bridged Adapter

Purpose:
- Simulate real LAN attacker
- Expose VM directly to network segment

Used by:
- redforge-01

Characteristics:
- Receives IP from LAN router (e.g., Gigacube)
- Visible to other LAN devices
- Must be used carefully and only against owned systems

Why it matters:
Allows comparison between:
- Controlled isolated attacks
- Realistic network exposure scenarios

---

## 4. Hybrid Model Summary

Operation Iron Watch 01 uses:

Host-Only → Detection validation  
NAT → Operational support  
Bridged → Realistic exposure simulation  

This hybrid model provides:

- Control
- Realism
- Scalability for future SIEM integration

---

## 5. Security Discipline Rule

When using Bridged mode:

- Only target systems you own
- Only execute controlled enumeration
- Avoid uncontrolled traffic generation
- Document all actions

Iron Watch is about discipline, not chaos.

