# Project Evidence Repository

## Purpose

This directory contains technical evidence collected during the
MediCare Health Australia cybersecurity project.

Evidence is collected to demonstrate:

- security-control implementation;
- control testing;
- security findings;
- vulnerabilities;
- remediation activities;
- detection events;
- incident-response activities;
- forensic investigation;
- backup and recovery testing;
- cloud-security testing; and
- control retesting.

## Evidence Principle

A screenshot alone does not prove that a cybersecurity control is
effective.

Where practical, evidence should demonstrate:

1. the original security state;
2. the test performed;
3. the observed result;
4. remediation performed;
5. the control retest; and
6. the final security state.

## Evidence Naming Convention

Evidence files use the following format:

EV-[NUMBER]_[AREA]_[DESCRIPTION]_[STATE].[extension]

Examples:

EV-001_IAM_LocalAdmins_Before.png

EV-002_IAM_LocalAdmins_After.png

EV-003_VULN_BaselineScan.pdf

EV-004_VULN_Rescan.pdf

EV-005_DET_FailedLoginAlert.png

EV-006_IR_AccountDisabled.json

EV-007_BKP_RestoreTest.png

EV-008_AWS_CloudTrailEvent.json

## Evidence Handling

Evidence should:

- contain no real patient information;
- contain no passwords, API keys or authentication tokens;
- contain no AWS secret keys;
- contain no private credentials;
- use synthetic information;
- be clearly named;
- be linked to the relevant test, risk or finding where possible;
- preserve original log or configuration exports where appropriate.

Sensitive secrets must never be committed to the public GitHub
repository.