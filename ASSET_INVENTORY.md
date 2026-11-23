# Vigilith Asset Inventory

## 1. Document Information

| Attribute | Description |
|---|---|
| Project | Vigilith |
| Document | Asset Inventory |
| Version | 1.0 |
| Status | Initial Asset Register |
| Environment | Security Monitoring Laboratory |
| Classification | Internal Project Documentation |

## 2. Purpose

This document establishes the asset inventory for Vigilith.

It identifies the infrastructure, systems, services and security resources that support the platform.

The inventory provides a foundation for asset classification, risk assessment, threat modelling, security monitoring and incident response prioritisation.

Assets are classified according to their operational importance and potential security impact.

## 3. Asset Classification

Vigilith uses four asset criticality levels.

| Classification | Description |
|---|---|
| Critical | Compromise could severely affect monitoring infrastructure, security evidence or administrative control. |
| High | Compromise could significantly affect security operations or infrastructure. |
| Medium | Compromise could create a limited operational or security impact. |
| Low | Limited impact on the overall environment. |

## 4. Asset Register

### 4.1 Active Laboratory Assets

| Asset ID | Asset | Environment | Function | Criticality | Status |
|---|---|---|---|---|---|
| VIG-AST-001 | Dell XPS 15 9570 | Ubuntu Host | Primary engineering workstation and monitored endpoint | High | Active |
| VIG-AST-002 | Wazuh Manager | Wazuh OVA | Central security event processing and detection | Critical | Active |
| VIG-AST-003 | Wazuh Indexer | Wazuh OVA | Security event indexing and storage | Critical | Active |
| VIG-AST-004 | Wazuh Dashboard | Wazuh OVA | Security monitoring and investigation interface | High | Active |
| VIG-AST-005 | Kali Linux | VirtualBox VM | Controlled security testing environment | Medium | Active |
| VIG-AST-006 | VirtualBox | Ubuntu Host | Virtualisation and laboratory infrastructure | High | Active |
| VIG-AST-007 | Host-only Virtual Network | VirtualBox | Communication between host and laboratory VM | High | Active |
| VIG-AST-008 | Bridged Network Interface | Ubuntu Host | External network connectivity through the host network | High | Active |
| VIG-AST-009 | GitHub Vigilith Repository | GitHub | Project source control and documentation | High | Active |

### 4.2 Planned and Future Assets

| Asset ID | Asset | Environment | Intended Function | Criticality | Status |
|---|---|---|---|---|---|
| VIG-AST-010 | Windows Endpoint | Virtual or Physical | Windows security telemetry collection | High | Planned |
| VIG-AST-011 | Suricata Sensor | Network | Network threat detection and inspection | High | Planned |
| VIG-AST-012 | Threat Intelligence Sources | External | IOC enrichment and reputation analysis | Medium | Planned |
| VIG-AST-013 | AWS Cloud Environment | Cloud | Cloud infrastructure monitoring | Critical | Planned for integration |
| VIG-AST-014 | TheHive | Incident Management | Incident tracking and case management | High | Planned |
| VIG-AST-015 | Cortex | Security Integration | Investigation enrichment and analysis | Medium | Planned |
| VIG-AST-016 | Shuffle | Automation | Security workflow automation | High | Planned |

Planned assets are not considered deployed, connected or operational.

## 5. Asset Ownership and Responsibilities

Vigilith is an independently managed laboratory environment.

The project operator is responsible for:

- Asset administration.
- Configuration management.
- Security monitoring.
- Access control.
- Security testing authorisation.
- Evidence protection.
- Incident investigation.
- Documentation and maintenance.

Individual system responsibilities are defined in the system architecture document.

## 6. Asset Dependencies

The following dependencies affect Vigilith's operation.

| Asset | Dependency | Potential Impact |
|---|---|---|
| Wazuh Manager | Network connectivity and Wazuh Agent communication | Loss of endpoint event processing |
| Wazuh Indexer | Available storage and indexing services | Reduced event search and investigation capability |
| Wazuh Dashboard | Indexer and dashboard services | Reduced analyst visibility |
| Ubuntu Endpoint | Operating system and Wazuh Agent | Endpoint telemetry interruption |
| VirtualBox | Host resources and virtual networking | Laboratory availability |
| Kali Linux | VirtualBox and network configuration | Security testing interruption |
| GitHub Repository | Internet connectivity and GitHub availability | Reduced remote version-control access |

