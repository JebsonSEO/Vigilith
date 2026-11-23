# Vigilith System Architecture

## 1. Document Information

| Attribute | Description |
|---|---|
| Project | Vigilith |
| Document | System Architecture |
| Version | 1.0 |
| Status | Initial Architecture Specification |
| Architecture Type | Security Monitoring and Incident Response |
| Primary SIEM | Wazuh |
| Deployment Model | Hybrid Laboratory Environment |

## 2. Architecture Overview

Vigilith is an integrated security monitoring, threat detection and incident response platform designed to provide security visibility across endpoint, network and cloud environments.

The architecture combines endpoint telemetry collection, centralised security monitoring, detection engineering, threat intelligence, investigation capabilities and controlled incident response.

The platform will be developed incrementally, beginning with the existing Wazuh laboratory infrastructure.

## 3. Architecture Objectives

The architecture is designed to:

- Establish centralised security visibility.
- Collect and analyse endpoint security telemetry.
- Identify suspicious activities through validated detection rules.
- Monitor network behaviour and investigate suspicious traffic.
- Enrich security events using threat intelligence.
- Support evidence-based incident investigations.
- Extend monitoring capabilities to AWS environments.
- Facilitate controlled incident containment and response.
- Maintain the integrity and availability of security evidence.

## 4. Current Infrastructure

### 4.1 Primary Engineering Workstation

The primary engineering environment is a Dell XPS 15 9570 running Ubuntu Linux.

Responsibilities:

- Project development and administration.
- Git and GitHub version control.
- Wazuh endpoint monitoring.
- Security investigation and analysis.
- Management of the VirtualBox laboratory.

### 4.2 Wazuh Infrastructure

The existing Wazuh OVA hosts the central monitoring components.

| Component | Responsibility |
|---|---|
| Wazuh Manager | Receives agent telemetry, analyses events and evaluates detection rules. |
| Wazuh Indexer | Stores and indexes security events. |
| Wazuh Dashboard | Provides security monitoring, visualisation and investigation interfaces. |

The existing laboratory uses Wazuh version 4.14.8.

### 4.3 Ubuntu Endpoint

The Ubuntu workstation runs Wazuh Agent 4.14.8.

Its responsibilities include:

- Collecting configured system telemetry.
- Monitoring relevant security events.
- Forwarding events to the Wazuh Manager.
- Supporting endpoint detection and investigation.

The agent communicates with the Wazuh Manager through TCP port 1514.

### 4.4 Kali Linux

Kali Linux runs as an existing VirtualBox virtual machine.

Its intended role is controlled security testing, including authorised simulations of suspicious activities.

Kali is not currently designated as a monitored production endpoint.

## 5. Current Logical Architecture

