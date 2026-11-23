# Vigilith STRIDE Threat Model

## 1. Document Information

| Attribute | Description |
|---|---|
| Project | Vigilith |
| Document | STRIDE Threat Model |
| Version | 1.0 |
| Status | Initial Threat Model |
| Methodology | STRIDE |
| Risk Assessment | Likelihood × Impact |
| Environment | Security Monitoring Laboratory |

## 2. Purpose

This document identifies potential threats against the Vigilith security monitoring and incident response platform.

It evaluates how an attacker could compromise monitored endpoints, manipulate security telemetry, disrupt monitoring infrastructure, obtain unauthorised access or interfere with incident investigations.

The threat model establishes security risks, recommended mitigations and validation requirements.

The scenarios documented here represent potential architectural threats. They are not claims of confirmed vulnerabilities, exploitation or security incidents.

## 3. Scope and Assumptions

The threat model covers:

- Ubuntu engineering workstation.
- Wazuh Agent.
- Wazuh Manager.
- Wazuh Indexer.
- Wazuh Dashboard.
- Kali Linux testing environment.
- VirtualBox infrastructure.
- Host-only and bridged networking.
- GitHub project repository.
- Planned AWS monitoring integrations.
- Future network monitoring and incident response integrations.

The existing laboratory infrastructure is distinguished from planned integrations.

The threat model assumes that an attacker may obtain initial access through compromised credentials, vulnerable services, malicious software or an authorised user's compromised account.

## 4. Security Objectives

Vigilith must protect the following security properties:

### Confidentiality

Prevent unauthorised access to security logs, credentials, configuration files and investigation evidence.

### Integrity

Prevent unauthorised modification or deletion of telemetry, detection rules, security configurations and investigation records.

### Availability

Maintain the availability of monitoring infrastructure, security telemetry and investigation capabilities.

### Accountability

Ensure important administrative and security-related activities can be traced to identifiable users or processes.

### Monitoring Resilience

Prevent compromise of an individual endpoint from automatically disabling the central monitoring infrastructure.

## 5. Trust Boundaries and Attack Surfaces

| Boundary ID | Trust Boundary | Potential Threat |
|---|---|---|
| TB-01 | Ubuntu host and VirtualBox guests | Host-to-guest isolation failure |
| TB-02 | Wazuh Agent and Manager | Agent impersonation or telemetry disruption |
| TB-03 | Kali and monitoring infrastructure | Unauthorised testing or lateral movement |
| TB-04 | Laboratory and external networks | Malicious inbound or outbound communications |
| TB-05 | Local infrastructure and AWS | Cloud identity compromise |
| TB-06 | Monitoring and response integrations | Unauthorised response actions |

These boundaries require validation against the actual network and service configuration.

## 6. STRIDE Threat Analysis

### 6.1 Spoofing

**Threat ID:** VIG-THR-001

**Affected assets:** Wazuh Agent, Wazuh Manager, Ubuntu endpoint.

**Scenario:**

An attacker attempts to impersonate a legitimate monitoring endpoint or abuse compromised agent credentials to establish unauthorised communication with the Wazuh Manager.

**Potential weakness:**

Insufficient protection of agent authentication material or inadequate access controls.

**Potential impact:**

- Untrusted telemetry entering the monitoring environment.
- Misleading security events.
- Reduced confidence in endpoint identity.
- Potential disruption of monitoring operations.

**Recommended controls:**

- Protect agent authentication material.
- Restrict agent enrolment.
- Use secure agent-manager communication.
- Monitor agent identity and connection changes.
- Restrict access to management interfaces.

**Validation approach:**

Review agent authentication and enrolment configuration. Conduct controlled tests to establish whether unauthorised endpoints can enrol or communicate.

### 6.2 Tampering

**Threat ID:** VIG-THR-002

**Affected assets:** Ubuntu endpoint, Wazuh Agent, Wazuh Manager, Indexer and security evidence.

**Scenario:**

An attacker obtains root privileges on the Ubuntu endpoint and attempts to modify local security logs, disable monitoring processes or alter security configurations.

