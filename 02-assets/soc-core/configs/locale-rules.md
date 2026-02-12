# $Id: local.rules,v 1.11 2004/07/23 20:15:44 bmc Exp $
# ----------------
# LOCAL RULES
# ----------------
# This file intentionally does not come with signatures.  Put your local
# additions here.

alert icmp any any -> 192.168.56.0/24 any (msg:"SOC LAB - ICMP Ping (Echoc Request) to 192.168.56.0/24"; itype:8; sid:1000001; rev:1;)

alert tcp any any -> 192.168.56.0/24 22 (msg:"SOC LAB - SSH Connection Attempt (SYN) to port 22"; flags:S; sid:1000002; rev:1;)

alert tcp any any -> 192.168.56.0/24 any (msg:"SOC LAB - Possible Port Scan (SYN Rate)"; flags:S; threshold:type both, track by_src, count 10, seconds 5; sid:1000003; rev:1;)

