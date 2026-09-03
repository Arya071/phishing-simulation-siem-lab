# Cloud Wazuh Deployment

## Purpose

This document records the cloud deployment work completed for the Wazuh monitoring environment. The goal was to move centralized security monitoring off the local lab infrastructure and host the Wazuh server on an OVHcloud VPS while keeping the monitored Ubuntu and Windows endpoints local.

## Final Architecture

```text
Ubuntu Endpoint ─────┐
                     │
                     ▼
              Internet / TCP
                     │
Windows Endpoint ────┤
                     │
                     ▼
          OVHcloud VPS (Wazuh)
     ┌──────────────────────────┐
     │ Wazuh Manager            │
     │ Wazuh Indexer            │
     │ Wazuh Dashboard          │
     └──────────────────────────┘
```

The final lab separates the centralized monitoring plane from the endpoint lab machines:

- **OVHcloud VPS:** Wazuh Manager, Indexer, and Dashboard.
- **Ubuntu endpoint:** Local Wazuh agent.
- **Windows endpoint:** Local Wazuh agent.

## Deployment Validation

The three primary Wazuh services were verified as running on the VPS:

- `wazuh-manager.service`
- `wazuh-indexer.service`
- `wazuh-dashboard.service`

The VPS was also configured to allow the Wazuh communication ports required by the lab:

- TCP 443 — Dashboard access
- TCP 1514 — Agent communication
- TCP 1515 — Agent enrollment
- SSH — Administrative access

## Resource Observation

During validation, the VPS showed approximately 3.7 GiB of memory with Wazuh Manager, Indexer, and Dashboard consuming most of the available resources. A 2 GiB swap file was added and verified to provide additional memory headroom.

This is a lab-scale deployment rather than a production sizing recommendation.

## Endpoint Migration

The Ubuntu agent was initially configured to communicate with a private/local manager address:

`192.168.92.137`

After the Wazuh server was hosted on OVHcloud, the agent configuration was updated to use the VPS public address and the agent was restarted.

The connection was subsequently confirmed by the agent log:

`Connected to the server (...:1514/tcp).`

## Agent Inventory

The Wazuh manager later reported the following active agents:

| ID | Name | Role | Status |
|---|---|---|---|
| 000 | Wazuh server | Server | Active/Local |
| 001 | ubuntu-bgnr | Ubuntu endpoint | Active |
| 002 | windows-bgnr | Windows endpoint | Active |

## Security Notes

The public IP address used during the lab is intentionally not repeated throughout this repository. Public-facing infrastructure should be reviewed before publication for unnecessary exposure, credentials, tokens, and environment-specific identifiers.

## Interview Takeaway

This deployment demonstrates:

- Cloud-hosted security infrastructure
- Centralized endpoint monitoring
- Network and firewall troubleshooting
- Agent-to-manager connectivity validation
- Linux and Windows endpoint integration
- Basic resource and service monitoring