**Potential weakness:**

Reliance on locally stored logs and insufficient protection of security configuration files.

**Potential impact:**

- Loss of investigative evidence.
- Reduced endpoint visibility.
- Detection evasion.
- Compromised incident investigations.

**Recommended controls:**

- Centralise security log collection.
- Configure File Integrity Monitoring.
- Monitor critical configuration changes.
- Restrict administrative permissions.
- Protect centralised security evidence.
- Maintain secure backups.
- Monitor unexpected agent termination.

**Validation approach:**

Perform controlled modifications to selected test files and verify whether monitoring identifies the changes. Test agent disruption and confirm whether telemetry-loss conditions are detectable.

### 6.3 Repudiation

**Threat ID:** VIG-THR-003

**Affected assets:** Wazuh Dashboard, Wazuh Manager, Ubuntu endpoint and administrative accounts.

**Scenario:**

An administrator performs an unauthorised configuration change and attempts to deny responsibility because the activity was not adequately recorded.

**Potential weakness:**

Insufficient administrative audit logging or shared administrative credentials.

**Potential impact:**

- Reduced accountability.
- Difficult incident reconstruction.
- Inability to establish the origin of configuration changes.
- Weak investigation evidence.

**Recommended controls:**

- Use individually assigned administrative accounts.
- Monitor privileged activities.
- Maintain centralised audit records.
- Restrict access to security logs.
- Record important configuration changes.
- Maintain reliable timestamps.

**Validation approach:**

Review administrative audit coverage and verify that authorised configuration changes can be attributed to identifiable accounts.

### 6.4 Information Disclosure

**Threat ID:** VIG-THR-004

**Affected assets:** Wazuh Dashboard, Indexer, GitHub repository and security logs.

**Scenario:**

An attacker obtains unauthorised access to security dashboards, indexed events, credentials or sensitive investigation records.

**Potential weakness:**

Weak authentication, excessive permissions, exposed management interfaces or accidental publication of sensitive information.

**Potential impact:**

- Exposure of security infrastructure details.
- Disclosure of sensitive endpoint information.
- Credential compromise.
- Increased opportunities for further attacks.

**Recommended controls:**

- Apply least-privilege access.
- Protect administrative credentials.
- Restrict management interfaces.
- Review repository contents before publication.
- Exclude credentials and sensitive logs from Git.
- Protect sensitive investigation evidence.

**Validation approach:**

Review dashboard access controls, repository contents and permissions. Confirm that unauthorised users cannot access restricted information.

### 6.5 Denial of Service

**Threat ID:** VIG-THR-005

**Affected assets:** Wazuh Manager, Indexer, Dashboard, Ubuntu workstation and virtual infrastructure.

**Scenario:**

An attacker generates excessive security events, consumes system resources or disrupts network communication to overwhelm the monitoring infrastructure.

**Potential weakness:**

Insufficient resource management, inadequate event filtering or limited capacity.

**Potential impact:**

- Delayed event processing.
- Loss of telemetry.
- Indexing failures.
- Dashboard unavailability.
- Reduced incident response capability.

**Recommended controls:**

- Monitor CPU, memory and storage utilisation.
- Tune noisy detection rules.
- Apply appropriate event filtering.
- Monitor service availability.
- Establish resource thresholds.
- Maintain recovery procedures.

**Validation approach:**

Use controlled event generation and resource monitoring to determine how the environment behaves under increased workload.

### 6.6 Elevation of Privilege

**Threat ID:** VIG-THR-006

**Affected assets:** Ubuntu endpoint, Wazuh Manager, Indexer, Dashboard and AWS IAM environment.

**Scenario:**

An attacker exploits excessive permissions, compromised administrator credentials or a vulnerable service to obtain privileges beyond those initially authorised.

**Potential weakness:**

Excessive administrative permissions, weak access controls or insecure service configurations.

**Potential impact:**

- Unauthorised administrative control.
- Modification of detection rules.
- Security monitoring disruption.
- Exposure of sensitive information.
- Potential compromise of connected infrastructure.

