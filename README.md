# Operation Iron Watch 01
Tactical Blue-Team Campaign (Hybrid Model)

## Mission
Operation Iron Watch 01 establishes the foundational defensive monitoring framework of Sentinel SOC Lab.

Goal:
- Validate network visibility
- Validate Snort-based detection
- Validate rsyslog ingestion reliability
- Build manual correlation discipline (pre-SIEM)

## Scope (Assets)
- soc-core (Ubuntu Server 22.04, detection node)
- redforge-02 (isolated internal attacker: Host-Only + NAT)
- redforge-01 (external exposure attacker: Bridged LAN)

## Detection Stack
- Snort IDS (primary detection)
- rsyslog ingestion
- Manual correlation (Snort alerts + auth.log + ingested copies)

## Repository Map
- `01-architecture/` — topology & network modes
- `02-assets/` — VM settings (VirtualBox) and baseline notes
- `03-soc-core/` — SOC node configs, detection, correlation
- `04-redforge-01/` — bridged attacker notes & commands
- `05-redforge-02/` — isolated attacker notes & commands
- `06-campaign-scenarios/` — scenario list + execution templates
- `07-evidence/` — logs + screenshots
- `08-lessons/` — lessons learned per phase

