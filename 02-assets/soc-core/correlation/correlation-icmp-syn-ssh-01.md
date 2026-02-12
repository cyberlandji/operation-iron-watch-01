# ============================================================
# SOC-CORE – CORRELATION PHASE
# Scenario: ICMP → SYN Scan (-sS) → SSH Login Attempt
# Attacker: 192.168.56.5 (redforge-02)
# Defender: 192.168.56.4 (soc-core)
# ============================================================


# -----------------------------
# 0. Verify Monitoring Interface
# -----------------------------
ip a | grep enp0s8


# -----------------------------
# 1. Review Network Evidence (PCAP)
# -----------------------------
cd /var/log/snort

# List Snort packet logs
ls -lh snort.log.*

# Read captured packets (no name resolution)
sudo tcpdump -nn -r snort.log.* host 192.168.56.5


# -----------------------------
# 2. Correlate ICMP (Host Discovery)
# -----------------------------
sudo tcpdump -nn -r snort.log.* icmp

# Expected:
# 192.168.56.5 > 192.168.56.4: ICMP echo request
# 192.168.56.4 > 192.168.56.5: ICMP echo reply

# Check host logs (should return nothing)
sudo grep 192.168.56.5 /var/log/auth.log


# -----------------------------
# 3. Correlate SYN Scan (-sS Enumeration)
# -----------------------------
sudo tcpdump -nn -r snort.log.* 'tcp[tcpflags] & tcp-syn != 0'

# Look for pattern:
# Flags [S]
# Flags [S.]
# Flags [R]
# Meaning:
# SYN → SYN/ACK → RST  (Stealth scan)

# Confirm no SSH daemon log triggered
sudo grep 192.168.56.5 /var/log/auth.log


# -----------------------------
# 4. Correlate SSH Login Attempt
# -----------------------------
sudo tcpdump -nn -r snort.log.* port 22

# Look for full handshake:
# Flags [S]
# Flags [S.]
# Flags [.]
# Meaning:
# SYN → SYN/ACK → ACK (Full TCP session)

# Confirm authentication failure logged
sudo grep 192.168.56.5 /var/log/auth.log


# -----------------------------
# 5. Timeline Alignment
# -----------------------------
date

# Compare timestamps between:
# - tcpdump output
# - /var/log/auth.log entries
# - Snort console alerts (if used)


# -----------------------------
# 6. Behavioral Interpretation
# -----------------------------
# ICMP       → Host discovery (network-only visibility)
# SYN Scan   → Enumeration (network-only visibility)
# SSH Login  → Access attempt (network + host visibility)

# This confirms layered detection capability:
# - Network evidence
# - Detection rule alerts
# - Host authentication logs
# ============================================================
