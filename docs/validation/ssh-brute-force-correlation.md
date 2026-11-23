# SSH Brute-Force Correlation Validation

## 1. Overview

This validation demonstrates Vigilith's ability to identify repeated SSH authentication failures and correlate individual events into a higher-severity security alert using Wazuh.

**Validation date:** 23 November 2026  
**Status:** Successfully Validated  
**Endpoint:** Ubuntu 26.04.1 LTS  
**SIEM:** Wazuh 4.14.8  
**Agent:** Ubuntu-CloudSec-Lab (ID: 001)

## 2. Objective

Validate whether repeated failed SSH authentication attempts from the same source IP can trigger Wazuh's built-in frequency-based detection rule.

The test was conducted against localhost using a deliberately non-existent username and incorrect passwords.

## 3. Test Configuration

| Parameter | Value |
|---|---|
| Source IP | 127.0.0.1 |
| Target | Local Ubuntu SSH service |
| Username | vigilith_probe |
| Authentication method | Password |
| Authentication result | Rejected |
| Required event frequency | 8 |
| Correlation timeframe | 120 seconds |

## 4. Detection Architecture

The validation exercised two detection layers.

### Rule 5710: Individual Event Detection

- Severity: Level 5
- Identifies SSH login attempts involving a non-existent user.
- Generates individual authentication failure alerts.

### Rule 5712: Frequency-Based Correlation

- Severity: Level 10
- Frequency: 8 matching events.
- Timeframe: 120 seconds.
- Correlation condition: Same source IP.
- Parent detection: Rule 5710.
- Description: SSH brute-force attempt involving a non-existent user.

The installed Wazuh ruleset provided this correlation capability without requiring a custom rule.

## 5. Observed Results

Wazuh successfully generated the Level 10 correlation alert after the controlled authentication failures.

| Field | Observed result |
|---|---|
| Rule ID | 5712 |
| Severity | Level 10 |
| Source IP | 127.0.0.1 |
| Username | vigilith_probe |
| Agent | Ubuntu-CloudSec-Lab |
| Correlation frequency | 8 |
| Correlation timeframe | 120 seconds |
| Dashboard result | 1 matching alert |

The alert was visible in the Wazuh Dashboard using the query:

`rule.id:5712 AND data.srcuser:vigilith_probe`

## 6. Validation Results

| Component | Result |
|---|---|
| SSH failure generation | PASS |
| Endpoint telemetry collection | PASS |
| Individual event detection | PASS |
| Frequency-based correlation | PASS |
| Same-source-IP correlation | PASS |
| Level 10 alert generation | PASS |
| Dashboard indexing and visibility | PASS |

## 7. MITRE ATT&CK Context

The built-in correlation rule maps to:

- T1110: Brute Force.

This mapping describes the detection rule's associated technique. The test was a controlled simulation, not evidence of an actual external attack or compromised account.

## 8. Security Assessment

The test demonstrates that Vigilith can distinguish an isolated authentication failure from a repeated pattern that meets a configured detection threshold.

The higher-severity alert provides a stronger signal for SOC investigation and prioritisation.

However, the test does not establish detection of distributed brute-force activity, password spraying, successful account compromise or automated containment.

## 9. Limitations

- All authentication attempts originated from localhost.
- The test used one non-existent username.
- The source IP was constant.
- No external attacker infrastructure was involved.
- No automated response or account containment was tested.
- Detection performance under high event volumes remains unvalidated.

## 10. Conclusion

Vigilith successfully demonstrated frequency-based SSH authentication failure correlation using the existing Wazuh ruleset.

The validation confirmed the transition from individual event detection at Level 5 to a higher-severity Level 10 correlation alert.

This establishes a functional foundation for further SOC detection engineering, threat investigation and incident response capabilities.
