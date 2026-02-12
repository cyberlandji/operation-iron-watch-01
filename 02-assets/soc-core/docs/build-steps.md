# soc-core — Build Steps (High Level)

1) Create VM (Ubuntu Server 22.04, no GUI)
2) Update system: apt update/upgrade
3) Install Snort
4) Validate interface + basic Snort test
5) Configure rsyslog forwarding/copy to /opt/soc/raw/
6) Validate ingestion consistency (auth.log vs ingested copy)
7) Baseline reset procedure before each scenario
