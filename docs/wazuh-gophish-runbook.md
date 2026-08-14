# GoPhish → Wazuh Validation Runbook

This runbook documents the controlled validation workflow used by SIM-001. It contains no credentials, API keys, or production configuration.

## 1. Confirm the Wazuh Docker stack

From the Docker host:

```bash
sudo docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}"
```

The lab stack should contain the Wazuh manager, dashboard, and indexer containers.

## 2. Inspect custom decoders and rules

Because Wazuh is containerized, `/var/ossec` is inside the manager container rather than directly on the Docker host.

```bash
sudo docker exec single-node-wazuh.manager-1 cat /var/ossec/etc/decoders/gophish_decoders.xml
sudo docker exec single-node-wazuh.manager-1 sed -n '1,40p' /var/ossec/etc/rules/gophish_rules.xml
```

Confirm the deployed configuration contains the `gophish` and `gophish-detail` decoders plus Rules `100010` and `100012`.

## 3. Validate configuration before restarting

Use the Wazuh configuration-validation utility supported by the deployed manager image before restarting after decoder/rule changes. For this lab image, the analysis daemon can be checked with:

```bash
sudo docker exec single-node-wazuh.manager-1 /var/ossec/bin/wazuh-analysisd -t
```

If the deployed image exposes a different supported validation syntax, use that image-specific command rather than changing configuration blindly.

## 4. Restart after a validated change

```bash
sudo docker restart single-node-wazuh.manager-1
sudo docker ps --format "table {{.Names}}\t{{.Image}}\t{{.Status}}"
```

## 5. Generate a controlled GoPhish event

Run SIM-001 only against the authorized test recipient. The simulated interaction should append telemetry to:

```text
C:\GoPhish\gophish.log
```

The lab does not collect passwords, MFA codes, session tokens, or other authentication secrets.

## 6. Verify endpoint collection

Confirm the Windows Wazuh agent is monitoring the GoPhish log and that the file changes after the controlled event. The agent should forward the resulting telemetry to the manager.

## 7. Verify decoding

The resulting Wazuh event should show:

```text
decoder.name = gophish
```

This confirms the event entered the custom GoPhish decoding path.

## 8. Verify rule chaining

Expected sequence:

```text
GoPhish log event
      ↓
gophish decoder
      ↓
Rule 100010 — Level 3
      ↓
if_sid = 100010
      ↓
match = GET /?rid=
      ↓
Rule 100012 — Level 8
      ↓
MITRE T1566.002
```

Rule `100010` provides baseline classification. Rule `100012` provides the simulation-specific higher-severity detection.

## 9. Investigate the alert

Filter the Wazuh Events view by:

```text
rule.id: 100012
```

Useful fields include `timestamp`, `agent.name`, `decoder.name`, `full_log`, `location`, `rule.id`, `rule.level`, `rule.groups`, and the MITRE fields.

The validated SIM-001 alert used Rule `100012`, Level 8, decoder `gophish`, and MITRE `T1566.002`.

## 10. Evidence capture

The final SIM-001 evidence set contains five sanitized screenshots:

1. `01-event-detection.png` — Wazuh Events view showing the SIM-001 alert.
2. `02-rule-details.png` — Rule `100012` configuration and ATT&CK mapping.
3. `03-alert-document-details.png` — underlying alert fields, decoder, log source, severity, and MITRE metadata.
4. `04-dashboard-alert.png` — Wazuh dashboard representation of the alert.
5. `05-events-filtered.png` — Events view filtered to `rule.id: 100012`.

The decoder and Rule `100010` are documented as configuration evidence in the repository rather than as separate screenshots.

Before committing screenshots, sanitize API identifiers, personal email addresses, unnecessary internal IP addresses, credentials, tokens, and private host information.

## 11. Troubleshooting sequence

If the alert does not appear, isolate the pipeline in this order:

```text
GoPhish event exists?
      ↓
Windows log updated?
      ↓
Wazuh agent collecting the file?
      ↓
Manager received the event?
      ↓
Decoder identified as gophish?
      ↓
Rule 100010 fired?
      ↓
Rule 100012 condition matched?
      ↓
Dashboard indexed/displayed the alert?
```

Do not change multiple layers at once. Isolating the failing stage makes decoder and rule debugging easier.

## 12. Security boundary

This runbook is for an authorized lab. Keep the phishing simulation confined to controlled recipients and sandbox infrastructure. Treat the detection logic as lab-specific until it has been tested against false positives and additional event variants.
