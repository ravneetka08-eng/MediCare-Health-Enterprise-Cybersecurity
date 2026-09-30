# Enterprise Cybersecurity Laboratory Network Plan

## Document Control

| Field | Value |
|---|---|
| Environment | Authorised Student Cybersecurity Laboratory |
| Virtualisation Platform | Oracle VirtualBox |
| Version | 1.2 |
| Status | Progressive Implementation |
| Addressing | RFC1918 Private Laboratory Addressing |

---

# 1. Purpose

This document defines the logical network architecture for the
cybersecurity laboratory.

The network is designed to support:

- Active Directory;
- DNS;
- Windows endpoints;
- security monitoring;
- vulnerability management;
- controlled security testing;
- future segmentation exercises.

The design must not be interpreted as the network architecture of a
real healthcare organisation.

---

# 2. Enterprise Network Concept

The fictional enterprise architecture contains multiple security
zones.

Representative enterprise zones include:

- workforce/user networks;
- clinical systems;
- server infrastructure;
- security-management systems;
- medical / IoT systems;
- cloud environments;
- external services.

The student laboratory initially implements a simplified network.

Segmentation will be introduced progressively where required for
technical testing.

---

# 3. Initial Laboratory Network

Initial representative subnet:

10.20.10.0/24

Subnet mask:

255.255.255.0

CIDR:

/24

This provides addresses:

10.20.10.1 – 10.20.10.254

for usable laboratory hosts, subject to reserved addresses and the
final VirtualBox network configuration.

---

# 4. Initial Addressing Plan

DC01 is the first address from this plan to be implemented and
validated.

The remaining addresses are project reservations and must not be
described as implemented until their corresponding systems have been
configured and tested.

These addresses are project reservations.

They are not considered implemented until configured and validated.

---

# 5. VirtualBox Network Architecture

Oracle VirtualBox provides the current virtual networking capability.

DC01 is connected to an isolated VirtualBox Host-Only network.

## Current DC01 Network Configuration

| Attribute | Configuration |
|---|---|
| Virtual Network Type | Host-Only Adapter |
| Server | DC01 |
| IPv4 Address | 10.20.10.10 |
| Prefix Length | /24 |
| Subnet Mask | 255.255.255.0 |
| Address Assignment | Static |
| DHCP on DC01 | Disabled |
| Default Gateway | None |
| DNS Service | DC01 |
| Internet Access | Not currently provided |

Current architecture:

DC01
  |
  | 10.20.10.10/24
  |
VirtualBox Host-Only Network
  |
  +---- Future Windows Clients
  |
  +---- Future Security Systems

The Host-Only design provides an isolated environment for building
and testing the initial Active Directory infrastructure without
unnecessary exposure to external networks.

Internet connectivity may be introduced separately when required for
legitimate updates, package installation or other authorised project
activities.

---

# 6. DNS Architecture

After Active Directory DNS is implemented, domain-joined Windows
clients should use:

DC01

10.20.10.10

as their primary lab DNS service.

Conceptually:

WIN-REC01
     |
     v
   DC01 DNS
     |
     v
Active Directory Service Discovery


WIN-CLIN01
     |
     v
   DC01 DNS
     |
     v
Active Directory Service Discovery

Active Directory depends heavily on DNS.

DC01 now provides the laboratory Active Directory DNS service.

The deployment has been validated for:

- the `corp.medicarehealth.test` DNS namespace;
- Active Directory-integrated DNS zones;
- LDAP service-location records;
- Kerberos service-location records;
- forward name resolution;
- reverse lookup capability.

Future domain-joined clients will use DC01 at `10.20.10.10` as their
Active Directory DNS server.

Using inappropriate DNS configuration on domain clients may cause:

- domain-join failures;
- authentication problems;
- Group Policy problems;
- service-discovery failures.

DNS configuration will therefore be explicitly validated during the
Active Directory build.

---

# 7. Internet Connectivity

Some laboratory systems may require Internet access for:

- operating-system updates;
- security updates;
- software installation;
- Linux package installation;
- Wazuh installation;
- vulnerability-feed updates;
- authorised software downloads.

Internet access will be provided only where required by the current
lab activity.

Deliberately vulnerable systems must not be intentionally exposed
directly to the public Internet.


## Current State

