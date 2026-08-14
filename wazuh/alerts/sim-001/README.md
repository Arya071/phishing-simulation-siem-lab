# SIM-001 — Wazuh Alert Evidence

## Alert Summary

SIM-001 produced an analyst-visible Wazuh alert after the controlled GoPhish tracked-link interaction.

| Field | Observed value |
|---|---|
| Rule ID | `100012` |
| Rule level | `8` |
| Fired times | `1` |
| Decoder | `gophish` |
| Groups | `gophish`, `phishing` |
| MITRE ID | `T1566.002` |
| MITRE tactic | Initial Access |
| MITRE technique | Spearphishing Link |
| Source log | `C:\GoPhish\gophish.log` |
| Timestamp | Aug 11, 2026 @ 15:49:40.549 |

**Description:** `Phishing Simulation: Target clicked the malicious NexaCore link!`

## Evidence Set

Recommended sanitized screenshots:

1. `01-events-overview.png` — GoPhish telemetry and phishing alert in the Events view.
2. `02-rule-100012.png` — custom Level-8 rule details and ATT&CK mapping.
3. `03-alert-document-details.png` — event fields including decoder, rule, groups, source log, and timestamp.
4. `04-rule-100012-dashboard.png` — filtered search/dashboard evidence.
5. `05-rule-100010.png` — baseline GoPhish rule.
6. `06-gophish-rules.png` — relevant custom rule definitions.

The existing GoPhish campaign PDF supplies simulation-side evidence.

## Sanitization Checklist

Before publishing screenshots, remove or obscure values that do not contribute to the technical analysis, especially account/API identifiers, unnecessary internal IP addresses, personal email addresses, tokens, credentials, and private host information.

## Analyst Interpretation

The alert confirms that the configured Wazuh pipeline observed a GoPhish event matching the SIM-001 tracked-link condition. Because the event occurred during an authorized simulation, it should be classified as expected lab activity after validation rather than as a genuine compromise.
