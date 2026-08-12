# Phishing Simulation & SIEM Detection Lab

An authorized cybersecurity home lab combining phishing simulation, email sandboxing, security telemetry, and SIEM-based detection using GoPhish, Mailtrap, and Wazuh.

The project demonstrates an end-to-end security workflow:

**GoPhish → Mailtrap → Controlled Test User → Simulation Event → Wazuh → Alert → Analysis**

---

## Objectives

- Build realistic phishing-awareness simulations in a controlled environment.
- Validate email delivery and campaign tracking using GoPhish.
- Use Mailtrap as a safe email sandbox.
- Measure controlled interactions such as email delivery, opens, and clicks.
- Forward phishing-simulation telemetry into Wazuh.
- Develop and validate security monitoring and detection logic.
- Analyze social-engineering techniques and defensive controls.
- Document the complete workflow as a cybersecurity portfolio project.

---

## Architecture

```text
                         SIMULATION LAYER
                                │
                                ▼
                         ┌─────────────┐
                         │   GoPhish   │
                         │             │
                         │  Campaign   │
                         │  Tracking   │
                         │  Landing    │
                         │    Page     │
                         └──────┬──────┘
                                │
                         Simulated Email
                                │
                                ▼
                         ┌─────────────┐
                         │  Mailtrap   │
                         │    Email    │
                         │   Sandbox   │
                         └──────┬──────┘
                                │
                        Controlled Recipient
                                │
                         Open / Click Event
                                │
                                ▼
                         GoPhish Telemetry
                                │
                                ▼
                         ┌─────────────┐
                         │    Wazuh    │
                         │     SIEM    │
                         │             │
                         │  Ingestion  │
                         │  Detection  │
                         │   Alerting  │
                         └──────┬──────┘
                                │
                                ▼
                         Security Analysis
```

---

## Technologies

- Kali Linux
- GoPhish
- Mailtrap
- Wazuh
- HTML/CSS
- Python
- Git/GitHub
- MITRE ATT&CK

---

## Project Structure

```text
phishing-simulation-lab/
│
├── analysis/
│
├── campaigns/
│   ├── recruitment/
│   │   └── scenario.md
│   │
│   ├── security-notification/
│   │   └── scenario.md
│   │
│   └── document-sharing/
│       └── scenario.md
│
├── docs/
│   ├── brand.md
│   ├── campaign-plan.md
│   ├── campaign-profile.md
│   ├── scope.md
│   └── test-users.md
│
├── landing-pages/
│   └── sim-001-recruitment/
│       └── index.html
│
├── screenshots/
│
├── templates/
│   ├── base/
│   │   ├── preview.html
│   │   └── style.css
│   │
│   └── sim-001-recruitment/
│       └── email.html
│
├── .gitignore
└── README.md
```

---

# Simulation Scenarios

| ID | Scenario | Status |
|---|---|---|
| SIM-001 | Internship / Recruitment | Validated |
| SIM-002 | IT Security Notification | Planned |
| SIM-003 | Document Sharing | Planned |

---

# SIM-001 — Recruitment Simulation

SIM-001 is a controlled phishing-awareness simulation based on a fictional technology internship opportunity from **NexaCore Technologies**.

The scenario was designed to reproduce common social-engineering characteristics found in recruitment-themed phishing messages while remaining entirely within the authorized lab environment.

### Social-Engineering Theme

The scenario uses:

- Career opportunity
- Curiosity
- Professional branding
- Internship relevance
- Call-to-action urgency

The simulated email contains a tracked **Review Opportunity** button that directs the controlled test recipient to a simulated recruitment landing page.

---

## SIM-001 Email

The email was developed as a modern enterprise-style HTML message.

It includes:

- NexaCore branding
- Technology/early-career theme
- Internship opportunity details
- Location and work-arrangement information
- Personalized recipient greeting
- GoPhish URL tracking
- Responsive HTML/CSS design
- Plain-text fallback

GoPhish variables used by the template include:

```text
{{.FirstName}}
```

for personalization and:

```text
{{.URL}}
```

for campaign URL tracking.

---

## SIM-001 Landing Page

The landing page mirrors the visual language of the simulated email and presents a fictional Software Engineering Internship opportunity.

After the simulated interaction, the page provides a:

**Security Awareness Simulation**

disclosure explaining the purpose of the exercise.

The landing page does **not** collect:

- Passwords
- Authentication tokens
- MFA codes
- Session credentials
- Other sensitive authentication information

Data capture is intentionally disabled.

---

# Technical Validation

SIM-001 was first executed as a controlled technical validation test using a single authorized test recipient.

The resulting GoPhish campaign metrics were:

| Event | Result |
|---|---:|
| Email Sent | 1 |
| Email Opened | 1 |
| Link Clicked | 1 |
| Data Submitted | 0 |
| Email Reported | 0 |

