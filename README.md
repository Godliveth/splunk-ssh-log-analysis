# Splunk SSH Log Analysis Project
*Day 18 of #30DaysOfSOC Challenge*

## 📘 Overview
This project demonstrates a real-world SOC workflow in Splunk using a **synthetic** Zeek SSH dataset (no real-world or sensitive data was used).
The objective was to **detect failed and successful login attempts**, identify **potential brute-force activity**, and demonstrate core skills in **log analysis, SPL querying, and threat detection**.

---

## 🎯 Project Objectives
- Ingest and parse JSON-formatted **Zeek-style SSH logs** into Splunk.  
- Build SPL queries to detect **failed, successful, and suspicious SSH events**.  
- Identify potential **malicious login attempts** or **brute-force indicators**.  
- Apply SOC analytical thinking to interpret security log data.

---

## Analyst Context
SSH authentication logs are critical for  monitoring remote access to servers.  
Repeated failed attempts, login bursts, or access from unusual IPs may indicate brute-force or unauthorized activity.  
This project reproduces that scenario to simulate how a SOC analyst investigates SSH-based intrusion attempts.

---

## 🖥️ Lab Environment

| Component            | Description 
|----------------------|-----------------------------------------------------------------------------------
| **SIEM Platform**    | Splunk Enterprise 
| **Data Source**      | Synthetic Zeek SSH logs (`synthetic_zeek_ssh.json`) 
| **Data Format**      | JSON 
| **Index**            | `ssh_lab` 
| **Sourcetype**       | zeek:ssh (custom sourcetype created to ingest and parse Zeek-style SSH JSON logs) 
| **Operating System** | Windows 10 (Host Machine) 

---

## ⚙️ Data Ingestion Workflow
1. Accessed **Splunk Web Interface**.  
2. Navigated to **Settings → Add Data → Upload**.  
3. Uploaded the file `synthetic_zeek_ssh.json`.  
4. Defined the following parameters:
   - **Source Type:** `json` (custom alias: `zeek:ssh`)
   - **Index:** `ssh_lab`
5. Verified successful indexing with a preliminary SPL query:
   ```spl
   index=ssh_lab | stats count
   ```

## 📂 Data Source
The dataset used for this project is a **synthetic Zeek SSH log** file created for educational purposes.  
It simulates authentication activity for SOC analysis and contains **no real-world or sensitive data**.

📄 [Download synthetic_zeek_ssh.json](./synthetic_zeek_ssh.json)

---

## 🔍 Log Analysis and SPL Queries

### 1️⃣ Identify Top 10 Endpoints with Failed SSH Logins
```spl
index=ssh_lab sourcetype="json" auth_success=false
| stats count by "id.orig_h"
| sort -count
| head 10
```
**Purpose:** Detect IP addresses generating the highest number of failed SSH attempts — a typical sign of brute-force or password spraying attacks.  
**Insight:** The output revealed multiple failed attempts from specific IPs, indicating possible attack sources.

---

### 2️⃣ Calculate Total SSH Connections
```spl
index=ssh_lab sourcetype="json"
| stats count as total_ssh_connections
```
**Purpose:** Establish baseline SSH activity volume for the dataset.  
**Insight:** Helps determine whether abnormal spikes in connection volume correspond with brute-force attempts.

---

### 3️⃣ Categorize Event Types
```spl
index=ssh_lab sourcetype="json"
| stats count by event_type
```
**Purpose:** Break down authentication outcomes (e.g., success, failed, no-auth, multiple-failed).  
**Insight:** Provides a quick snapshot of overall authentication health and event distribution.

---

## 📊 Key Findings
- Several IPs recorded **multiple failed SSH attempts** within a short timeframe — strong indicators of **brute-force attempts**.  
- Very few **successful authentications**, suggesting **limited legitimate access**.  
- No anomalous successful logins from suspicious IPs — implying attack attempts did not succeed.  
- The SPL queries enabled rapid **detection and categorization of login behaviors** across the dataset.

---

## 🧩 SOC Skills Demonstrated
| Skill Category               | Description 
|------------------------------|----------------------------------------------------------------------
| **SIEM Operations**          | Data ingestion, parsing, and index management in Splunk. 
| **SPL Proficiency**          | Use of `stats`, `sort`, and filtering operators for data analysis. 
| **Threat Detection Logic**   | Identification of brute-force patterns and malicious login behaviors. 
| **Analytical Reasoning**     | Interpretation of SSH log data to infer attack intent and activity. 
| **Security Monitoring**      | Applying real-world SOC procedures to authentication logs. 

---

## 🧾 Sample Output Screenshot
All screenshots from the analysis can be viewed in the 📸 screenshots/folder
![View all screenshots here](./screenshots)
---

## 🚀 Impact & Relevance
This project demonstrates the **core competencies of a Tier 1 SOC Analyst**, including:
- Log ingestion and parsing in a SIEM.  
- Writing SPL queries for event detection.  
- Investigating potential security incidents.  
- Communicating findings effectively through documentation.

Such analytical exercises reflect readiness to operate in a live SOC environment — where analysts are expected to monitor authentication patterns, detect intrusion attempts, and provide actionable intelligence.

---

## 📁 Repository Contents
| File                         | Description 
|-------------------------------------------------------------------------
| `synthetic_zeek_ssh.json`    | SSH log data used for ingestion
| `screenshots`                | Screenshot of SPL results 
| `README.md`                  | Project documentation

---

## 👨‍💻 Author
**Godliveth Madu**  
_Security Operations Center (SOC) Trainee  
🔗 [LinkedIn](https://www.linkedin.com/in/godlivethmadu)  


---