The initial DC01 network is intentionally isolated and does not
currently have a normal upstream Internet route.

As a consequence, services requiring external connectivity, including
external NTP synchronisation, are not currently available through the
Host-Only network.

This is a documented laboratory constraint rather than a production
network design.

---

# 8. Future Network Segmentation

The enterprise architecture will eventually be represented through
additional logical security zones.

Reserved design ranges include:

| Zone | Reserved Subnet | Purpose |
|---|---|---|
| Initial Enterprise Lab | 10.20.10.0/24 | Initial systems |
| Server Zone | 10.20.20.0/24 | Representative servers |
| Security Management | 10.20.30.0/24 | Security tooling |
| Medical / IoT Simulation | 10.20.40.0/24 | Legacy/medical scenario |

These are currently:

DESIGN ONLY

They must not be described as implemented network segmentation until
the corresponding virtual networks, routing and enforcement controls
have actually been configured and tested.

---

# 9. Medical / IoT Segmentation Scenario

A later project phase may represent the legacy clinical technology
scenario defined under EX-001.

Conceptual architecture:

Enterprise Network
       |
       v
Security Enforcement
       |
       v
Medical / IoT Zone
       |
       v
Representative Legacy System

Desired security behaviour:

Authorised Required Traffic
          |
          v
        ALLOW

Unnecessary Enterprise Traffic
          |
          v
         DENY

Unnecessary Internet Traffic
          |
          v
         DENY

This will demonstrate compensating controls for technology that
cannot meet the standard security baseline.

---

# 10. Security Monitoring Communication

Representative telemetry path:

WIN-REC01 --------+
                  |
WIN-CLIN01 -------+
                  |
DC01 -------------+----> SEC-MON01
                         Wazuh

Security telemetry may include:

- authentication events;
- Windows security events;
- Sysmon telemetry;
- selected endpoint events.

---

# 11. Vulnerability-Management Communication

Representative scanning path:

VULN01
   |
   +------> DC01 where authorised
   |
   +------> WIN-REC01
   |
   +------> WIN-CLIN01
   |
   +------> other authorised lab targets

Scanning must remain within the defined authorised project scope.

---

# 12. AWS Relationship

AWS represents a separate cloud-security environment.

Conceptually:

LOCAL LAB
    |
    |
 Internet / Management Access
    |
    v
AWS EDUCATIONAL ENVIRONMENT
    |
    +---- IAM
    |
    +---- Security Groups
    |
    +---- CloudTrail
    |
    +---- Selected Project Resources

The project does not initially require a permanent site-to-site
connection between the local VirtualBox network and AWS.

Cloud controls can be implemented and tested independently unless a
later project requirement justifies integration.

---

# 13. Network Security Validation

Initial validation has been performed for the DC01 infrastructure.

Validation performed includes:

- static IPv4 configuration;
- DNS zone availability;
- forward DNS resolution;
- reverse DNS configuration;
- Active Directory service discovery;
- LDAP SRV record resolution;
- Kerberos SRV record resolution;
- Domain Controller discovery.

Future testing will additionally validate:

- Windows client domain communication;
- permitted and denied connectivity;
- endpoint-to-Wazuh communication;
- scanner-to-target communication;
- segmentation;
- firewall behaviour.
---

# 14. Current Implementation Status

| Component | Status |
|---|---|
| 10.20.10.0/24 Addressing Plan | Implemented / Progressive |
| VirtualBox Host-Only Network | Implemented |
| DC01 Static Address | Implemented — 10.20.10.10/24 |
| DC01 Default Gateway | None — isolated lab |
| Active Directory DNS | Implemented |
| Forward DNS Resolution | Validated |
| Reverse DNS | Implemented / Validated |
| LDAP / Kerberos Service Discovery | Validated |
| Windows Clients | Pending |
| Wazuh Communication | Pending |
| Vulnerability Scanner Communication | Pending |
| Security Segmentation | Design Only |
| Internet Connectivity | Not currently provided to DC01 |
| External NTP | Pending appropriate upstream connectivity |
| AWS Local Integration | Not Required Initially |

The current network provides the minimum infrastructure required for
the initial Active Directory environment.

Additional connectivity and segmentation will be introduced only
when required by later project phases and will be documented after
implementation and validation.