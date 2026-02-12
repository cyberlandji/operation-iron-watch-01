# soc-core — VM Settings (VirtualBox)

## Role
Detection Node (Snort + rsyslog ingestion + manual correlation)

## OS
Ubuntu Server 22.04 LTS (no GUI)

## Resources
- RAM: 4 GB
- CPU: 3 vCPU
- Disk: 100 GB (VDI)

## Networking (Hybrid Lab)
Adapter 1: Host-Only
- Purpose: isolated lab traffic visibility with redforge-02
- Expected subnet example: 192.168.56.0/24 (adjust to your host-only network)

Adapter 2: NAT (optional)
- Purpose: updates/apt access without exposing services to LAN

## Notes
- Keep Snort environment clean: clear alert/log artifacts before each scenario
- Log ingestion target directory example: /opt/soc/raw/
- SSH hardening was problematic and is documented separately in soc-core docs
