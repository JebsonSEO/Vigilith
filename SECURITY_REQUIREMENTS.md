# Vigilith Security Requirements and Acceptance Criteria

## 1. Document Information

| Attribute | Description |
|---|---|
| Project | Vigilith |
| Document | Security Requirements and Acceptance Criteria |
| Version | 1.0 |
| Status | Initial Requirements Baseline |
| Related Documents | SYSTEM_ARCHITECTURE.md, ASSET_INVENTORY.md, THREAT_MODEL.md |

## 2. Purpose

This document defines the security requirements and acceptance criteria governing the design, implementation and validation of Vigilith.

It translates identified architectural threats into measurable security capabilities.

Each requirement establishes an expected security outcome and a method for verifying whether that outcome has been achieved.

The requirements will guide implementation, testing and future security improvements.

## 3. Scope

The requirements cover:

- Endpoint security monitoring.
- Identity and access management.
- Security telemetry collection.
- Network security monitoring.
- SIEM operations.
- Detection engineering.
- Security evidence protection.
- Incident investigation.
- Incident response.
- Cloud security monitoring.
- Platform availability and resilience.

Requirements relating to planned integrations will remain pending until those integrations are implemented.

## 4. Requirement Classification

Requirements are classified into three priority levels.

| Priority | Meaning |
|---|---|
| P1 - Critical | Essential security requirements that must be satisfied before the relevant capability is accepted. |
| P2 - High | Important requirements that significantly strengthen platform security. |
| P3 - Medium | Supporting requirements that improve operational maturity and resilience. |

Implementation status:

| Status | Meaning |
|---|---|
| Planned | Requirement identified but implementation has not started. |
| In Progress | Implementation has started. |
| Implemented | Control has been configured. |
| Validated | Testing has demonstrated that the control works as intended. |
| Not Applicable | Requirement does not apply to the current implementation scope. |

Implementation and validation status must be supported by evidence.

## 5. Identity and Access Management Requirements

### SEC-IAM-001: Least-Privilege Access

Priority: P1

Requirement:

Administrative and user accounts must receive only the permissions necessary to perform their assigned responsibilities.

Acceptance criteria:

- Ordinary accounts cannot perform unauthorised administrative operations.
- Administrative privileges are restricted to authorised accounts.
- Excessive permissions can be identified through access reviews.

Verification:

Review account permissions and perform authorised access-control tests.

### SEC-IAM-002: Administrative Authentication

Priority: P1

Requirement:

Administrative interfaces must enforce appropriate authentication controls.

Acceptance criteria:

- Unauthenticated access to protected management interfaces is denied.
- Administrative authentication mechanisms are documented.
- MFA is enabled where supported and appropriate.

Verification:

Conduct controlled authentication and access-control tests.

### SEC-IAM-003: Endpoint Identity Protection

Priority: P1

Requirement:

The monitoring infrastructure must establish and maintain trusted endpoint identities.

Acceptance criteria:

- Endpoint enrolment follows an authorised process.
- Agent authentication material is protected.
- Unexpected endpoint identity or connection changes can be investigated.

Verification:

Review Wazuh agent enrolment and authentication configuration.

## 6. Endpoint Security Requirements

### SEC-END-001: Endpoint Telemetry Collection

Priority: P1

Requirement:

Monitored endpoints must generate and transmit relevant security telemetry to the central monitoring infrastructure.

Acceptance criteria:

- The endpoint agent successfully communicates with the manager.
- Configured security events are received centrally.
- Collection failures can be identified.

Verification:

Generate controlled endpoint events and confirm their appearance in Wazuh.

### SEC-END-002: File Integrity Monitoring

Priority: P1

Requirement:

Critical system files and selected security configurations must be monitored for unauthorised modifications.

Acceptance criteria:

- Selected files are included in the monitoring configuration.
- Controlled modifications generate observable alerts.
- Alert records identify the affected file and relevant event details.

Verification:

Modify a designated test file and inspect the resulting Wazuh alert.

### SEC-END-003: Endpoint Monitoring Health

Priority: P1

Requirement:

The platform must identify unexpected endpoint monitoring interruptions.

Acceptance criteria:

- Agent disconnection can be identified.
- Monitoring-health information is available to the analyst.
- Recovery of endpoint communication can be verified.

Verification:

Perform a controlled agent shutdown and restart in the laboratory.

## 7. Network Security Requirements

### SEC-NET-001: Network Segmentation

Priority: P1

Requirement:

Network communication between the engineering workstation, virtual machines and monitoring infrastructure must be restricted according to operational requirements.

Acceptance criteria:

- Required communication paths are documented.
- Unnecessary network exposure is restricted.
- Unauthorised communication attempts can be investigated.

