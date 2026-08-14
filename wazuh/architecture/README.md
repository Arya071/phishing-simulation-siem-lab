# GoPhish → Wazuh Integration Architecture

## Overview

The defensive phase of the lab converts GoPhish application activity into Wazuh security telemetry. The implementation separates collection, parsing, classification, detection, and investigation so each stage can be validated independently.

## Components

| Layer | Component | Responsibility |
|---|---|---|
| Simulation | GoPhish | Generates the authorized campaign and interaction telemetry |
| Email safety | Mailtrap | Provides a controlled email sandbox |
| Application log | `C:\GoPhish\gophish.log` | Records GoPhish activity observed by the lab |
| Endpoint collection | Wazuh Windows agent | Collects monitored endpoint telemetry |
| Processing | Wazuh manager | Applies decoders and rules |
| Parsing | `gophish`, `gophish-detail` | Identifies and extracts GoPhish log fields |
| Classification | Rule `100010` | Establishes baseline GoPhish telemetry |
| Detection | Rule `100012` | Detects the configured tracked-link request |
| ATT&CK mapping | `T1566.002` | Classifies the simulated spearphishing-link technique |
| Investigation | Wazuh dashboard/events | Provides alert context for analyst review |

## Data Flow

```text
GoPhish
   |
   +--> Mailtrap --> controlled test recipient
   |                    |
   |                    v
   |              simulated interaction
   |                    |
   v                    v
C:\GoPhish\gophish.log
   |
   v
Wazuh Windows agent
   |
   v
Wazuh manager (Docker)
   |
   +--> gophish decoder
   |       |
   |       v
   |   gophish-detail
   |       |
   v       v
Rule 100010 (Level 3)
   |
   v
Rule 100012 (Level 8)
   |
   +--> MITRE T1566.002
   |
   v
Wazuh alert / investigation
```

## Wazuh Deployment Context

The lab's Wazuh 4.14.7 stack was observed running as Docker containers for the manager, indexer, and dashboard. Configuration paths such as `/var/ossec/etc/rules/` and `/var/ossec/etc/decoders/` therefore exist inside the Wazuh manager container rather than on the Docker host.

## Trust Boundary

The project is restricted to controlled test infrastructure. Mailtrap and authorized test identities prevent the simulation from becoming a real-world phishing campaign, while the defensive pipeline is used to study telemetry and alert behavior rather than to target external users.
