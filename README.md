# Splunk SOC Authentication Analysis Lab

A recruiter-ready SOC investigation lab using **Splunk Enterprise**, **SPL**, **MITRE ATT&CK mapping**, and structured incident reporting.

## Project Overview

This project demonstrates how a junior SOC analyst can investigate authentication logs, identify suspicious login behavior, validate evidence, map findings to MITRE ATT&CK, and write a clear incident report.

The lab is based on a controlled dataset named `failed_login_lab` and is intended for defensive cybersecurity learning, SOC portfolio proof, and internship applications.

## What This Lab Demonstrates

- Splunk SIEM investigation workflow
- SPL query writing
- Authentication log analysis
- Failed login trend analysis
- Suspicious source IP identification
- Successful login after repeated failures detection
- MITRE ATT&CK mapping
- Incident report writing
- Business impact explanation
- Remediation planning

## Repository Structure

```text
splunk-soc-authentication-analysis-lab/
├── README.md
├── spl_queries/
│   └── splunk_queries.md
├── docs/
│   ├── mitre_mapping.md
│   └── remediation.md
├── reports/
│   ├── incident_report.md
│   └── Splunk_Failed_Login_Detection_Report.pdf
└── screenshots/
    ├── authentication-summary.svg
    ├── failed-logins-by-source-ip.svg
    ├── suspicious-source-ips-targeted-users.svg
    └── successful-login-after-failures.svg
```

## Key Finding

The investigation identified a high-risk authentication pattern:

- `195` authentication events reviewed
- `96` failed login events
- `99` successful login events
- `7` unique source IPs
- `20` unique users
- One source IP generated `45` failed login attempts
- Multiple user accounts were targeted
- A later successful VPN login was observed for the `admin` account after repeated failures

## MITRE ATT&CK Mapping

| Technique ID | Technique | Relevance |
|---|---|---|
| T1110 | Brute Force | Repeated failed authentication attempts |
| T1078 | Valid Accounts | Successful login after repeated failures |
| T1133 | External Remote Services | VPN gateway authentication involved |
| T1589 | Gather Victim Identity Information | Multiple usernames targeted |

## Included Reports

- [SPL Detection Queries](spl_queries/splunk_queries.md)
- [Incident Report](reports/incident_report.md)
- [MITRE Mapping](docs/mitre_mapping.md)
- [Remediation Plan](docs/remediation.md)

## Visual Evidence

- [Authentication Summary](screenshots/authentication-summary.svg)
- [Failed Logins by Source IP](screenshots/failed-logins-by-source-ip.svg)
- [Suspicious Source IPs Targeting Multiple Users](screenshots/suspicious-source-ips-targeted-users.svg)
- [Successful Login After Failures](screenshots/successful-login-after-failures.svg)

## Resume Bullet

Built a Splunk-based SOC investigation lab to analyze authentication logs, detect suspicious login patterns, identify successful login after repeated failures, map findings to MITRE ATT&CK, and produce a structured incident report with remediation recommendations.

## LinkedIn Project Description

This project demonstrates hands-on SOC analysis using Splunk Enterprise. I investigated authentication logs, identified suspicious authentication patterns, mapped the activity to MITRE ATT&CK, and documented the case with business impact and remediation actions.

## Ethical Usage

This project is based on a controlled lab dataset and is intended only for ethical, defensive, and educational cybersecurity learning. Do not test systems, applications, or networks without explicit authorization.
