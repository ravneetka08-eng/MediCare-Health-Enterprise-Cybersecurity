# Laboratory System Inventory

## Document Control

| Field | Value |
|---|---|
| Environment | Authorised Student Cybersecurity Laboratory |
| Virtualisation | Oracle VirtualBox |
| Host RAM | 16 GB |
| Host OS | Windows 11 Home 25H2 |
| Version | 1.2 |
| Status | Progressive Implementation |

---

# 1. Purpose

This inventory defines the representative systems planned for the
cybersecurity laboratory.

Systems will be introduced progressively.

Resource allocations are laboratory starting points and are not
production sizing recommendations.

---

# 2. Host Computer

| Attribute | Value |
|---|---|
| Host Operating System | Windows 11 Home 25H2 |
| Processor | Intel Core Ultra 5 125H |
| Architecture | x64 |
| Physical RAM | 16 GB |
| Total Storage | Approximately 477 GB |
| Free Storage at Design Time | Approximately 265 GB |
| Hypervisor | Oracle VirtualBox |

---

# 3. Planned Core Systems

| Hostname | Role | OS / Platform | Lab RAM Target | Primary Purpose |
|---|---|---|---:|---|
| DC01 | Domain Controller / DNS Server | Windows Server 2025 Standard Evaluation | 2–3 GB | AD DS / DNS / GPO |
| WIN-REC01 | Reception Endpoint | Windows Client | 3 GB | Reception role simulation |
| WIN-CLIN01 | Clinical Endpoint | Windows Client | 3 GB | Clinical role simulation |
| SEC-MON01 | Security Monitoring | Linux | 4–6 GB | Wazuh |
| VULN01 | Vulnerability Management | Linux / selected platform | 3–4 GB | Vulnerability assessment |

Exact resource allocation may be adjusted after observing actual
system performance.

---

# 4. Resource Management Strategy

The host contains 16 GB physical memory.

The architecture therefore uses staged VM operation instead of
running every component simultaneously.

## IAM / Active Directory Scenario

Expected active systems:

- DC01;
- WIN-REC01;
- WIN-CLIN01 where required.

Expected inactive systems:

- SEC-MON01;
- VULN01.

---

## Detection Scenario

Expected active systems:

- DC01 where required;
- WIN-REC01 or WIN-CLIN01;
- SEC-MON01.

Expected inactive system:

- VULN01.

---

## Vulnerability Scenario

Expected active systems:

- VULN01;
- selected target system;
- DC01 only where required.

Other systems should remain powered off where unnecessary.

---

# 5. DC01

## Role

DC01 is the first Domain Controller and DNS server deployed for the
MediCare Health laboratory Active Directory environment.

## Current Configuration

| Attribute | Configuration |
|---|---|
| Hostname | DC01 |
| Operating System | Windows Server 2025 Standard Evaluation |
| IPv4 Address | 10.20.10.10/24 |
| Addressing | Static |
| AD Forest | corp.medicarehealth.test |
| AD Domain | corp.medicarehealth.test |
| NetBIOS Domain | MEDICARE |
| AD DS | Installed and operational |
| DNS | Installed and AD-integrated |
| Global Catalog | DC01 |
| FSMO Roles | All five roles currently hosted on DC01 |
| Network | Isolated VirtualBox Host-Only network |

## Implemented Services

- Active Directory Domain Services;
- AD-integrated DNS;
- forward DNS resolution;
- reverse DNS lookup zone;
- LDAP and Kerberos service discovery;
- SYSVOL;
- NETLOGON;
- Global Catalog.

Group Policy infrastructure is provided by Active Directory and will
be configured during the relevant project phase.

## Validation Performed

The DC01 deployment was validated using:

- `ipconfig /all`;
- `Get-ADDomain`;
- `Get-ADForest`;
- `Get-ADDomainController`;
- `Get-DnsServerZone`;
- `Resolve-DnsName`;
- `nltest`;
- `net share`;
- `netdom query fsmo`;
- `dcdiag`;
- `dcdiag /test:DNS /v`;
- `w32tm`.

Validation confirmed:

- static IPv4 configuration;
- Active Directory domain and forest availability;
- AD-integrated DNS zones;
- LDAP and Kerberos SRV records;
- Domain Controller discovery;
- SYSVOL and NETLOGON shares;
- FSMO role placement;
- core Domain Controller and DNS functionality.

