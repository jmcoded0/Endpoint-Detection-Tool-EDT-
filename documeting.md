# Endpoint Detection & Response (EDR) with Wazuh Cloud

**Project by Johnson (jmcoded)**  

This project documents my journey from a frustrating starting point to successfully deploying a live Endpoint Detection & Response (EDR) environment. The goal was to set up a **Wazuh SIEM** to monitor a Kali Linux machine and to gain hands-on experience with a real-world security tool.

---

## 🚧 The Challenge
For weeks, I was stuck trying to set up a SIEM. My attempts to host Wazuh on an **AWS cloud server** were repeatedly blocked by billing issues, and every solution I tried with a regular bank card failed.  

I almost gave up — but I learned that in cybersecurity, **persistence is everything**.  
My goal was clear: *get a SIEM working, no matter the obstacles*.

---

## 1️⃣ Choosing the Right Path: Wazuh Cloud
My breakthrough came when I decided to shift my approach. Instead of self-hosting, I opted for the **Wazuh Cloud free trial**.  

This decision was a game-changer because it eliminated all the issues with cloud hosting and credit card payments. The trial provided a **fully managed SIEM environment** with zero cost.  

✅ *My dashboard view after my environment was provisioned and ready.*
<img width="1024" height="576" alt="image" src="https://github.com/user-attachments/assets/5dd74176-b5db-4d56-9d4d-c135300527b0" />

---

## 2️⃣ Overcoming the Business Email Hurdle
Just when I thought I had a clear path, I hit another wall: the **Wazuh Cloud sign-up required a business email address**.  

For a student like me, this was a major roadblock. But instead of quitting, I got creative:  
- I used my personal website (`jmcoded.site`)  
- Integrated it with **Zoho Mail** to create a **legitimate business email**  

This was the key that unlocked the sign-up process.  

✅ *Creating a business email to bypass the sign-up requirement.*
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2ffd817a-0550-4fe7-8710-801ba7b3a84a" />

---

## 3️⃣ SIEM Environment Setup & Agent Deployment
With my new business email, the sign-up process was smooth. Wazuh provisioned my dedicated SIEM environment in just a few minutes.  

A small challenge remained — the credentials email didn’t arrive. I solved this by resetting the password, and boom 💥 — I was finally in!  

Next step: **Deploy the Wazuh Agent** on my Kali Linux machine.  
<img width="1024" height="576" alt="image" src="https://github.com/user-attachments/assets/e2de1822-6ead-4deb-83f2-5e53893961b3" />

### 🔹 Agent Installation Procedure
On the Wazuh dashboard:  
1. Selected the **Linux agent (DEB amd64)** for Kali Linux  
2. Copied the generated one-line install command  
3. Ran it on my Kali VM  

The agent installed successfully, but I had to start the service manually.
<img width="1278" height="798" alt="VirtualBox_Kali Linux_09_09_2025_03_38_11" src="https://github.com/user-attachments/assets/066d24c1-f304-46b0-a4c6-6e0a1dc0f18d" />

#### Commands Used  
```bash
# Start the agent service
sudo systemctl start wazuh-agent

# Verify it's running
sudo systemctl status wazuh-agent
```
---

## 4️⃣ The Final Result: A Live, Connected SIEM
After starting the service, I returned to the Wazuh dashboard.  

After a few minutes, the moment I had been waiting for arrived:  
👉 My **Kali Linux machine appeared as an active agent**, changing from *“Never connected”* to *“Active.”*  

The months of frustration, late nights, AWS billing issues, and creative problem-solving all led to this victory.  

✅ *The dashboard showing one active agent: my Kali Linux machine.*
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/fde80edc-281e-410c-861b-c704f08d3b14" />

---

## 5️⃣ Next Steps & Learnings
Now that the SIEM is live, I can move to the **core security work**:  

- 🔍 **Log Analysis:** Generate events on Kali and analyze them in Wazuh  
- 🛡 **Threat Simulation:** Simulate attacks (e.g., failed SSH logins) to test detections  
- ⚙️ **Rule Creation:** Write custom rules to monitor specific activities  

---

## ✨ Key Takeaways
This project taught me:  
- Cybersecurity isn’t about having all the answers — it’s about **persistence**  
- Solving problems across **cloud, DNS, and system levels** is what makes you grow  
- Every small victory builds confidence for the bigger challenges ahead  

🚀 *From frustration to a live SIEM — this project is proof that persistence pays off.*  

# Attacker Simulation Walkthrough

After finishing the setup, I didn’t want the lab to stay too clean — I needed to simulate a real attacker messing around the system.  
I started dropping noise and testing multiple attack surfaces on the Meta machine from my Kali box.

---

## Recon – Scanning the Target

I kicked things off with an **Nmap stealth scan** to see what doors were open:

```bash
nmap -sS -T4 192.168.117.3
```

The output came back loud. I wasn’t dealing with a tiny lab VM — this thing was running everything under the sun:

- FTP, SSH, Telnet  
- HTTP, RPC, SMB  
- MySQL, VNC, X11  
- Even old-school ports like ingreslock and RMI  
<img width="1278" height="798" alt="VirtualBox_Kali Linux_09_09_2025_06_00_31" src="https://github.com/user-attachments/assets/86da359f-562a-4b92-a97c-6b8a75aa7aab" />

Basically, a perfect playground for attacks.

---

## Brute-Force Attempt on SSH

Next thing I tried was a **Hydra brute-force** on the SSH service:

```bash
hydra -l 192.168.117.3 -P /usr/share/wordlists/rockyou.txt ssh://192.168.117.3
```
<img width="1278" height="798" alt="VirtualBox_Kali Linux_09_09_2025_06_02_14" src="https://github.com/user-attachments/assets/5d31a406-b549-4e59-b322-13bc0e05381c" />

