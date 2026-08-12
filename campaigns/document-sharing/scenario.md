# SIM-003 — Document Sharing

## Scenario ID

SIM-003

## Campaign Name

NexaCore Engineering & Collaboration

## Objective

Evaluate whether test participants interact with an unexpected
document-sharing notification.

## Scenario

A fictional NexaCore Engineering & Collaboration department
sends a notification stating that a project document has been
shared with the recipient.

## Sender

NexaCore Engineering & Collaboration

## Sender Address

collaboration@nexacore.example

## Subject

Project Document Shared With You

## Primary Psychological Technique

Curiosity and routine workplace behavior.

## Secondary Techniques

- Familiarity
- Expected workplace activity
- Information curiosity

## Primary CTA

Review Shared Document

## Security Indicators

1. Unexpected document notification
2. Link-based action
3. Familiar-looking workplace context
4. Sender identity should be verified
5. Recipient should determine whether the document was expected

## Expected User Behavior

A security-aware participant should:

1. Verify the sender.
2. Consider whether the document was expected.
3. Inspect the destination before following the link.
4. Confirm the document through an independent channel if necessary.
5. Report suspicious communication.

## Data Collected

Only non-sensitive behavioral events are recorded:

- Email delivery
- Email opening
- Link click
- Reporting behavior
- Event timestamps

## Credentials

No passwords, authentication tokens, MFA codes, or other
authentication information are collected.

## Landing Page

The landing page will present a simulated document-sharing
notification and subsequently disclose the security-awareness
exercise.

## Security Lesson

Unexpected document-sharing notifications can exploit normal
workplace behavior and curiosity. Users should verify unexpected
documents and links before interacting with them.

## MITRE ATT&CK Mapping

Primary technique:

T1566 — Phishing

The final report will document the applicable phishing
sub-technique.
