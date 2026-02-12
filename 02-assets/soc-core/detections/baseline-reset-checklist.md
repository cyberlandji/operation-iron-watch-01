# Baseline Reset Checklist
Operation Iron Watch 01

This checklist MUST be completed before every scenario execution.

Purpose:
Ensure clean detection environment and eliminate false correlation artifacts.

---

## 1. Stop Snort

sudo pkill snort

Verify:
ps aux | grep snort

No active Snort processes should remain.

---

## 2. Clear Snort Alert Logs

Default locations (adjust if needed):

sudo rm -f /var/log/snort/alert
sudo rm -f /var/log/snort/snort.log*

If using custom alert directory, clear accordingly.

---

## 3. Clear Previous Ingested Logs (Optional but Recommended)

Example:

sudo rm -f /opt/soc/raw/auth.log

OR archive instead:

sudo mv /opt/soc/raw/auth.log /opt/soc/raw/archive/auth-<date>.log

---

## 4. Record Baseline Timestamp

date

Document the exact timestamp before starting detection.

Example:
Baseline Start Time: YYYY-MM-DD HH:MM:SS

All alerts after this timestamp belong to the new scenario.

---

## 5. Verify Network Interfaces

ip a

Confirm correct monitoring interface.

---

## 6. Restart Snort in Clean Mode

sudo snort -i <interface> -c /etc/snort/snort.conf -A console

Verify:
- No immediate alerts
- No configuration errors
- No residual traffic

---

## 7. Confirm Log Ingestion Active

sudo systemctl status rsyslog

Ensure:
- Active (running)
- No errors

---

## 8. Final Pre-Execution Confirmation

Checklist:
[ ] Snort stopped and restarted
[ ] Old alerts removed
[ ] Ingested logs cleared or archived
[ ] Timestamp recorded
[ ] Interface verified
[ ] rsyslog active

Only after all boxes are validated:
Proceed with scenario execution.

---

Iron Watch Rule:
No scenario starts from a dirty environment.
