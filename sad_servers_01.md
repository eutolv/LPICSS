```markdown
# 🛠️ Post-Mortem: Saint John — What is writing to this log file? 📜

## 📝 Problem
A testing program was continuously writing entries to the log file: `/var/log/bad.log`. This caused the disk to keep filling up. 🚨 The objective was to **identify the process** writing to the log and **terminate it**, without deleting the log file. 🚫🗑️

---

## 🔍 Initial Verification
To confirm the issue, I monitored the log file in real time:

`tail -f /var/log/bad.log`

**Output example:**
```text
2026-03-10 17:08:52.097552 token: 1123181594
2026-03-10 17:08:52.398011 token: 1749693477
2026-03-10 17:08:52.698459 token: 460446815

```

The log was being written continuously every **~0.3 seconds**. ⏱️

---

## 🕵️ Process Investigation

First step was checking running processes:

`ps aux`

To narrow down possible sources, I searched for **Python** processes: 🐍

`ps aux | grep python`

### 🎯 Identifying the Source

The process corresponded to a Python script: `/home/admin/badlog.py`. This script was responsible for generating the continuous log entries.

---

## 🚀 Solution

Terminate the process responsible for writing to the log file:

`sudo kill -9 619`

After terminating the process, the log file stopped growing. ✅

**Verification:**
`/home/admin/agent/check.sh`

**Result:** `OK` 👍

---

## ⚠️ Important Note

The instructions explicitly stated: **Do not delete the log file.** Therefore, the correct solution was only terminating the process, not removing `/var/log/bad.log`.

---

## 💻 Commands Used

* `tail -f /var/log/bad.log` — Monitor logs in real time. 🧐
* `ps aux` — List all running processes.
* `ps aux | grep python` — Identify the specific script. 🔍
* `sudo kill -9 619` — Force termination of the PID. ⚡
* `/home/admin/agent/check.sh` — Run the validation script.

---

## 💡 Key Takeaways

* `tail -f` is essential for observing live behavior.
* `ps aux | grep <process>` quickly identifies runaway scripts.
* `kill -9` is the heavy hitter for stopping processes that won't close normally. 🔨
* **Read instructions carefully:** Saving the log file was a requirement!

**Estimated Time to Solve:** ~10 minutes ⏳

```

```
