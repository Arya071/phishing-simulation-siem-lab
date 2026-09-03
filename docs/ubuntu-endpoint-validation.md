# Ubuntu Endpoint Validation

## Objective

Validate that the Ubuntu endpoint can communicate with the cloud-hosted Wazuh server and generate security-relevant telemetry for centralized analysis.

## Confirmed Components

The Ubuntu Wazuh agent was verified with the following processes:

- `wazuh-agentd`
- `wazuh-logcollector`
- `wazuh-syscheckd`
- `wazuh-execd`
- `wazuh-modulesd`

The endpoint also showed active Wazuh functionality including:

- File Integrity Monitoring (FIM)
- Security Configuration Assessment (SCA)
- Syscollector
- Journal monitoring
- System command/log collection

The configuration also showed collection of:

- `/var/log/suricata/eve.json`
- `/var/ossec/logs/active-responses.log`
- `/var/log/dpkg.log`

## Connectivity Issue and Resolution

The endpoint initially attempted to communicate with the previous private manager address. The agent produced connection and enrollment failures because that local address was no longer the centralized Wazuh server.

The manager address was updated to the OVHcloud-hosted server and the agent was restarted.

A subsequent agent log confirmed successful connectivity:

`Connected to the server (...:1514/tcp).`

The Wazuh manager later listed the Ubuntu agent as **Active**.

## Controlled Event Validation

The Ubuntu endpoint was used to generate and observe authentication and privileged activity. Observed categories included:

- SSH authentication activity
- Failed login activity
- Attempts involving a non-existent user
- PAM authentication/session events
- Privileged `sudo` execution

These events were used to validate that endpoint activity was reaching centralized monitoring.

## What This Demonstrates

The Ubuntu portion of the lab demonstrates the complete monitoring path:

```text
Endpoint activity
      ↓
Ubuntu system/authentication telemetry
      ↓
Wazuh agent
      ↓
Cloud-hosted Wazuh Manager
      ↓
Rule processing and alert generation
      ↓
Analyst review
```

## Evidence Status

The exact alert screenshots and final analyst timeline will be added after the evidence set is consolidated. This document intentionally avoids claiming additional attack simulations that have not yet been validated.

