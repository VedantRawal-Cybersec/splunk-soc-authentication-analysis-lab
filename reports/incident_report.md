# Incident Report - Authentication Log Investigation

## Executive Summary

A Splunk authentication log review identified a high-risk login sequence. One source IP created many failed login events, several user accounts appeared in the activity, and a later successful VPN login was observed for the `admin` account.

This report documents the evidence, risk, and recommended defensive actions.

## Incident Details

| Field | Value |
|---|---|
| Incident Type | Suspicious authentication activity |
| Severity | Critical |
| Primary Source IP | 45.77.32.11 |
| Secondary Source IP | 185.220.101.23 |
| Affected User | admin |
| Affected Asset | vpn-gateway |
| Application | vpn |
| Country | Netherlands |
| Detection Tool | Splunk Enterprise |

## Evidence Summary

- Total authentication events reviewed: 195
- Failed login events: 96
- Successful login events: 99
- Unique source IPs: 7
- Unique users: 20
- Highest failed login source: 45.77.32.11 with 45 failures
- Second highest failed login source: 185.220.101.23 with 30 failures
- A successful VPN login was observed after repeated failures

## Detection Query

```spl
index=failed_login_lab
| sort 0 _time
| streamstats count(eval(action="failure")) as previous_failures by src_ip
| where action="success" AND previous_failures>=20
| table _time src_ip user dest_host app action previous_failures reason severity country
```

## Analyst Assessment

The activity is high risk because it includes repeated failures and a later successful VPN login. In a real SOC environment, this should trigger review of the user account, VPN session, and related authentication logs.

Confidence: High
Severity: Critical
Priority: Immediate review recommended

## Recommended Actions

1. Confirm whether the `admin` VPN login was legitimate.
2. Review VPN activity associated with the source IP.
3. Reset the affected account password if the activity is not verified.
4. Enable MFA for VPN and privileged accounts.
5. Create a Splunk alert for repeated failures followed by success.
6. Add privileged accounts to a SIEM watchlist.

## Conclusion

This lab demonstrates SOC-style log investigation using Splunk Enterprise. The workflow includes data review, SPL detection logic, authentication analysis, incident documentation, and remediation planning.
