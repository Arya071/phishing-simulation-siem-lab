# Wazuh Cloud Deployment Troubleshooting

## Purpose

This document captures real troubleshooting encountered while migrating the endpoint monitoring workflow from a local/private manager address to a cloud-hosted Wazuh server.

## Issue 1: Private Network Address Was Unreachable

### Symptom

A local connectivity test to the original private address returned:

`Destination net unreachable`

### Interpretation

The endpoint could not route traffic to the previous private network address from the current network path. A private RFC1918 address is not directly reachable through the public Internet.

### Resolution

The monitoring architecture was changed to use the public address of the OVHcloud VPS for the centralized Wazuh server.

---

## Issue 2: Ubuntu Agent Continued Using the Previous Address

### Symptoms

The agent log showed repeated messages similar to:

- Unable to connect to enrollment service
- Unable to connect to any server
- Transport endpoint is not connected
- Requesting a key from the previous server address

### Root Cause

The Ubuntu Wazuh agent configuration still referenced the previous private manager address.

### Resolution

The manager address in `/var/ossec/etc/ossec.conf` was updated to the cloud-hosted server and the agent was restarted.

The agent later reported a successful connection to TCP 1514.

### Lesson

A service restart is not sufficient if the configuration still points to the wrong destination. Troubleshooting required checking both:

1. **Network reachability**
2. **Application configuration**

---

## Issue 3: Agent Service Was Inactive

### Symptom

After stopping/restarting the Wazuh processes, `wazuh-agent.service` was shown as inactive.

### Resolution

The agent was started again and its logs were reviewed to verify the new connection attempt.

### Lesson

Process-level status and service-manager status should both be checked. A configuration change can be correct while the service itself remains stopped.

---

## Issue 4: Firewall Exposure

### Observation

The VPS firewall initially allowed SSH only. The required Wazuh ports were added for the lab:

- 443/TCP
- 1514/TCP
- 1515/TCP

### Lesson

Cloud connectivity depends on both the application listening state and the network/firewall policy.

## Troubleshooting Method Used

The debugging sequence was:

```text
Check service status
        ↓
Check configured manager address
        ↓
Check network reachability
        ↓
Check firewall rules
        ↓
Restart agent
        ↓
Inspect agent logs
        ↓
Verify manager-side agent status
```

This sequence is reusable for similar agent-to-server connectivity issues.

