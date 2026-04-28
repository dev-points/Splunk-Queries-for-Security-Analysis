# 🚨 1. Detect Multiple Failed Logins (Brute Force)

```
index=auth_logs action=failure
| stats count by user, src_ip
| where count > 5
| sort -count
```


👉 **Use case:** Detect brute-force attacks of more than 5 failed login attempts

---

# 🌍 2. Suspicious IP Activity

```
index=network_logs
| stats count by src_ip
| sort -count
| head 10
```


👉 **Use case:** Identify Top 10 most unusual traffic sources

---

# ⏰ 3. Login Activity Over Time

```
index=auth_logs action=success
| timechart count by user
```


👉 **Use case:** Spot abnormal login spikes over time.

---

# 🚫 4. Detect Access to Forbidden Resources

```
index=web_logs status=403
| stats count by src_ip, uri
```


👉 **Use case:** Possible reconnaissance or attack attempts on restricted pages

---

# 📦 5. Large Data Transfer - Possible Exfiltration

```
index=network_logs
| stats sum(bytes) as total_bytes by src_ip
| where total_bytes > 100000000
| sort -total_bytes
```


👉 **Use case:** Find IPs sending unusually large data.

---

# 👤 6. New or Rare Users

```
index=auth_logs
| rare user
```


👉 **Use case:** Detect suspicious or new accounts or users that appear rarely.

---

# 🔁 7. Repeated Errors (System Issues or Attacks)

```
index=system_logs error
| stats count by host
| sort -count
```


👉 **Use case:** Which systems are generating the most errors.

---

# 🧩 8. Extract IP from Raw Logs

```
index=logs
| rex "src_ip=(?<ip>\d+\.\d+\.\d+\.\d+)"
| stats count by ip
```

**Meaning:**

Pull IP address from raw log text.

👉 **Use case:** Pull IP address from raw log text.

---

# 🔍 9. Detect Suspicious File Access

```
index=file_logs
| search filename="*.exe"
| stats count by user, filename
```


👉 **Use case:** Track access to executable files like Malware.

---

# 🧠 10. Combine Login + Activity 

```
index=auth_logs action=success
| join user [ search index=activity_logs ]
```

**Meaning:**

Match login events with user actions.

👉 **Use case:** Match login events and track what users did after login

---

# 🚨 11. Possible DDoS Attack

```
index=network_logs
| stats count by src_ip
| where count > 1000
```

**Meaning:**

IPs generating massive traffic.

👉 **Use case:** Detect IPs generating massive traffic attacks.

---

# 📊 12. Top Failed Login Locations

```
index=auth_logs action=failure
| stats count by src_ip
| sort -count
```

**Meaning:**

Where failed logins are coming from.

👉 **Use case:** Where failed logins are coming from.

---
