# Wazuh Detection Rules

## SIM-001 Rule Chain

SIM-001 uses a two-stage rule chain: a low-severity baseline rule identifies decoded GoPhish telemetry, then a higher-severity child rule identifies the tracked-link request associated with the controlled simulation.

## Rule 100010 — GoPhish Event Logged

```xml
<rule id="100010" level="3">
  <decoded_as>gophish</decoded_as>
  <description>Gophish Event Logged</description>
</rule>
```

| Property | Value |
|---|---|
| ID | `100010` |
| Level | `3` |
| Required decoder | `gophish` |
| Purpose | Baseline GoPhish telemetry classification |

Rule `100010` confirms that the event was classified as GoPhish telemetry. It should not be interpreted as proof that a recipient clicked a phishing link.

## Rule 100012 — Simulated Link Click

```xml
<rule id="100012" level="8">
  <if_sid>100010</if_sid>
  <match>GET /?rid=</match>
  <description>Phishing Simulation: Target clicked the malicious NexaCore link!</description>
  <mitre>
    <id>T1566.002</id>
  </mitre>
</rule>
```

| Property | Value |
|---|---|
| ID | `100012` |
| Level | `8` |
| Parent rule | `100010` |
| Match | `GET /?rid=` |
| Groups observed in alert | `gophish`, `phishing` |
| MITRE ATT&CK | `T1566.002` |
| Technique | Spearphishing Link |
| Tactic observed in alert | Initial Access |

## Rule Chaining

```text
GoPhish event
    |
    v
100010 / Level 3
    |
    | if_sid=100010
    v
match GET /?rid=
    |
    v
100012 / Level 8
    |
    v
T1566.002
```

The child rule's dependency on `100010` provides context: the tracked-link pattern is evaluated after the event has already been classified as GoPhish telemetry.

## Detection Semantics

Rule `100012` indicates that the lab observed a GoPhish event matching the configured tracked-link pattern. It does not prove credential submission, malware execution, persistence, or endpoint compromise.

## Tuning Considerations

Before adapting this logic outside the lab, test repeated requests, browser refreshes, automated URL scanners, malformed events, and legitimate GoPhish administrative testing. The match expression is specific to this simulation and is not intended to function as a universal phishing detector.
