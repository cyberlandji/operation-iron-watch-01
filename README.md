# Operation Iron Watch 01
Tactical Blue-Team Campaign (Hybrid Model)

## Mission
Operation Iron Watch 01 establishes the foundational defensive monitoring framework of Sentinel SOC Lab.

Goal:
- Validate network visibility
- Validate Snort-based detection
- Build manual correlation discipline (pre-SIEM)

## Scope (Assets)
- soc-core (Ubuntu Server 22.04, detection node)
- redforge-02 (isolated internal attacker: Host-Only + NAT)

## Detection Stack
- Snort IDS (primary detection)
- Manual correlation (Snort live detection + auth.log)

## Repository Map
- `01-architecture/` — topology & network modes
- `02-assets/` — VM settings (VirtualBox) and baseline notes
- `03-campaign-scenario/` — SOC node configs, detection, correlation
- `04-evidence/` — logs + screenshots
- `05-lessons/` — lessons learned per phase


# ============================================================
# Operation Iron Watch 01
# MITRE ATT&CK Mapping (Post-Analysis)
# ============================================================
#
# Scope & Transparency
# --------------------
# - MITRE ATT&CK mapping was applied retrospectively
# - Only observed activity is mapped
# - No successful compromise occurred
# - No incident response actions were performed
#
# Lab Characteristics
# -------------------
# - Two virtual machines on the same physical host
# - Host-only network for attacker/defender interaction
# - NAT used only for system updates
# - Intentionally noise-free environment
#
# Objective
# ---------
# Establish a clear baseline understanding of early-stage
# attacker behavior (signal) before introducing noise,
# correlation, or response workflows in later operations.
#
# ============================================================
# Observed Activity Summary
# ============================================================
#
# The attacker activity observed during Operation Iron Watch 01
# was limited to:
#
# - Host availability probing (ICMP)
# - Network service discovery (TCP connect scan)
# - Failed authentication attempts (SSH)
#
# No privilege escalation, persistence, execution, or lateral
# movement was observed.
#
# ============================================================
# MITRE ATT&CK Mapping (Enterprise)
# ============================================================
#
# NOTE:
# Mapping reflects ATTEMPTED or OBSERVED behavior only.
# Success is NOT required for ATT&CK classification.
#
# ------------------------------------------------------------
# Observed Action: ICMP ping
# Tactic: Reconnaissance
# Technique: Active Scanning
# Status: Observed
# Analyst Note:
# Ping was used to confirm target host availability prior to
# further probing.
#
# ------------------------------------------------------------
# Observed Action: nmap TCP connect scan (-sT)
# Tactic: Reconnaissance
# Technique: Network Service Discovery
# Status: Observed
# Analyst Note:
# Default TCP connect scan indicates lack of sudo / raw socket
# privileges. No stealth scanning (-sS) was performed.
#
# ------------------------------------------------------------
# Observed Action: SSH login attempts
# Tactic: Initial Access
# Technique: Valid Accounts
# Status: Attempted - Failed
# Analyst Note:
# Authentication attempts failed. No credentials obtained and
# no access granted.
#
# ============================================================
# Techniques Explicitly NOT Observed
# ============================================================
#
# - Execution
# - Persistence
# - Privilege Escalation
# - Lateral Movement
# - Defense Evasion
# - Command and Control
#
# ============================================================
# Analyst Assessment
# ============================================================
#
# The observed activity is consistent with an opportunistic or
# low-sophistication attacker:
#
# - No stealth techniques
# - No privilege awareness (no sudo usage)
# - No adaptation after authentication failure
#
# The purpose of Operation Iron Watch 01 was not to simulate a
# production SOC environment, but to build foundational
# detection understanding in a clean and controlled setting.
#
# ============================================================
# Progression
# ============================================================
## Operation Iron Watch 01 establishes a clean detection baseline.
# Future operations (e.g. Iron Watch 02) intentionally introduce:
#
# - Background noise and routine benign activity
# - Mixed benign and malicious traffic
# - Increased correlation complexity
# - Deeper investigation and response workflows
#

# NOTE: This operation was conducted in a low-noise environment
# to clearly observe early-stage attacker behavior.
# ============================================================


# ============================================================
# Detection Opportunities
# Operation Iron Watch 01
# ============================================================
#
# NOTE:
# The environment was intentionally noise-free.
# Detection ideas are derived from observed behavior only.
#
# ============================================================
# Detection Idea 1: ICMP Host Discovery
# ============================================================
#
# Description:
# Detect ICMP echo requests used to confirm host availability.
#
# Signals:
# - ICMP echo-request packets
# - Single or repeated probes to internal hosts
#
# Analyst Context:
# - A single ping is weak on its own
# - Ping followed by scanning increases confidence
#
# SOC Value:
# Early indicator of reconnaissance activity, especially useful
# for detecting insider movement or pivot attempts.
#
# ============================================================
# Detection Idea 2: TCP Connect Port Scanning
# ============================================================
#
# Description:
# Detect TCP connect scans generated without raw socket access.
#
# Signals:
# - Multiple full TCP handshakes
# - Sequential or patterned destination ports
#
# Analyst Context:
# - Indicates low-privilege or low-skill attacker
# - Common in opportunistic attacks and misconfigured tools
#
# SOC Value:
# Identifies reconnaissance even when stealth techniques
# are not used.
#
# ============================================================
# Detection Idea 3: Failed SSH Authentication Attempts
# ============================================================
#
# Description:
# Detect repeated SSH authentication failures from a single source.
#
# Signals:
# - "Failed password" or authentication failure logs
# - No successful login events
#
# Analyst Context:
# - Represents attempted initial access
# - Often precedes either abandonment or escalation attempts
#
# SOC Value:
# Early detection of credential probing activity.
#
# ============================================================


# ============================================================
# Observed Attack Flow (Kill Chain View)
# Operation Iron Watch 01
# ============================================================
#
# This flow represents ONLY what was observed.
# No stages beyond failed initial access occurred.
#
# ------------------------------------------------------------
#
# [Attacker VM]
#      |
#      |  ICMP Echo Request
#      v
# [Target VM]
#      |
#      |  TCP Connect Scan (-sT)
#      |  Multiple destination ports
#      v
# [Service Enumeration]
#      |
#      |  SSH Authentication Attempts
#      |  (Invalid credentials)
#      v
# [Access Denied]
#
# ------------------------------------------------------------
#
# Analyst Notes:
# - Ping confirmed host availability
# - Nmap scan identified exposed services
# - SSH login attempts failed
# - Attack chain terminated naturally
#
# No execution, persistence, privilege escalation,
# or lateral movement was observed.
#
# ============================================================