Hydra fired up fine, but then hit me with an error about **key exchange algorithms**.  
The server was only accepting old `ssh-rsa` / `ssh-dss`, while Hydra was trying newer algos.  

That’s real-world noise right there — attackers will often try brute-force and fail because of outdated or mismatched crypto. I kept it as-is because it still simulates attacker traffic hitting SSH.

---

## Dumping Shadow File (Privilege Abuse Simulation)

To make the attack more realistic, I went straight for the **juicy stuff** — the `/etc/shadow` file.  

```bash
sudo cat /etc/shadow
```
<img width="1278" height="798" alt="VirtualBox_Kali Linux_09_09_2025_06_04_27" src="https://github.com/user-attachments/assets/b5b9087f-1d03-4ffb-b2a2-a5a06417cacf" />

And boom, I dumped all the user hashes.  
Scrolling down I even spotted my own user `jmcoded` with its hash clearly visible.  

This represents what an attacker would aim for after privilege escalation: stealing password hashes for offline cracking.

---

## Dropping a Fake Malware Script

Finally, I wanted to simulate a **malware dropper / persistence**.  
So I planted a simple script in `/bin`:

```bash
echo "MALWARE_TEST" | sudo tee /bin/badscript.sh
sudo chmod +x /bin/badscript.sh
```
<img width="1278" height="798" alt="VirtualBox_Kali Linux_09_09_2025_06_05_27" src="https://github.com/user-attachments/assets/666988bc-2d95-4df2-8ab0-7bf85f641c10" />

Now there’s a fake “badscript” binary sitting in the system, marked executable.  
It doesn’t do anything destructive — just prints `MALWARE_TEST` — but in a real-world scenario this is where an attacker could drop ransomware or a backdoor.

---

## Why These Steps Matter

I didn’t just want a clean, theoretical lab — I wanted **noise that looks like a real attacker**:

- Recon traffic from Nmap  
- Brute-force attempts on SSH  
- Privilege abuse with `/etc/shadow`  
- Malware persistence via `/bin/badscript.sh`  

All of these leave behind logs, alerts, and traces that my SOC tools (Splunk, Wazuh, Zeek, etc.) can pick up in the next phase.  
That’s the whole point — building a messy environment that defenders actually deal with.
---

# The Defender’s View: Wazuh’s Real-Time Intelligence

After throwing all that attacker noise at the system, I switched hats — from attacker to defender.  
Now it was time to see if my SIEM (Wazuh) could actually pick up the chaos and make sense of it. Spoiler: it didn’t disappoint.

---

## 1. Triage: The Alerts Dashboard

The first stop was the **main dashboard** — and it was already lit.  
This wasn’t some quiet, sterile lab. It looked alive, like a real SOC environment.

- **Total Alerts:** Over 200 alerts triggered in just a few minutes.  
- **Severity Breakdown:** 107 tagged as *Medium* and 108 as *Low*.  

Right away, Wazuh proved it’s not just a dumb logger. It was *scoring, ranking, and prioritizing* activity in real time.

![Wazuh Dashboard Overview]<img width="1024" height="576" alt="image" src="https://github.com/user-attachments/assets/8fbd8a46-ca1a-42e2-91d5-9446cb0a6b16" />
---

## 2. Deep Dive: Agent Metrics

Next, I zoomed in on the **agent running on my Kali machine**.  
This wasn’t just a log forwarder — it acted like a mini-EDR.

- **Vulnerability Detection:** It scanned installed software and flagged known CVEs, giving me proactive defense data.  
- **MITRE ATT&CK Mapping:** Events were mapped to ATT&CK tactics, showing exactly how actions lined up with real-world adversary playbooks.  

That’s pro-level visibility, and it’s built right in.

![Wazuh Agent Metrics]<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2139f261-e070-4d3a-86f7-3da83081ceb4" />

---

## 3. Live Detection: `/etc/shadow` Access

One of my biggest tests was whether Wazuh would catch me reading `/etc/shadow`.  
That’s a **red-flag attacker move** every SOC analyst looks for.

And it did:

- **Rule Level:** 7 (high-severity anomaly).  
- **Description:** “Host-based anomaly detection event” — simple, clear, accurate.  

This wasn’t noise. It was exactly the kind of crisp alert a SOC analyst wants.

![Wazuh Anomaly Detection]<img width="1024" height="576" alt="image" src="https://github.com/user-attachments/assets/e48c4d2c-f194-43a7-8a5a-da9d5bc7b006" />

---

## 4. File Integrity & Malware Detection

Last move: I dropped my fake malware script (`/bin/badscript.sh`) into a critical directory.  
If Wazuh missed this, the whole setup would be questionable.  

But it didn’t miss.  
The **File Integrity Monitoring (FIM) module** immediately flagged the change as suspicious — exactly how attackers are caught trying to persist in systems.

![Wazuh Malware Detection]<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/0319c2bb-5c96-4f87-87ee-ba9d9ed83e52" />


---

## Conclusion: A Professional-Grade Lab

The results spoke for themselves:

- Every attacker move I made was **detected, scored, and alerted** in real time.  
- Vulnerabilities and tactics were mapped to industry standards (MITRE ATT&CK).  
- File drops and shadow file access didn’t slip through the cracks.  

I built this with **free, open-source tools**, yet the intelligence and visibility rival what multi-million-dollar enterprise SIEMs deliver.  

This lab proves that with the right setup and mindset, you can train like a real SOC analyst — without spending a fortune.  
It’s not just a lab, it’s a **battlefield I created and defended**

