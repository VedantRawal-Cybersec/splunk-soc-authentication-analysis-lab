# Remediation Recommendations

## Immediate Actions

1. Verify whether the successful VPN login was authorized.
2. Review authentication logs for the affected user account.
3. Reset passwords for high-risk or privileged accounts if activity is not confirmed as legitimate.
4. Enable MFA for VPN and privileged access.
5. Review VPN session logs for post-login activity.

## Short-Term Controls

1. Create a Splunk alert for repeated failures followed by a successful login.
2. Monitor privileged accounts such as `admin`, `administrator`, `root`, and `itadmin`.
3. Apply account lockout or rate-limiting policies.
4. Monitor unusual login countries or source IPs.
5. Review internal and external authentication sources separately.

## Long-Term Improvements

1. Implement conditional access policies.
2. Establish a baseline of normal login behavior.
3. Create dashboards for authentication monitoring.
4. Build SOC playbooks for suspicious login investigation.
5. Map detections to MITRE ATT&CK for consistent reporting.

## Suggested Splunk Alert Logic

Trigger an alert when the same source IP has 20 or more failures followed by a successful login.

```spl
index=failed_login_lab
| sort 0 _time
| streamstats count(eval(action="failure")) as previous_failures by src_ip
| where action="success" AND previous_failures>=20
| table _time src_ip user dest_host app action previous_failures reason severity country
```
