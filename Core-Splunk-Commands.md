Trying to list all Splunk commands and every possible combination would turn into a small book and still be incomplete. Splunk has 60+ commands, and combinations are practically unlimited because you can chain them in different ways.
What actually helps is understanding.
1. All major commands.
2. How they combine in patterns.


# 🔹 Core Splunk Commands (Grouped + Meaning)

## 🔍 Searching & Filtering

- `search` – Find events matching criteria
- `where` – Filter with conditions (like SQL)
- `regex` – Filter using regular expressions

---

## 📊 Transforming (Aggregation)

- `stats` – Aggregate values (count, avg, sum)
- `chart` – Create table-style charts
- `timechart` – Time-based aggregation
- `top` – Most frequent values
- `rare` – Least frequent values

---

## 🧱 Field & Data Handling

- `fields` – Include/exclude fields
- `table` – Display selected fields
- `rename` – Rename fields
- `eval` – Create/modify fields
- `fillnull` – Replace null values

---

## 🔄 Data Processing

- `sort` – Sort results
- `dedup` – Remove duplicates
- `head` / `tail` – Limit results
- `reverse` – Reverse order

---

## 🔗 Correlation

- `join` – Combine datasets
- `append` – Add search results
- `appendcols` – Add columns
- `lookup` – Enrich with external data

---

## 📦 Extraction & Parsing

- `rex` – Extract with regex
- `spath` – Extract JSON/XML
- `kv` – Extract key-value pairs

---

## 📅 Time Handling

- `bin` / `bucket` – Group time ranges

---

## 🚨 Alerts & Output

- `alert` – Trigger alert
- `sendemail` – Send email
- `outputlookup` – Save to lookup file

---

## ⚙️ Utility

- `metadata` – Show index info
- `eventcount` – Fast event count
- `history` – Show past searches

---

# 🔥 Common Command Combinations (VERY Important)

This is where real power comes in.

---

## ✅ 1. Basic Filtering + Table

```
index=web_logs status=200
| table _time, host, status
```

➡️ Filter → display clean output

---

## ✅ 2. Search + Stats (Most Common)

```
index=web_logs
| stats count by status
```

➡️ Count events grouped by status

---

## ✅ 3. Search + Timechart (Monitoring)

```
index=web_logs
| timechart count by status
```

➡️ Trends over time

---

## ✅ 4. Eval + Stats (Calculated Insights)

```
index=web_logs
| eval kb=bytes/1024
| stats avg(kb)
```

➡️ Calculate then analyze

---

## ✅ 5. Where + Filtering Logic

```
index=web_logs
| where bytes > 1000
```

➡️ Advanced filtering

---

## ✅ 6. Rex + Extraction

```
index=logs
| rex "user=(?<username>\w+)"
```

➡️ Extract username from raw logs

---

## ✅ 7. Dedup + Sort

```
index=logs
| sort -_time
| dedup user
```

➡️ Latest unique users

---

## ✅ 8. Lookup Enrichment

```
index=logs
| lookup users.csv user OUTPUT role
```

➡️ Add extra data

---

## ✅ 9. Join (Correlation)

```
index=web_logs
| join user [ search index=auth_logs ]
```

➡️ Combine login + activity data

---

## ✅ 10. Full SOC Example 🚨

```
index=security_logs error
| rex "src_ip=(?<ip>\d+\.\d+\.\d+\.\d+)"
| stats count by ip
| where count > 10
| sort -count
```

➡️ Detect suspicious IP activity

---

# 🧠 Key Idea (Very Important)

Splunk works like a **pipeline**:

```
Search → Filter → Extract → Transform → Visualize
```

Example flow:

```
index=logs
| search error
| rex ...
| stats ...
| sort ...
```

---

# ⚠️ Reality Check

- You don’t memorize *everything*
- You master **patterns + logic**
- 80% of real work uses:
    - `search`
    - `stats`
    - `timechart`
    - `eval`
    - `where`
    - `rex`
