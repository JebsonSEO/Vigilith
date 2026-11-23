# SSH Authentication Detection Validation

## 1. Overview

This validation confirms that Vigilith can collect, decode, detect and display failed SSH authentication activity through the Wazuh SIEM.

The test used a controlled SSH authentication attempt against the local Ubuntu endpoint.

**Validation date:** 23 November 2026  
**Status:** Successfully Validated  
**Endpoint:** Ubuntu 26.04.1 LTS  
**SIEM:** Wazuh 4.14.8  
**Agent:** Ubuntu-CloudSec-Lab (ID: 001)

## 2. Test Objective

Verify that a failed SSH authentication attempt is:

- Generated and recorded by the Ubuntu SSH service.
- Collected by the Wazuh agent.
- Decoded by the Wazuh Manager.
- Matched against an applicable SSH detection rule.
- Indexed and displayed in the Wazuh Dashboard.

## 3. Test Procedure

A controlled SSH connection was initiated against localhost using a deliberately non-existent username.

The test used password authentication with public-key authentication disabled.

Target:
127.0.0.1

Test username:
vigilith_probe

Authentication result:
Permission denied.

No external system was targeted.

## 4. Endpoint Evidence

Ubuntu authentication logs recorded:

- Invalid user notification.
- PAM authentication failure.
- Failed password event.
- SSH connection closure.

The source address was 127.0.0.1.

These records confirmed that the SSH service generated and logged the expected authentication failure events.

## 5. Wazuh Detection Results

The Wazuh Manager successfully processed the endpoint telemetry.

Observed detection metadata:

| Field | Observed value |
|---|---|
| Rule ID | 5710 |
| Severity | Level 5 |
| Description | sshd: Attempt to login using a non-existent user |
| Decoder | sshd-session |
| Source IP | 127.0.0.1 |
| Source port | 40318 |
| Username | vigilith_probe |
| Agent | Ubuntu-CloudSec-Lab |
| Agent ID | 001 |

The Wazuh Dashboard returned two matching alerts for the controlled test.

The events represented related records from the same SSH connection, not two independent attack attempts.

## 6. Detection Coverage

| Validation component | Result |
|---|---|
| SSH event generation | PASS |
| Authentication log recording | PASS |
| Wazuh agent collection | PASS |
| Event decoding | PASS |
| Detection rule matching | PASS |
| Alert generation | PASS |
| Dashboard visibility | PASS |

## 7. MITRE ATT&CK Context

Wazuh rule 5710 includes an association with:

- T1110: Brute Force
- T1021.004: SSH

These are the rule's framework mappings. This isolated test does not establish that a brute-force attack or lateral movement occurred.

## 8. Security Assessment

The validation demonstrates that Vigilith can identify a failed SSH authentication attempt involving a non-existent username.

However, a single failed authentication event is insufficient to establish malicious intent or a brute-force campaign.

Further detection engineering should consider:

- Repeated authentication failures.
- Multiple targeted usernames.
- Repeated attempts from a common source address.
- Successful authentication following repeated failures.
- Unusual SSH access patterns.

## 9. Limitations

This was a controlled localhost validation.

The test does not establish:

- Detection of distributed brute-force activity.
- Detection of successful account compromise.
- Detection of external SSH attacks.
- Automated containment or response.
- Detection performance under high event volumes.

These capabilities require separate validation.

## 10. Conclusion

Vigilith successfully demonstrated end-to-end SSH authentication failure detection using Ubuntu endpoint telemetry and Wazuh 4.14.8.

The test confirmed event generation, collection, decoding, rule matching, alert indexing and Dashboard visibility.

This establishes a validated foundation for subsequent SSH threat detection and incident investigation scenarios.
