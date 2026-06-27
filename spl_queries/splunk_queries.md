# Splunk SPL Queries

This file contains the SPL queries used in the Splunk SOC Authentication Analysis Lab.

## 1. Base Search

```spl
source="splunk_failed_login_lab (1).csv" host="ABHAY" index="failed_login_lab" sourcetype="csv"
```

Purpose: Validate that the dataset was imported and review raw authentication events.

---

## 2. Authentication Summary

```spl
index=failed_login_lab
| stats count as total_events,
        count(eval(action="failure")) as failed_logins,
        count(eval(action="success")) as successful_logins,
        dc(src_ip) as unique_source_ips,
        dc(user) as unique_users
```

Observed result:

| Metric | Value |
|---|---:|
| Total events | 195 |
| Failed logins | 96 |
| Successful logins | 99 |
| Unique source IPs | 7 |
| Unique users | 20 |

---

## 3. Failed Logins by Source IP

```spl
index=failed_login_lab action=failure
| stats count by src_ip
| sort - count
```

Purpose: Identify source IPs generating the highest failed login volume.

| Source IP | Failed Login Count |
|---|---:|
| 45.77.32.11 | 45 |
| 185.220.101.23 | 30 |
| 10.10.5.23 | 6 |
| 10.10.5.22 | 4 |
| 10.10.5.24 | 4 |
| 10.10.5.25 | 4 |
| 10.10.5.21 | 3 |

---

## 4. Failed Logins by User

```spl
index=failed_login_lab action=failure
| stats count by user
| sort - count
```

Purpose: Identify targeted accounts.

Visible top results:

| User | Failed Login Count |
|---|---:|
| administrator | 15 |
| admin | 9 |
| itadmin | 7 |
| vedant | 7 |
| backup | 6 |
| oracle | 6 |
| test | 6 |
| root | 5 |
| arjun | 4 |
| finance | 4 |
| priya | 4 |

---

## 5. Suspicious Source IPs Targeting Multiple Users

```spl
index=failed_login_lab action=failure
| stats count dc(user) as unique_users values(user) as targeted_users by src_ip
| where count > 20 OR unique_users > 5
| sort - count
```

Purpose: Detect suspicious authentication behavior where one source IP targets many accounts.

| Source IP | Failed Count | Unique Users | Targeted Users |
|---|---:|---:|---|
| 45.77.32.11 | 45 | 9 | admin, administrator, backup, devops, oracle, postgres, root, test, vedant |
| 185.220.101.23 | 30 | 6 | admin, administrator, finance, hr, itadmin, support |

---

## 6. Successful Login After Multiple Failures

```spl
index=failed_login_lab
| sort 0 _time
| streamstats count(eval(action="failure")) as previous_failures by src_ip
| where action="success" AND previous_failures>=20
| table _time src_ip user dest_host app action previous_failures reason severity country
```

Purpose: Identify a successful login that occurred after repeated failures from the same source IP.

| Time | Source IP | User | Destination | App | Previous Failures | Reason | Severity | Country |
|---|---|---|---|---|---:|---|---|---|
| 2026-06-26 11:50:00.450 | 45.77.32.11 | admin | vpn-gateway | vpn | 45 | valid_password_after_failures | critical | Netherlands |

---

## 7. Timeline of Failed Logins

```spl
index=failed_login_lab action=failure
| timechart span=10m count by src_ip
```

Purpose: Visualize failed login bursts by source IP over time.

---

## 8. Blocked Failed Login Events

```spl
index="failed_login_lab" action="failure" blocked
```

Purpose: Review blocked failed authentication events and inspect raw fields such as source IP, user, asset, application, reason, country, and severity.