Verification:

Review network configurations and conduct authorised connectivity tests.

### SEC-NET-002: Network Threat Visibility

Priority: P2

Requirement:

The platform should provide visibility into relevant network activity and suspicious communication patterns.

Acceptance criteria:

- Selected network telemetry sources are documented.
- Suspicious network activity can be identified from collected events.
- Network alerts retain sufficient information for investigation.

Verification:

Validate network monitoring using controlled traffic and available network analysis tools.

## 8. SIEM and Detection Engineering Requirements

### SEC-SIEM-001: Centralised Security Monitoring

Priority: P1

Requirement:

Security events from configured endpoints and monitoring sources must be collected and made available for investigation.

Acceptance criteria:

- Configured event sources are identifiable.
- Events can be retrieved from the central monitoring platform.
- Relevant event timestamps and source information are retained.

Verification:

Generate controlled events and confirm their ingestion and availability.

### SEC-SIEM-002: Detection Rule Effectiveness

Priority: P1

Requirement:

Detection rules must identify defined suspicious activities with sufficient context for investigation.

Acceptance criteria:

- Each implemented detection has a documented detection objective.
- Controlled test activity triggers the expected detection.
- Alerts contain relevant event information.
- Detection results are reproducible.

Verification:

Execute documented detection test cases and inspect generated alerts.

### SEC-SIEM-003: Detection Quality

Priority: P2

Requirement:

Detection rules must be reviewed and tuned to reduce unnecessary alerts without suppressing meaningful security events.

Acceptance criteria:

- Detection rules have documented purposes.
- False positives can be investigated.
- Changes to detection logic are recorded.
- Detection changes are tested before acceptance.

Verification:

Review alert samples and perform controlled positive and negative tests.

## 9. Security Evidence and Information Protection

### SEC-DATA-001: Security Evidence Integrity

Priority: P1

Requirement:

Security evidence must be protected against unauthorised modification and deletion.

Acceptance criteria:

- Access to investigation evidence is restricted.
- Evidence collection and handling procedures are documented.
- Integrity verification is available where technically appropriate.

Verification:

Review evidence permissions and perform integrity checks on selected test evidence.

### SEC-DATA-002: Sensitive Information Protection

Priority: P1

Requirement:

Credentials, tokens, private keys and sensitive investigation information must not be exposed through public project materials.

Acceptance criteria:

- Secrets are excluded from Git commits.
- Sensitive logs are sanitised before publication.
- Access to restricted investigation information is controlled.

Verification:

Review repository contents and inspect relevant Git changes before publication.

### SEC-DATA-003: Auditability

Priority: P2

Requirement:

Important administrative and security-related activities must produce records sufficient to support investigation and accountability.

Acceptance criteria:

- Relevant administrative activities are logged.
- Event timestamps are available.
- Important configuration changes can be investigated.

Verification:

Perform controlled administrative activities and inspect the available audit records.

## 10. Incident Response Requirements

### SEC-IR-001: Incident Identification

Priority: P1

Requirement:

The platform must provide sufficient security telemetry and detection information to support incident identification.

Acceptance criteria:

- Defined suspicious activities generate observable security events.
- Events contain information useful for initial triage.
- Detection evidence can be retrieved.

Verification:

Execute documented security scenarios and inspect the resulting events.

### SEC-IR-002: Investigation Support

Priority: P1

Requirement:

Analysts must be able to investigate security events using available endpoint, network and cloud evidence.

Acceptance criteria:

- Security events can be correlated with relevant supporting evidence.
- Investigation findings are documented.
- Evidence sources and limitations are identified.

Verification:

Conduct a controlled investigation and produce an incident investigation record.

### SEC-IR-003: Controlled Incident Response

Priority: P1

Requirement:

Response actions must be authorised, documented and appropriate to the incident.

Acceptance criteria:

- Response procedures identify the required authorisation.
- Response actions are recorded.
- Recovery and post-incident verification are documented.

Verification:

Execute an approved response simulation in the isolated laboratory.

## 11. Cloud Security Requirements

The following requirements apply when AWS integration is implemented.

### SEC-CLD-001: Cloud Activity Monitoring

Priority: P1

Requirement:

Relevant AWS account activity must be collected and monitored through appropriate cloud audit sources.

Acceptance criteria:

- CloudTrail activity is available for configured accounts and event types.
- Relevant security events can be identified.
- Collection failures can be investigated.

Verification:

Generate controlled AWS activity and confirm the corresponding audit records.

### SEC-CLD-002: Cloud Threat Detection

Priority: P1

Requirement:

The platform must support the identification and investigation of relevant cloud security threats.

Acceptance criteria:

