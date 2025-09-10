# Endpoint Detection & Response (EDR) with Wazuh Cloud

🚨 **Hands-on SOC Project** – In this lab, I deployed a cloud-hosted **Wazuh SIEM/EDR** to monitor a Kali Linux endpoint. The project simulates how SOC teams collect, analyze, and respond to suspicious activities on endpoints using **open-source tools** instead of expensive enterprise platforms.

---

## 🔎 Project Overview
This project demonstrates:
- Collecting endpoint telemetry (processes, file activity, network logs).
- Detecting brute-force, port scanning, and custom attack simulations.  
- Visualizing security alerts in the **Wazuh Dashboard**.  
- Mapping events to **MITRE ATT&CK techniques**.  
- Practicing real SOC-style workflows from **attacker → defender**.  

---

## 🛠️ Tools & Technologies
- **Kali Linux** → Endpoint & attack simulation  
- **Wazuh Cloud (Free Trial)** → Cloud-hosted SIEM/EDR platform  
- **Wazuh Agent** → Endpoint telemetry collection  
- **Nmap** → Reconnaissance & port scanning  
- **Hydra** → Brute-force attack simulation  
- **Custom Bash scripts** → Fake malware & persistence testing  
- **Zoho Mail + Custom Domain** → Business email setup for sign-up  

---

## 🎯 Learning Objectives
- Understand how SIEM/EDR platforms ingest and classify endpoint data.  
- Build detection use cases for **common attacker behaviors**.  
- Gain hands-on skills in **alert triage & SOC investigation workflows**.  
- Prove that **open-source tools** can deliver enterprise-grade security.  

---

## 🚀 Next Steps
1. Expand detection with **File Integrity Monitoring (FIM)**.  
2. Test **response actions** like disabling processes or quarantining files.  
3. Forward Wazuh alerts into **Splunk/ELK** for correlation practice.  
4. Add more advanced attack simulations (reverse shells, persistence).  

---

💡 *This project highlights practical SOC workflows and detection engineering using Wazuh Cloud. All steps are reproducible on a free trial setup.*  
