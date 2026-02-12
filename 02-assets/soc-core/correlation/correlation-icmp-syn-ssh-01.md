
# ============================================================
# SOC-CORE CORRELATION CASE 01
# ICMP → TCP CONNECT SCAN → SSH LOGIN ATTEMPT
#
# Case ID: SOC-CORE-CORR-01
# Attacker: redforge-02 (192.168.56.5)
# Defender: soc-core (192.168.56.4)
# Detection Engine: Snort (console + PCAP logging)
# Log Sources:
#   - /var/log/snort/snort.log.*
#   - /var/log/auth.log
# ============================================================


# ============================================================
# PHASE 1 – ICMP HOST DISCOVERY
# ============================================================

# Action performed on redforge-02:
# ping 192.168.56.4

# On soc-core – verify ICMP traffic in PCAP logs
cd /var/log/snort
sudo tcpdump -nn -r snort.log.* icmp

# Expected observation:
# 192.168.56.5 > 192.168.56.4: ICMP echo request
# 192.168.56.4 > 192.168.56.5: ICMP echo reply

# Check host authentication logs (should be empty)
sudo grep 192.168.56.5 /var/log/auth.log

# Interpretation:
# ICMP confirms host discovery.
# Visible at network layer only.
# No application-layer interaction triggered.


# ============================================================
# PHASE 2 – TCP CONNECT SCAN (NMAP FALLBACK)
# ============================================================

# Action performed on redforge-02:
# nmap -sS 192.168.56.4
#
# Note:
# Command executed without root privileges.
# Because -sS requires root, Nmap automatically fell back to:
# TCP Connect Scan (-sT).

# On soc-core – inspect port 22 traffic
sudo tcpdump -nn -r snort.log.* port 22

# Observed TCP flag sequence:
# SYN → SYN/ACK → ACK

# Example pattern:
# 192.168.56.5:50706 > 192.168.56.4:22  Flags [S]
# 192.168.56.4:22 > 192.168.56.5:50706  Flags [S.]
# 192.168.56.5:50706 > 192.168.56.4:22  Flags [.]


# Optional verification: check for RST packets
sudo tcpdump -nn -r snort.log.* 'tcp[tcpflags] & tcp-rst != 0' | grep 192.168.56.5

# Result:
# No RST observed → confirms NOT a stealth SYN scan.

# Check host logs
sudo grep 192.168.56.5 /var/log/auth.log

# Expected:
# No authentication attempts during scan phase.

# Interpretation:
# Full TCP handshake confirms TCP connect scan behavior.
# Port 22 confirmed open.
# SSH daemon reachable but no credentials used yet.


# ============================================================
# PHASE 3 – SSH AUTHENTICATION ATTEMPT
# ============================================================

# Action performed on redforge-02:
# ssh socadmin@192.168.56.4

# On soc-core – inspect port 22 traffic again
sudo tcpdump -nn -r snort.log.* port 22

# Observed:
# SYN → SYN/ACK → ACK
# Followed by encrypted SSH session traffic.

# Check host authentication logs
sudo grep 192.168.56.5 /var/log/auth.log

# Observed:
# pam_unix(sshd:auth): authentication failure
# Failed password for socadmin from 192.168.56.5

# Interpretation:
# Full TCP handshake established.
# SSH daemon engaged.
# Authentication failure logged.
# Application-layer interaction confirmed.


# ============================================================
# MULTI-LAYER CORRELATION SUMMARY
# ============================================================

# ICMP:
#   Network visibility: YES
#   Host log visibility: NO
#   Classification: Host Discovery

# TCP Connect Scan:
#   Network visibility: YES
#   Host log visibility: NO
#   Classification: Port Enumeration

# SSH Login Attempt:
#   Network visibility: YES
#   Host log visibility: YES
#   Classification: Access Attempt


# ============================================================
# KEY TECHNICAL OBSERVATION
# ============================================================

# Because nmap -sS was executed without root:
#   - Stealth SYN scan was not performed.
#   - Nmap defaulted to TCP connect scan (-sT).
#   - Full TCP handshakes observed (ACK instead of RST).
#   - Increased host-level visibility.


# ============================================================
# DETECTION CAPABILITY VALIDATION
# ============================================================

# SOC-Core successfully demonstrated:
#   - Packet-level visibility (Snort PCAP logs)
#   - IDS detection output (console alerts)
#   - Host authentication logging (auth.log)
#   - Timeline alignment across layers

# Correlation confirms structured attacker workflow:
#   Discovery → Enumeration → Access Attempt
# ============================================================
