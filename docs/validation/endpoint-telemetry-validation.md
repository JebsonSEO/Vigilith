# Endpoint Telemetry Integration and Detection Validation

## 1. Document Information

Project: Vigilith
Component: Endpoint Security Monitoring
Platform: Wazuh 4.14.8
Validation Date: 23 November 2026
Status: Successfully Validated

## 2. Objective

Validate the endpoint telemetry pipeline by generating a controlled
test event on the Ubuntu endpoint, processing it through Wazuh,
and confirming that the resulting security alert is searchable
in the Wazuh Dashboard.

## 3. Test Environment

- Ubuntu endpoint: Dell XPS 15 9570
- Wazuh Agent: 4.14.8
- Wazuh Manager: 4.14.8
- Wazuh Indexer: Active
- Wazuh Dashboard: Active
- Log source: systemd journal (journald)

## 4. Detection Implementation

A custom Wazuh decoder was created to identify the test marker:

VIGILITH_TEST:

A custom detection rule was configured:

- Rule ID: 10030
- Severity: Level 5
- Decoder: vigilith_test
- Description: Vigilith: Endpoint telemetry validation event detected.

## 5. Test Procedure

A controlled test event was generated on the Ubuntu endpoint.

Test marker:

VIGILITH_TEST: Ubuntu endpoint telemetry validation

The event was verified in the endpoint's authentication log
and system journal.

The Wazuh agent collected the journal event and forwarded it
to the Wazuh Manager.

The Manager processed the event using the custom decoder
and detection rule.

## 6. Validation Results

| Validation Component | Result |
|---|---|
| Endpoint event generation | PASS |
| Journal event verification | PASS |
| Custom decoder matching | PASS |
| Custom rule execution | PASS |
| Alert generation | PASS |
| Filebeat alert ingestion | PASS |
| Indexer connectivity | PASS |
| Dashboard event retrieval | PASS |

Dashboard evidence:

- Rule ID: 10030
- Rule level: 5
- Agent: Ubuntu-CloudSec-Lab
- Alert description: Vigilith: Endpoint telemetry validation event detected.
- Search result: 1 hit

## 7. Telemetry Processing Architecture

Ubuntu Endpoint
    |
    v
Systemd Journal
    |
    v
Wazuh Agent
    |
    v
Wazuh Manager
    |
    v
Custom Decoder
    |
    v
Custom Detection Rule
    |
    v
Wazuh alerts.json
    |
    v
Filebeat
    |
    v
Wazuh Indexer
    |
    v
Wazuh Dashboard

## 8. Security Significance

This validation demonstrates that Vigilith can collect a controlled
endpoint event, decode its content, apply a custom detection rule,
generate an alert, and make the alert searchable through the
monitoring interface.

It establishes a working foundation for subsequent detection
engineering and incident investigation scenarios.

## 9. Limitations

This was a controlled telemetry validation event, not a real
security incident.

The successful test does not establish that all endpoint log
sources, attack scenarios, or detection rules have been validated.

Further testing is required for authentication attacks,
privilege escalation, suspicious process execution, and
network-based threats.

## 10. Validation Status

Milestone: Endpoint Telemetry Integration

Result: Successfully Validated

Next Objective: Validate detection of repeated failed SSH
authentication attempts.