**Recommended controls:**

- Enforce least privilege.
- Separate administrative and ordinary user accounts.
- Restrict privileged access.
- Use MFA where supported.
- Monitor privileged command execution.
- Review IAM permissions and trust policies.
- Apply security updates.

**Validation approach:**

Review account permissions and perform authorised privilege escalation simulations in the isolated testing environment.

## 7. Risk Assessment Register

The following ratings are initial estimates based on potential impact and assumed likelihood.

| Threat ID | Threat | Likelihood | Impact | Score | Initial Risk |
|---|---|---:|---:|---:|---|
| VIG-THR-001 | Agent impersonation | 3 | 4 | 12 | High |
| VIG-THR-002 | Security telemetry tampering | 4 | 5 | 20 | Critical |
| VIG-THR-003 | Administrative repudiation | 3 | 3 | 9 | Medium |
| VIG-THR-004 | Information disclosure | 3 | 4 | 12 | High |
| VIG-THR-005 | Monitoring denial of service | 3 | 4 | 12 | High |
| VIG-THR-006 | Privilege escalation | 3 | 5 | 15 | High |

These ratings represent preliminary inherent risk estimates before the effectiveness of recommended mitigations has been established.

## 8. Threat Prioritisation

Threats are prioritised according to their potential impact on monitoring integrity, infrastructure security, evidence preservation and operational availability.

### Priority 1: Security Telemetry Integrity

An attacker who manipulates security telemetry may conceal malicious activity, undermine detection capabilities and compromise the reliability of incident investigations.

### Priority 2: Privileged Access Security

Unauthorised privilege escalation could provide access to critical monitoring components, security configurations and administrative functions. A successful compromise could affect multiple platform assets.

### Priority 3: Monitoring Availability

Disruption of the Wazuh Manager, Indexer or supporting infrastructure could create monitoring gaps, delay detection and reduce incident response capability.

### Priority 4: Information Protection

Unauthorised access to security logs, credentials and infrastructure configurations could expose sensitive information and create opportunities for further compromise.

### Priority 5: Identity and Accountability

Weak identity attribution and insufficient administrative audit records could obstruct investigations and make it difficult to establish responsibility for security-related actions.

This prioritisation is preliminary and will be reviewed as implementation and validation evidence becomes available.

## 9. Defence-in-Depth Strategy

Vigilith will use multiple defensive layers.

| Layer | Security objective | Example controls |
|---|---|---|
| Identity | Protect accounts and privileges | MFA, least privilege, account monitoring |
| Endpoint | Protect monitored systems | FIM, audit logging, process monitoring |
| Network | Restrict unauthorised communication | Segmentation, firewall rules, traffic monitoring |
| SIEM | Protect central monitoring | Restricted administration, service monitoring |
| Data | Preserve security evidence | Access controls, secure backups, integrity protection |
| Cloud | Protect cloud identities and resources | IAM, CloudTrail, GuardDuty |
| Operations | Support controlled response | Incident procedures, authorisation and evidence preservation |

## 10. Residual Risk and Control Validation

Implementing a security control does not automatically eliminate a threat.

Each mitigation must be evaluated for effectiveness.

Residual risk should consider:

- Whether the control is implemented.
- Whether the control is functioning.
- Whether the threat can bypass it.
- Whether additional attack paths exist.
- Whether the control introduces operational limitations.

Risk ratings should only be reduced when supporting evidence demonstrates that risk has been meaningfully mitigated.

## 11. Threat Model Maintenance

The threat model must be reviewed when:

- New assets are introduced.
- Network trust boundaries change.
- New services or integrations are deployed.
- New vulnerabilities are identified.
- Detection capabilities change.
- Security incidents reveal previously unidentified attack paths.

Threat scenarios must remain aligned with the actual system architecture and asset inventory.

## 12. Document Status

This document establishes the initial STRIDE threat model for Vigilith.

The threats and risk ratings are architectural assessments, not confirmed vulnerabilities or incidents.

Recommended security controls will be implemented and validated progressively.

The threat model will evolve as the platform develops.

---
