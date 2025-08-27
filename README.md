# Endpoint Detection Tool (EDT)

🚨 **Hands-on SOC Project** – This lab simulates an Endpoint Detection & Telemetry (EDT) system designed to collect, analyze, and detect suspicious activities on endpoints.  
The goal is to build security monitoring skills, strengthen SOC workflows, and document the full detection process.

---

## 🔎 Project Overview
This project focuses on:
- Collecting endpoint events (process, file, network logs).
- Detecting suspicious or malicious activity using rules & signatures.
- Forwarding events to a SIEM for correlation and alerting.
- Documenting incident response steps as if working in a real SOC.

---

## 🛠️ Tools & Technologies
- Python (log collection & parsing)
- Sysmon (endpoint event logging)
- Splunk / ELK (event ingestion & detection)
- Windows test environment
- MITRE ATT&CK mapping

---

## 🎯 Learning Objectives
- Build a custom endpoint event detection workflow.
- Understand how raw logs can be transformed into SOC alerts.
- Practice detection engineering using realistic attacker simulations.
- Document SOC-style investigation steps.

---

## 📂 Repo Structure
- `scripts/` → Python scripts for parsing logs  
- `rules/` → Detection rules / queries  
- `docs/` → Step-by-step documentation with screenshots  
- `samples/` → Test attack logs  

---

## 🚀 Next Steps
1. Set up endpoint with Sysmon for logging.  
2. Collect and parse events using Python.  
3. Forward logs into SIEM for alerting.  
4. Test detection using simulated attacker behavior.  
5. Document results and findings.  

---

💡 *This is a learning-focused project designed to simulate SOC workflows. Not for production use.*  
