# 🧠 Failed Logon Analysis in Splunk

## 📁 1. Log Generation (Windows)
- Manually generated **failed logons** on my Windows system.  
- Verified events in **Event Viewer → Windows Logs → Security**.  
- Filtered by **Event ID 4625 (Failed Logon Attempts)**.  
- Exported logs as `.evtx` file (e.g., `failed_logons_4625.evtx`).

---

## 🧩 2. Import into Splunk
- Uploaded `.evtx` file via **Add Data → Upload**.  
- **Sourcetype:** `WinEventLog:Security`  
- Data indexed into the **default index**.

---

## 🔍 3. Search Queries Used

### Base Search – All Failed Logons
```spl
sourcetype="WinEventLog:Security" EventCode=4625

Trend Over Time (Line Chart)
sourcetype="WinEventLog:Security" EventCode=4625
| timechart count

Failed Logons by Failure Reason
sourcetype="WinEventLog:Security" EventCode=4625
| stats count by Failure_Reason

Failed Logons by User
sourcetype="WinEventLog:Security" EventCode=4625
| stats count by Account_Name


Dashboard Panels
Panel 1 – Timechart	Trend of failed logon attempts → Sharp spike Wed–Fri.
Panel 2 – Bar/Table	Failed logons grouped by Failure_Reason (single bar since all logs had one reason).
Panel 3 – Bar Chart	Failed logons grouped by Account_Name.

Observations
Total failed logons: 29
All attempts originated from the same host (my machine).
Same failure reason repeated → single bar in “Failure Reason” panel.

Key Takeaways
Event ID 4625 is critical for detecting failed login attempts in Windows systems.
Splunk’s sourcetype WinEventLog:Security provides easy filtering and visualization.
Repeated failed logons may indicate brute force or misconfigured account.


Author: ENWEREJI OBIOMACHI
Tool: Splunk Enterprise
Category: SOC Analyst Lab – Windows Event Analysis
