# ============================================================
# SOC-CORE CORRELATION CASE 01
# ICMP → TCP CONNECT SCAN → SSH LOGIN ATTEMPT
#
# Case ID: SOC-CORE-CORR-01
# Attacker: redforge-02 (192.168.56.5)
# Defender: soc-core (192.168.56.4)
# ============================================================


# ============================================================
# PHASE 1 – ICMP HOST DISCOVERY
# ============================================================

# Action performed on redforge-02:
# ping 192.168.56.4

# On soc-core – verify ICMP traffic
cd /var/log/snort
sudo tcpdump -nn -r snort.log.* icmp

# Expected:
# 192.168.56.5 > 192.168.56.4: ICMP echo request
# 192.168.56.4 > 192.168.56.5: ICMP echo reply

# Host authentication log check (should return nothing)
sudo grep 192.168.56.5 /var/log/auth.log


# -----------------------------
# Evidence – ICMP
# -----------------------------
# Screenshot:
# ../04-evidences/corr-01-icmp-syn-ssh/corr-icmp.png
#
# Markdown reference (outside this bash block):

# ![ICMP Evidence](../04-evidences/corr-01-icmp-syn-ssh/corr-icmp.png)


# Interpretation:
# ICMP confirms host discovery.
# Visible at network layer only.
# No application-layer logging triggered.


# ============================================================
# PHASE 2 – TCP CONNECT SCAN (NMAP FALLBACK)
# ============================================================

# Action performed on redforge-02:
# nmap -sS 192.168.56.4
#
# Executed without root privileges.
# Nmap automatically fell back to TCP connect scan (-sT).

# On soc-core – inspect port 22 traffic
sudo tcpdump -nn -r snort.log.* port 22

# Observed TCP sequence:
# SYN → SYN/ACK → ACK

# Example:
# 192.168.56.5:50706 > 192.168.56.4:22  Flags [S]
# 192.168.56.4:22 > 192.168.56.5:50706  Flags [S.]
# 192.168.56.5:50706 > 192.168.56.4:22  Flags [.]


# Optional verification (RST check)
sudo tcpdump -nn -r snort.log.* 'tcp[tcpflags] & tcp-rst != 0' | grep 192.168.56.5

# No RST observed → confirms TCP connect scan.


# -----------------------------
# Evidence – SYN / TCP Connect
# -----------------------------
# Screenshot:
# ../04-evidences/corr-01-icmp-syn-ssh/corr-syn.png
#
# Markdown reference (outside this bash block):

# ![TCP Connect Scan Evidence](../04-evidences/corr-01-icmp-syn-ssh/corr-syn.png)


# Interpretation:
# Full TCP handshake confirms TCP connect scan.
# Port 22 reachable.
# No authentication yet.


# ============================================================
# PHASE 3 – SSH AUTHENTICATION ATTEMPT
# ============================================================

# Action performed on redforge-02:
# ssh socadmin@192.168.56.4

# On soc-core – inspect SSH traffic
sudo tcpdump -nn -r snort.log.* port 22

# Full handshake observed:
# SYN → SYN/ACK → ACK

# Host authentication logs
sudo grep 192.168.56.5 /var/log/auth.log

# Observed:
# pam_unix(sshd:auth): authentication failure
# Failed password for socadmin from 192.168.56.5


# -----------------------------
# Evidence – SSH Authentication Failure
# -----------------------------
# Screenshot:
# ../04-evidences/corr-01-icmp-syn-ssh/corr-ssh-failure.png
#
# Markdown reference (outside this bash block):

# ![SSH Failure Evidence](../04-evidences/corr-01-icmp-syn-ssh/corr-ssh-failure.png)


# Interpretation:
# SSH daemon engaged.
# Authentication failure logged.
# Application-layer interaction confirmed.


# ============================================================
# MULTI-LAYER CORRELATION SUMMARY
# ============================================================

# ICMP:
#   Network visibility: YES
#   Host visibility: NO
#   Classification: Host Discovery

# TCP Connect Scan:
#   Network visibility: YES
#   Host visibility: NO
#   Classification: Port Enumeration

# SSH Login Attempt:
#   Network visibility: YES
#   Host visibility: YES
#   Classification: Access Attempt

# Attacker workflow confirmed:
# Discovery → Enumeration → Access Attempt
# ============================================================

## Scan Type Observation

During analysis, no RST packets were observed after SYN/ACK responses.

Instead, the following pattern was recorded:

SYN → SYN/ACK → ACK

This indicates a full TCP connect scan (-sT), not a stealth SYN scan (-sS).

Reason:
The nmap command was executed without root privileges.
When -sS is executed without root, Nmap automatically falls back to TCP connect scan.

This resulted in full TCP handshakes and application-layer logging.