```text
                 VIGILITH LABORATORY
                 ===================

                  UBUNTU WORKSTATION
                 Dell XPS 15 9570
                         |
             +-----------+-----------+
             |                       |
        Wazuh Agent              Kali Linux
             |                       |
             |                  Controlled
             |                  Security Tests
             |
             | TCP 1514
             |
             v
       +----------------------+
       |     WAZUH OVA        |
       |                      |
       |   Wazuh Manager      |
       |         |            |
       |         v            |
       |   Wazuh Indexer      |
       |         |            |
       |         v            |
       |   Wazuh Dashboard    |
       +----------------------+
                  |
                  v
           Security Analysis
           and Investigation

## 6. Target Architecture

The intended architecture will extend the existing environment through additional monitoring and response capabilities.

### 6.1 Endpoint Monitoring

Planned capabilities:

- Linux security telemetry.
- Windows Event Log collection.
- Sysmon process and network telemetry.
- File Integrity Monitoring.
- Authentication monitoring.
- Privilege escalation detection.

### 6.2 Network Security Monitoring

Planned capabilities:

- Network traffic inspection.
- Suspicious connection identification.
- Network reconnaissance detection.
- DNS and outbound connection analysis.
- Packet-level investigation.

Proposed technologies include Suricata and Wireshark.

### 6.3 Threat Intelligence

Planned integrations:

- VirusTotal.
- AbuseIPDB.
- MISP.

These integrations will support IOC enrichment, reputation checks and investigation context.

External intelligence will be evaluated for source reliability, confidence and recency before being used in incident decisions.

### 6.4 Cloud Security Monitoring

The planned AWS monitoring architecture will use selected services:

| Service | Intended responsibility |
|---|---|
| AWS CloudTrail | API activity and account auditing |
| Amazon GuardDuty | Threat detection |
| AWS Security Hub | Security findings and posture visibility |
| Amazon CloudWatch | Logs, metrics and monitoring |
| Amazon EventBridge | Event routing and response workflows |

AWS services will be enabled only when required. Applicable costs and permissions will be reviewed before deployment.

### 6.5 Incident Management and Automation

Future integrations may include:

- TheHive for incident management.
- Cortex for investigation enrichment.
- Shuffle for workflow automation.

Automated response actions will require appropriate safeguards, authorisation and testing.

## 7. Security Telemetry Flow

The primary endpoint telemetry flow is:

1. Ubuntu generates system and security events.
2. Configured Wazuh Agent collection modules collect relevant events.
3. The agent forwards telemetry to the Wazuh Manager.
4. The Manager processes events through decoders and detection rules.
5. Processed events are indexed by the Wazuh Indexer.
6. Analysts access relevant information through the Wazuh Dashboard.
7. Suspicious findings are investigated and documented.

Additional telemetry sources will be integrated progressively.

## 8. Network and Trust Boundaries

The architecture contains several trust boundaries.

| ID | Boundary | Security consideration |
|---|---|---|
| TB-01 | Ubuntu host and VirtualBox guests | Host and guest isolation |
| TB-02 | Ubuntu Agent and Wazuh Manager | Agent authentication and protected communication |
| TB-03 | Kali and monitoring infrastructure | Controlled testing and restricted access |
| TB-04 | Laboratory and external networks | Network exposure and outbound traffic |
| TB-05 | Local infrastructure and AWS | Cloud identity and API security |
| TB-06 | Monitoring and response integrations | Authorised access and controlled actions |

### 8.1 Agent Communication

The Wazuh Agent communicates with the Wazuh Manager over TCP port 1514.

Agent enrolment and registration are separate processes and may use additional services and ports depending on configuration.

### 8.2 Administrative Access

Administrative access to monitoring infrastructure must be restricted.

Planned controls include:

- Least-privilege access.
- Strong authentication.
- Restricted management interfaces.
- Controlled administrative accounts.
- Monitoring of privileged activities.

## 9. Security Architecture Principles

Vigilith will follow these principles:

### Least Privilege

Users, services and monitoring components should receive only the permissions necessary for their responsibilities.

### Defence in Depth

Security must be implemented across endpoint, network, identity, monitoring and cloud layers.

### Segmentation

Monitoring infrastructure should be separated from systems used for security testing wherever practical.

### Evidence Integrity

Security evidence should be centrally collected, access-controlled and protected against unauthorised modification.

### Continuous Monitoring

Monitoring health, telemetry availability and detection effectiveness should be evaluated continuously.

### Controlled Response

Incident response actions must consider operational impact, evidence preservation and authorisation requirements.


## 10. Availability and Failure Considerations

Vigilith must account for failures that could affect monitoring visibility.

| Failure scenario | Potential impact | Planned consideration |
|---|---|---|
| Wazuh Agent stops | Endpoint telemetry interruption | Agent health monitoring |
| Wazuh Manager unavailable | Central event processing interruption | Service health monitoring and recovery |
| Indexer unavailable | Event indexing and search disruption | Storage and service monitoring |
| Dashboard unavailable | Reduced analyst visibility | Dashboard health checks |
| Network interruption | Telemetry delivery failure | Connectivity monitoring |
| Endpoint compromise | Potential local log and agent manipulation | Independent monitoring and centralised evidence |
| Excessive event volume | Resource exhaustion | Capacity monitoring and detection tuning |

Monitoring interruptions must not automatically be classified as confirmed security incidents. Investigation should establish their cause.

## 11. Architecture Development Roadmap

| Phase | Development focus | Status |
|---|---|---|
| Phase 1 | Project foundation and architecture | In progress |
| Phase 2 | Endpoint monitoring and telemetry | Planned |
| Phase 3 | Detection engineering and validation | Planned |
| Phase 4 | Network security monitoring | Planned |
| Phase 5 | Threat intelligence integration | Planned |
| Phase 6 | Incident investigation and forensics | Planned |
| Phase 7 | AWS cloud threat detection | Planned |
| Phase 8 | Incident response and automation | Planned |
| Phase 9 | Security testing and final demonstration | Planned |

## 12. Architecture Validation Requirements

Before a component is considered operational, the following must be established:

- Its purpose and security requirements.
- Its communication paths and dependencies.
- Its access permissions.
- Its expected telemetry or output.
- Its failure conditions.
- Its testing and validation procedures.
- Its security limitations.

Architecture decisions must be updated when implementation reveals significant differences from the original design.

## 13. Document Status

This document establishes the initial logical and target architecture for Vigilith.

It is an evolving engineering specification. Components described as planned are not considered implemented or operational until independently deployed and validated.

