# Windows Endpoint Validation

## Objective

Integrate a Windows endpoint with the cloud-hosted Wazuh deployment and confirm that the endpoint is visible to centralized monitoring.

## Completed Integration

The Windows Wazuh agent was installed and configured to communicate with the OVHcloud-hosted Wazuh server.

The manager later reported:

| ID | Name | Status |
|---|---|---|
| 002 | windows-bgnr | Active |

Windows telemetry was also observed in the Wazuh environment, confirming that the endpoint integration was functioning.

## Current Scope

This document records **confirmed endpoint integration**.

The following items are deliberately not claimed as complete yet:

- Suspicious PowerShell execution detection
- Persistence detection
- Account creation detection
- Privilege escalation simulation
- Custom Windows detection rules

Those activities require controlled validation and evidence before they are added to the project.

## Next Validation Phase

The next phase will perform controlled Windows-side activity, verify the resulting telemetry, select meaningful alerts, and document the investigation.

The final evidence will follow this workflow:

```text
Controlled Windows activity
        ↓
Windows telemetry
        ↓
Wazuh agent
        ↓
Cloud Wazuh Manager
        ↓
Alert/rule processing
        ↓
Evidence capture
        ↓
Analyst investigation
        ↓
Incident timeline
```

## Interview Takeaway

Even before advanced simulations, this phase demonstrates:

- Windows endpoint integration
- Cross-platform monitoring
- Centralized cloud security visibility
- Agent health validation
- Separation of validated results from planned work