- Configured cloud detection sources produce observable findings.
- Findings retain relevant resource and event context.
- Analysts can investigate the available evidence.

Verification:

Validate configured cloud detections using approved test scenarios.

### SEC-CLD-003: Cloud Identity Security

Priority: P1

Requirement:

Cloud identities and permissions must follow least-privilege principles.

Acceptance criteria:

- IAM permissions are reviewed.
- Excessive permissions are identifiable.
- Administrative access is appropriately restricted.

Verification:

Review IAM policies, roles and trust relationships.

## 12. Availability and Resilience Requirements

### SEC-AVL-001: Monitoring Service Availability

Priority: P1

Requirement:

The operational health of critical monitoring components must be observable.

Acceptance criteria:

- Critical service states can be checked.
- Unexpected service interruptions can be identified.
- Recovery procedures are documented.

Verification:

Inspect service health and perform controlled service interruption tests.

### SEC-AVL-002: Resource Monitoring

Priority: P2

Requirement:

System resources supporting the monitoring infrastructure must be monitored to identify conditions that could affect service availability.

Acceptance criteria:

- CPU, memory and storage utilisation can be inspected.
- Resource pressure can be identified.
- Resource-related monitoring failures can be investigated.

Verification:

Review resource metrics during normal and controlled elevated workloads.

## 13. Requirements Traceability Matrix

| Requirement ID | Related Threat | Security Objective | Verification | Status |
|---|---|---|---|---|
| SEC-IAM-001 | VIG-THR-006 | Least privilege | Access-control testing | Planned |
| SEC-IAM-002 | VIG-THR-004, VIG-THR-006 | Authentication security | Authentication testing | Planned |
| SEC-IAM-003 | VIG-THR-001 | Trusted endpoint identity | Enrolment review | Planned |
| SEC-END-001 | VIG-THR-002 | Telemetry visibility | Event ingestion test | Planned |
| SEC-END-002 | VIG-THR-002 | Integrity monitoring | FIM test | Planned |
| SEC-END-003 | VIG-THR-005 | Monitoring availability | Agent interruption test | Planned |
| SEC-NET-001 | VIG-THR-001, VIG-THR-006 | Network protection | Connectivity testing | Planned |
| SEC-NET-002 | VIG-THR-001, VIG-THR-005 | Network visibility | Traffic analysis | Planned |
| SEC-SIEM-001 | VIG-THR-002 | Centralised monitoring | Event ingestion test | Planned |
| SEC-SIEM-002 | VIG-THR-002, VIG-THR-005 | Detection effectiveness | Detection testing | Planned |
| SEC-SIEM-003 | VIG-THR-002 | Detection quality | Rule validation | Planned |
| SEC-DATA-001 | VIG-THR-002 | Evidence integrity | Integrity verification | Planned |
| SEC-DATA-002 | VIG-THR-004 | Information protection | Repository review | Planned |
| SEC-DATA-003 | VIG-THR-003 | Accountability | Audit review | Planned |
| SEC-IR-001 | VIG-THR-002 | Incident identification | Scenario testing | Planned |
| SEC-IR-002 | VIG-THR-002, VIG-THR-004 | Investigation capability | Investigation exercise | Planned |
| SEC-IR-003 | VIG-THR-005, VIG-THR-006 | Controlled response | Response simulation | Planned |
| SEC-CLD-001 | VIG-THR-002 | Cloud audit visibility | CloudTrail validation | Planned |
| SEC-CLD-002 | VIG-THR-002, VIG-THR-004 | Cloud threat detection | Detection validation | Planned |
| SEC-CLD-003 | VIG-THR-006 | Cloud identity protection | IAM review | Planned |
| SEC-AVL-001 | VIG-THR-005 | Service availability | Service health testing | Planned |
| SEC-AVL-002 | VIG-THR-005 | Resource resilience | Resource monitoring | Planned |

## 14. Acceptance and Validation Policy

A security requirement must not be marked validated merely because a configuration exists.

Validation requires evidence demonstrating that the expected security outcome was achieved.

Acceptable evidence may include:

- Wazuh alerts and event records.
- Configuration inspection.
- Authentication test results.
- Network connectivity test results.
- File integrity monitoring events.
- CloudTrail records.
- Detection test reports.
- Incident investigation records.
- Service health and resource metrics.

Testing must be authorised and performed within the approved laboratory or cloud environment.

## 15. Document Status

This document establishes the initial security requirements baseline for Vigilith.

Requirements will be implemented progressively according to priority and platform development milestones.

Each requirement will move through the defined implementation lifecycle as evidence becomes available.

Planned capabilities must not be represented as implemented or validated until their acceptance criteria have been satisfied.
