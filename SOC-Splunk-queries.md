# 🚨 1. Detect Multiple Failed Logins (Brute Force)

```
index=auth_logs action=failure
| stats count by user, src_ip
| where count > 5
| sort -count
```

**Meaning:**

Find users/IPs with more than 5 failed login attempts.

👉 **Use case:** Detect brute-force attacks

---

# 🌍 2. Suspicious IP Activity

```
index=network_logs
| stats count by src_ip
| sort -count
| head 10
```

**Meaning:**

Top 10 most active IP addresses.

👉 **Use case:** Identify unusual traffic sources

---

# ⏰ 3. Login Activity Over Time

```
index=auth_logs action=success
| timechart count by user
```

**Meaning:**

Shows login trends over time.

👉 **Use case:** Spot abnormal login spikes

---

# 🚫 4. Detect Access to Forbidden Resources

```
index=web_logs status=403
| stats count by src_ip, uri
```

**Meaning:**

Find who is trying to access restricted pages.

👉 **Use case:** Possible reconnaissance or attack attempts

---

# 📦 5. Large Data Transfer (Possible Exfiltration)

```
index=network_logs
| stats sum(bytes) as total_bytes by src_ip
| where total_bytes > 100000000
| sort -total_bytes
```

**Meaning:**

Find IPs sending unusually large data.

👉 **Use case:** Data exfiltration detection

---

# 👤 6. New or Rare Users

```
index=auth_logs
| rare user
```

**Meaning:**

Shows users that appear rarely.

👉 **Use case:** Detect suspicious or new accounts

---

# 🔁 7. Repeated Errors (System Issues or Attacks)

```
index=system_logs error
| stats count by host
| sort -count
```

**Meaning:**

Which systems are generating the most errors.

👉 **Use case:** Troubleshooting or attack detection

---

# 🧩 8. Extract IP from Raw Logs

```
index=logs
| rex "src_ip=(?<ip>\d+\.\d+\.\d+\.\d+)"
| stats count by ip
```

**Meaning:**

Pull IP address from raw log text.

👉 **Use case:** When fields aren’t pre-extracted

---

# 🔍 9. Detect Suspicious File Access

```
index=file_logs
| search filename="*.exe"
| stats count by user, filename
```

**Meaning:**

Track access to executable files.

👉 **Use case:** Malware investigation

---

# 🧠 10. Combine Login + Activity (Correlation)

```
index=auth_logs action=success
| join user [ search index=activity_logs ]
```

**Meaning:**

Match login events with user actions.

👉 **Use case:** Track what users did after login

---

# 🚨 11. Possible DDoS Attack

```
index=network_logs
| stats count by src_ip
| where count > 1000
```

**Meaning:**

IPs generating massive traffic.

👉 **Use case:** Detect flooding attacks

---

# 📊 12. Top Failed Login Locations

```
index=auth_logs action=failure
| stats count by src_ip
| sort -count
```

**Meaning:**

Where failed logins are coming from.

👉 **Use case:** Geo-based threat detection

---