## Security Functions

DC01 will support testing of:

- workforce identities;
- authentication;
- organisational units;
- security groups;
- RBAC;
- least privilege;
- privileged identities;
- account lifecycle;
- password/account policies;
- Group Policy;
- Windows auditing.

## Current Laboratory Constraint

DC01 currently operates on an isolated VirtualBox Host-Only network.

No normal upstream network path is currently configured. The
forest-root PDC Emulator therefore currently reports the local clock
as its time source.

External time synchronisation will be addressed when appropriate
network connectivity is intentionally introduced.

## Enterprise Limitation

The laboratory currently uses one Domain Controller.

A production enterprise architecture would normally consider:

- multiple Domain Controllers;
- DNS redundancy;
- fault tolerance;
- multiple sites where required;
- reliable upstream time synchronisation;
- backup and recovery;
- centralised monitoring;
- privileged administration controls.

The single-DC design is appropriate for the current laboratory stage
but is not presented as a production high-availability architecture.
---

# 6. WIN-REC01

## Role

Representative Reception / Patient Administration endpoint.

## Representative Identity

reception.user

## Business Function

Representative patient-administration activity using synthetic data.

## Security Purpose

Used to test:

- domain authentication;
- RBAC;
- least privilege;
- Group Policy;
- endpoint hardening;
- Microsoft Defender;
- authentication logging;
- Sysmon;
- Wazuh telemetry;
- controlled security scenarios.

---

# 7. WIN-CLIN01

## Role

Representative Clinical endpoint.

## Representative Identity

clinician.user

## Business Function

Representative access to synthetic clinical information.

## Security Purpose

Provides a separate role/security context from Reception.

It will support testing of:

- clinical role permissions;
- access boundaries;
- Group Policy;
- endpoint controls;
- security logging;
- monitoring.

---

# 8. SEC-MON01

## Role

Security monitoring server.

## Planned Platform

Linux.

## Planned Technology

Wazuh.

## Security Purpose

Receive security telemetry from representative laboratory systems.

Capabilities demonstrated may include:

- log collection;
- endpoint monitoring;
- detection;
- alerting;
- event investigation;
- SOC-style triage.

## Resource Note

SEC-MON01 is expected to be one of the most memory-intensive systems
in the local laboratory.

It should normally be powered on only when required for monitoring
and detection exercises.

---

# 9. VULN01

## Role

Authorised vulnerability-management system.

## Purpose

Assess selected project-owned systems for:

- known vulnerabilities;
- missing security updates;
- selected configuration weaknesses.

Workflow:

Authorised Scan
      |
      v
Finding
      |
      v
Analysis
      |
      v
Risk
      |
      v
Treatment
      |
      v
Remediation
      |
      v
Rescan
      |
      v
Validation

The scanner must only target authorised laboratory systems.

---

# 10. AWS Environment

AWS provides the project's cloud-security environment.

The user has access to an educational/student AWS environment for the
project period.

Potential services may include:

- IAM;
- EC2 where appropriate;
- Security Groups;
- S3 where appropriate;
- CloudTrail;
- selected logging/security services.

Services will be selected according to:

- educational-account availability;
- project requirements;
- security;
- cost.

AWS credentials and secrets must never be committed to GitHub.

---

# 11. Synthetic Data

No real patient information will be used.

Representative synthetic information may contain:

- synthetic patient ID;
- fictional patient name;
- fictional date of birth;
- fictional appointment information;
- fictional clinical classification;
- fictional contact information.

All data must be clearly artificial.

---

# 12. System Lifecycle Status

Each system may use the following lifecycle states:

- Planned
- VM Created
- Operating System Installed
- Base Configuration Complete
- Security Configuration In Progress
- Operational
- Testing
- Snapshot Available
- Retired

The project documentation will be updated as systems move through
these stages.

---

# 13. Initial Status

| System | Current Status |
|---|---|
| DC01 | Base Configuration Complete / Operational |
| WIN-REC01 | Planned |
| WIN-CLIN01 | Planned |
| SEC-MON01 | Planned |
| VULN01 | Planned |
| AWS Environment | Available / Configuration Pending |

A system must not be marked Operational until the relevant build and
validation steps have been completed.