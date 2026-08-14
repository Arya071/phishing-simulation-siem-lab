# Phishing Simulation & SIEM Detection Lab

An authorized cybersecurity home lab combining controlled phishing simulation, email sandboxing, endpoint telemetry, and SIEM detection using GoPhish, Mailtrap, and Wazuh.

**GoPhish → Mailtrap → Controlled Test User → GoPhish Log → Wazuh Agent → Decoder → Detection Rules → Alert → Analysis**

## Current Result

SIM-001, a fictional NexaCore recruitment scenario, has been validated end to end. A controlled link interaction produced GoPhish telemetry that was collected from `C:\GoPhish\gophish.log`, decoded by Wazuh, classified by Rule `100010`, escalated by Rule `100012` to Level 8, and mapped to MITRE ATT&CK `T1566.002 — Spearphishing Link`.

> The single-user campaign metrics are technical validation results, not a statistical phishing-susceptibility measurement.

## Objectives

- Build realistic phishing-awareness simulations in a controlled environment.
- Validate safe email delivery and tracking through GoPhish and Mailtrap.
- Collect GoPhish telemetry with a Windows Wazuh agent.
- Parse application logs with custom Wazuh decoders.
- Develop and validate chained SIEM detection rules.
- Map detections to MITRE ATT&CK.
- Investigate alerts and document limitations and false-positive considerations.
- Present the workflow as a reproducible cybersecurity portfolio project.

## Architecture

```text
GoPhish
   |
   +--> Mailtrap --> Controlled Test User
   |                    |
   |                    v
   |              Simulated Interaction
   |                    |
   v                    v
C:\GoPhish\gophish.log
   |
   v
Windows Wazuh Agent
   |
   v
Wazuh Manager
   |
   v
gophish / gophish-detail decoders
   |
   v
Rule 100010 — Level 3
   |
   v
Rule 100012 — Level 8
   |
   v
MITRE T1566.002
   |
   v
Alert Investigation
```

## Technologies

- Kali Linux
- GoPhish
- Mailtrap
- Wazuh 4.14.7
- Docker
- Windows Wazuh Agent
- HTML/CSS
- Git/GitHub
- MITRE ATT&CK

## Repository Structure

```text
phishing-simulation-siem-lab/
├── analysis/
│   └── sim-001/              # End-to-end detection case study
├── campaigns/                # Scenario design
├── docs/                     # Scope, brand, plan, controlled users
├── landing-pages/            # Simulation landing pages
├── screenshots/              # Sanitized evidence
├── templates/                # Email/template assets
└── wazuh/
    ├── alerts/               # Alert evidence and interpretation
    ├── architecture/         # Integration/data flow
    ├── dashboards/           # Dashboard status and future views
    ├── decoders/             # GoPhish parsing logic
    └── rules/                # Custom detection logic
```

## Simulation Scenarios

| ID | Scenario | Status |
|---|---|---|
| SIM-001 | Internship / Recruitment | Validated with Wazuh |
| SIM-002 | IT Security Notification | Planned |
| SIM-003 | Document Sharing | Planned |

## SIM-001 — Recruitment Simulation

SIM-001 uses a fictional technology internship opportunity from **NexaCore Technologies**. The message and landing page were designed for an authorized lab and delivered only to controlled test infrastructure.

### GoPhish Validation

| Event | Result |
|---|---:|
| Email sent | 1 |
| Email opened | 1 |
| Link clicked | 1 |
| Data submitted | 0 |
| Email reported | 0 |

The operator performed the interaction to validate delivery, tracking, the landing page, telemetry generation, and SIEM detection. These values must not be represented as a real-world user susceptibility rate.

### Wazuh Detection

| Stage | Implementation |
|---|---|
| Source log | `C:\GoPhish\gophish.log` |
| Parent decoder | `gophish` |
| Child decoder | `gophish-detail` |
| Baseline rule | `100010`, Level 3 — Gophish Event Logged |
| Detection rule | `100012`, Level 8 |
| Detection condition | `GET /?rid=` after Rule `100010` |
| ATT&CK mapping | `T1566.002 — Spearphishing Link` |
| Tactic observed | Initial Access |

The detailed implementation, evidence interpretation, rule semantics, and limitations are documented in [`analysis/sim-001/README.md`](analysis/sim-001/README.md).

## Detection Engineering

The defensive portion deliberately separates parsing from detection. The `gophish` decoder identifies application telemetry, `gophish-detail` extracts fields, Rule `100010` establishes a baseline GoPhish event, and Rule `100012` uses the parent-rule relationship plus the tracked-link request pattern to generate the higher-severity simulation alert.

See:

- [`wazuh/architecture/`](wazuh/architecture/)
- [`wazuh/decoders/`](wazuh/decoders/)
- [`wazuh/rules/`](wazuh/rules/)
- [`wazuh/alerts/sim-001/`](wazuh/alerts/sim-001/)

## Security Controls & Scope

This repository documents an authorized security-awareness laboratory. Only controlled test accounts and sandboxed email delivery are used. The project does not collect real passwords, authentication tokens, MFA codes, or session credentials, and it does not target real organizations or unauthorized individuals.

Before screenshots or logs are made public, they should be reviewed for credentials, tokens, account/API identifiers, personal email addresses, unnecessary IP addresses, and private host information.

## Detection Limitations

Rule `100012` is specific to this GoPhish simulation workflow. A Level-8 alert means the configured telemetry matched the lab detection condition; it does not independently prove credential compromise, malware execution, persistence, endpoint compromise, or an external threat actor.

A production adaptation would require broader testing for repeated requests, browser refreshes, automated URL scanners, malformed events, legitimate administrative activity, and other potential false-positive conditions.

## Future Work

- Sanitize and add the final SIM-001 Wazuh evidence set.
- Validate SIM-002 and SIM-003.
- Extend scenario-specific detection logic.
- Add repeatable decoder/rule testing.
- Build multi-scenario Wazuh visualizations.
- Measure detection latency only where timestamps permit a defensible calculation.
- Document investigation playbooks and tuning decisions.

## Disclaimer

This project is an authorized cybersecurity laboratory created for education, security-awareness research, detection engineering, and portfolio development. All phishing activity is restricted to controlled test infrastructure and explicitly authorized test accounts.
