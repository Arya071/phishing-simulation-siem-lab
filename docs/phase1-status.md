# Phase 1 Implementation Status

## Completed

### Cloud Infrastructure
- [x] Wazuh Manager deployed on an OVHcloud VPS
- [x] Wazuh Indexer running
- [x] Wazuh Dashboard running
- [x] Required lab firewall ports configured
- [x] Basic resource usage observed
- [x] Swap configured for additional memory headroom

### Ubuntu Endpoint
- [x] Wazuh agent running
- [x] Agent configuration migrated from private/local manager addressing to cloud-hosted monitoring
- [x] Successful connection to the cloud Wazuh server validated
- [x] Agent shown as Active on the manager
- [x] Authentication and privileged activity observed
- [x] FIM, SCA, Syscollector, and log collection active

### Windows Endpoint
- [x] Wazuh agent integrated
- [x] Agent shown as Active on the manager
- [x] Windows telemetry observed centrally

### Custom Detection Engineering
- [x] New local Wazuh rule created with ID **100100**
- [x] Custom rule validated with the Wazuh rule-testing workflow
- [x] Alert evidence captured
- [x] Wazuh Manager operational status captured
- [x] Validation screenshots added to the repository

See: [Custom Wazuh SSH Detection Rule](custom-wazuh-ssh-rule.md)

## Pending Evidence-Based Work

- [ ] Controlled Windows activity validation
- [ ] Capture final Windows alert evidence
- [ ] Select investigation case(s)
- [ ] Populate analyst investigation
- [ ] Create final incident timeline
- [ ] Final repository review and merge

## Project Principle

The repository distinguishes between:

- **Completed and validated work**
- **Planned work**
- **Evidence pending**

This is intentional. The project should remain defensible during technical interviews.
