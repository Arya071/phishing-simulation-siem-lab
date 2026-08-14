# Wazuh Integration

This directory documents the defensive/SIEM portion of the phishing-simulation lab. It focuses on how controlled GoPhish activity becomes observable telemetry, how Wazuh parses it, and why a detection rule fires.

## SIM-001 Status

SIM-001 has been validated end to end:

```text
GoPhish event
   ↓
Windows log collection
   ↓
Custom Wazuh decoder
   ↓
Rule 100010 (Level 3)
   ↓
Rule 100012 (Level 8)
   ↓
MITRE T1566.002
   ↓
Analyst-visible alert
```

## Documentation

- [`architecture/`](architecture/) — component and data-flow architecture.
- [`decoders/`](decoders/) — GoPhish parsing logic.
- [`rules/`](rules/) — custom detection rules and rule chaining.
- [`alerts/sim-001/`](alerts/sim-001/) — alert evidence and interpretation.
- [`../analysis/sim-001/`](../analysis/sim-001/) — complete SIM-001 detection case study.

## Publishing Safety

Only sanitized configuration, documentation, and evidence should be published. Do not commit credentials, tokens, private keys, real passwords, or unnecessary environment-specific identifiers.