### Interpretation

These results demonstrate that the technical simulation workflow successfully operated end-to-end.

The results **must not be interpreted as a 100% phishing-susceptibility rate**, because the test interaction was performed by the lab operator.

The purpose of this run was to validate:

```text
Email Delivery
      ↓
Open Tracking
      ↓
URL Tracking
      ↓
Landing Page
      ↓
Security Awareness Disclosure
```

---

# Wazuh Integration

Following the successful GoPhish validation, SIM-001 telemetry was integrated with the Wazuh home lab.

The objective is to move beyond simply observing campaign statistics inside GoPhish and demonstrate how phishing-simulation activity can become observable security telemetry within a SIEM.

Current workflow:

```text
GoPhish Event
      │
      ▼
Telemetry
      │
      ▼
Wazuh Manager
      │
      ▼
Detection Logic
      │
      ▼
Wazuh Alert
      │
      ▼
Security Analysis
```

SIM-001 has successfully generated an event that was logged by the Wazuh manager.

Further documentation of the Wazuh integration, event structure, decoders, rules, and alerts will be maintained as the defensive phase of the project develops.

---

# Detection Engineering

The defensive portion of the project focuses on transforming phishing-simulation activity into useful security telemetry.

Areas of investigation include:

- Event ingestion
- Log normalization
- Wazuh decoders
- Custom detection rules
- Alert severity
- Event correlation
- Phishing interaction detection
- Investigation workflows
- Detection limitations
- Security-awareness recommendations

The goal is to demonstrate the complete path from simulated activity to SIEM detection.

---

# Security Analysis

Each phishing scenario is analyzed from both offensive and defensive perspectives.

### Offensive Perspective

The simulation examines social-engineering characteristics such as:

- Authority
- Urgency
- Curiosity
- Career motivation
- Familiar branding
- Call-to-action design

### Defensive Perspective

The project examines controls such as:

- Sender verification
- URL inspection
- Independent verification of opportunities
- Email security monitoring
- SIEM telemetry
- Detection rules
- Security-awareness training
- Incident investigation

---

# Metrics

The project tracks technical campaign metrics including:

- Emails sent
- Delivery status
- Email opens
- Link clicks
- Reported messages
- Landing-page interactions
- Wazuh alerts
- Detection severity
- Detection latency

For controlled single-user validation runs, these metrics are used to verify system functionality rather than to make statistical claims about human phishing susceptibility.

---

# Security Controls & Scope

This project is an authorized cybersecurity laboratory.

The following restrictions apply:

- Only explicitly authorized test accounts are used.
- Mailtrap is used as the email sandbox.
- No real passwords are collected.
- No authentication tokens are collected.
- No MFA codes are collected.
- No credential-harvesting forms are used.
- No real corporate infrastructure is targeted.
- Fictional organization names are used.
- Test identities are sanitized for public documentation.
- Campaign results are not presented as real-world susceptibility data.

---

# MITRE ATT&CK Mapping

The project can be mapped to relevant MITRE ATT&CK techniques associated with phishing and social engineering.

Potential mappings include:

| Technique | Description |
|---|---|
| T1566 | Phishing |
| T1566.002 | Phishing: Spearphishing Link |

Mappings are used for analytical and educational purposes and are applied to the simulated scenarios rather than real-world attacks.

---

# Lessons Learned

The project demonstrates the lifecycle of a controlled phishing simulation:

```text
Scenario Design
      ↓
Social-Engineering Analysis
      ↓
Email Template Development
      ↓
Controlled Email Delivery
      ↓
Campaign Tracking
      ↓
Landing-Page Interaction
      ↓
Telemetry Generation
      ↓
SIEM Ingestion
      ↓
Detection Engineering
      ↓
Security Analysis
```

The project also demonstrates an important distinction between **offensive simulation** and **defensive validation**.

A successful simulated click during a controlled technical test does not by itself demonstrate user susceptibility. Meaningful susceptibility studies require an appropriately designed participant study with authorization, methodology, and sufficient sample size.

---

# Future Work

Planned improvements include:

- Complete SIM-002 security-notification scenario.
- Complete SIM-003 document-sharing scenario.
- Expand Wazuh detection rules.
- Improve event correlation.
- Build phishing-simulation dashboards.
- Compare telemetry across multiple scenarios.
- Measure detection latency.
- Document false positives and detection limitations.
- Develop a repeatable detection-engineering methodology.
- Create a final incident-investigation workflow.
- Produce a consolidated security assessment report.

---

# Disclaimer

This project is an authorized cybersecurity laboratory created for education, security-awareness research, detection engineering, and portfolio development.

All phishing activity is restricted to controlled test infrastructure and explicitly authorized test accounts.

No real credentials are collected and no unauthorized systems or individuals are targeted.