## 7. Network Zones and Trust Boundaries

The laboratory contains several logical security zones.

| Zone ID | Zone | Description | Trust Level |
|---|---|---|---|
| VIG-ZN-001 | Host Environment | Ubuntu workstation and host services | Restricted |
| VIG-ZN-002 | Monitoring Environment | Wazuh Manager, Indexer and Dashboard | Highly Restricted |
| VIG-ZN-003 | Testing Environment | Kali Linux and authorised testing activities | Untrusted for production purposes |
| VIG-ZN-004 | External Network | Internet and external services | Untrusted |
| VIG-ZN-005 | Cloud Environment | Future AWS monitoring integration | Separately controlled |

These are logical classifications. Their exact network segmentation and enforcement mechanisms must be validated during implementation.

## 8. Asset Attack Surface

Each asset introduces potential security exposure.

| Asset | Potential Attack Surface | Security Concern |
|---|---|---|
| Ubuntu Host | SSH, local accounts, services and applications | Endpoint compromise |
| Wazuh Manager | Management interfaces and agent communication | Unauthorised administrative access |
| Wazuh Indexer | Indexing and API interfaces | Unauthorised access to security data |
| Wazuh Dashboard | Web interface and authentication | Account compromise |
| Kali Linux | Security tools and network interfaces | Unauthorised testing activity |
| VirtualBox | Hypervisor and virtual network configuration | Host-to-guest security boundary failure |
| GitHub Repository | Repository access and software dependencies | Unauthorised changes or sensitive data exposure |
| AWS Environment | IAM, APIs and cloud resources | Cloud account compromise |

These are potential attack surfaces, not confirmed vulnerabilities.

## 9. Monitoring and Telemetry Requirements

| Asset | Expected Telemetry | Monitoring Objective |
|---|---|---|
| Ubuntu Host | Authentication, system and process events | Detect suspicious endpoint behaviour |
| Wazuh Manager | Service health and security events | Monitor central detection infrastructure |
| Wazuh Indexer | Service and storage health | Maintain security event availability |
| Wazuh Dashboard | Authentication and service events | Monitor analyst access |
| Kali Linux | Testing activity and relevant network events | Support controlled security validation |
| Network Interfaces | Connection and traffic metadata | Identify suspicious communications |
| AWS Environment | CloudTrail and selected cloud security findings | Detect suspicious cloud activity |

Telemetry availability depends on actual configuration and successful integration.

## 10. Asset Protection Requirements

Vigilith will apply the following protection requirements:

### Identity and Access Management

- Apply least-privilege permissions.
- Restrict administrative access.
- Protect privileged credentials.
- Use strong authentication.

### Infrastructure Security

- Restrict unnecessary network exposure.
- Separate testing and monitoring environments.
- Protect management interfaces.
- Monitor critical services.

### Data and Evidence Protection

- Restrict access to security logs.
- Preserve relevant investigation evidence.
- Protect sensitive configuration files.
- Maintain secure backups where appropriate.
- Avoid exposing credentials or sensitive logs in public repositories.

### Operational Resilience

- Monitor service availability.
- Identify telemetry interruptions.
- Maintain documented recovery procedures.
- Evaluate resource utilisation.

## 11. Asset Risk Prioritisation

Asset criticality will inform security monitoring and incident response priorities.

For example:

1. Compromise of the Wazuh Manager could undermine central monitoring and detection.
2. Compromise of the Indexer could affect security evidence availability and integrity.
3. Compromise of the Ubuntu endpoint could disrupt endpoint visibility.
4. Compromise of Kali could undermine the integrity of controlled testing.
5. Compromise of the GitHub repository could expose project documentation or introduce unauthorised changes.

Asset criticality does not independently establish incident severity. Actual risk must also consider threat likelihood, exposure, vulnerabilities and business impact.

## 12. Inventory Maintenance

The inventory must be reviewed whenever:

- New assets are introduced.
- Existing assets are removed.
- Network architecture changes.
- New monitoring integrations are deployed.
- Asset ownership or responsibilities change.
- Security incidents reveal previously unidentified assets or dependencies.

Each new asset must receive a unique identifier and an appropriate classification.

## 13. Document Status

This document establishes the initial asset inventory for Vigilith.

Asset statuses distinguish existing laboratory resources from planned integrations.

Asset ownership, network placement, telemetry availability and security controls must be verified and updated as implementation progresses.
