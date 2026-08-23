# Day 9: MITRE ATT&CK Framework & Log Correlation

## Core Framework Architecture & Terminology
* **What is MITRE ATT&CK?:** A globally accessible, comprehensive knowledge base and structured vocabulary of real-world threat actor behaviors, tactics, and techniques compiled from actual incident observations. ***https://attack.mitre.org***
* **Tactics vs. Techniques vs. Procedures (TTPs):**
  * **Tactics:** The attacker's immediate operational goal or objective (the *What* and *Why*, e.g., Initial Access, Discovery, Credential Access).
  * **Techniques:** The specific technical method or mechanism the threat actor leverages to achieve that operational goal (the *How*, e.g., Brute Force, Scheduled Task).
  * **Procedures:** The exact contextual, platform-specific implementation or execution string used during an active intrusion sequence (e.g., executing an explicit script or specific terminal command line).
* **Analysis:** Log correlation allows analysts to differentiate between a *Detected Attack* (where threat behavior generates real telemetry flags in our central logging matrix) and an *Undetected Attack* (where gaps in log enforcement or misconfigured endpoints allow a malicious technique to execute completely unrecorded).

---

## Activity 1: MITRE ATT&CK TTP Mapping Matrix

These real-world attacker threat behaviors map cleanly to official database categories on [attack.mitre.org](https://mitre.org):

| Attacker Behavior Profile | Identified Technique Name | Technique ID | Parent Tactic Category |
| :--- | :--- | :--- | :--- |
| **Behavior 1:** An employee receives an email with an attachment disguised as an invoice. Opening it secretly installs malware. | **Phishing: Spearphishing Attachment** | `T1566.001` | **Initial Access** |
| **Behavior 2:** After gaining access, an attacker creates a new scheduled task on the computer that automatically restarts their malware every time the computer reboots. | **Scheduled Task/Job: Scheduled Task** | `T1053.005` | **Persistence** |
| **Behavior 3:** An attacker who has already compromised one computer uses stolen credentials to log into 5 other computers on the same network over the next hour. | **Lateral Movement: Remote Services** | `T1021` | **Lateral Movement** |

---

## Activity 2: Blue Team Challenge — The Hammered Lab

Forensic log triage and correlation executed against an abstracted Linux system honeypot workspace to map a live compromise vector.

### 📁 Technical Setup & Verification Checkpoints
* **Evidence Ingestion:** Downloaded and extracted the standard forensic archive container using password parameters (`cyberdefenders.org`) to bypass host-level automated anti-virus containment engines.
* **Operational Constraint Note:** On macOS platforms, opening password-protected compressed archives natively can drop threads silently due to OS volume handling design. The engineering remediation requires explicit extraction via terminal tools or third-party utility applications.
* **File Scoping Strategy:** The extracted forensic target directory presents multiple platform indicators. Large multi-megabyte log files like `auth.log` must be handled inside robust, developer-grade paging engines (e.g., Visual Studio Code or Notepad++) to prevent buffer crashes or memory locks. Small package logging units (`dpkg.log`) can be safely evaluated inside baseline text parsers.

### 🔬 Forensic Log Extraction Evidence & Proofs

#### Step 1: Identifying Password Guessing Waveforms
* **Target File Layer:** `auth.log`
* **Target Search String:** `Invalid user`
* **Forensic Observation:** Searching this keyword exposes thousands of rapid, consecutive authentication logs generated only 2 to 4 seconds apart. This structural velocity signature confirms an active **Password Guessing / Credential Brute-Force** reconnaissance vector hitting the system boundary.

#### Step 2: Pinpointing the Compromise Breach Vector
* **Target File Layer:** `auth.log`
* **Target Search String:** `Accepted password for root`
* **Forensic Observation:** The initial successful validation match records an attacker pivoting onto the system. The first event occurs on **Apr 19 05:41:44** originating from external threat infrastructure asset **`219.150.161.20`** over SSH. 
* **Timeline Ordering:** Comparing timestamps proves that the password-guessing campaign started **before** the successful validation, establishing a direct cause-and-effect exploit chain.

#### Step 3: Mapping Post-Exploitation Actions & Defensive Evasion
* **Target File Layer:** `auth.log`
* **Target Search String:** `iptables`
* **Forensic Observation:** Telemetry logs track the compromised `root` account running immediate localized system audits to evaluate system policies and host network protections.
* **Target File Layer:** `dpkg.log`
* **Target Search String:** `install nmap`
* **Forensic Observation:** System records log a package state transition on **2010-04-24 19:38:15** validating the successful installation of the **nmap** scanning toolkit onto the honeypot node.

---

### 📋 Chronological Incident Narrative (Managerial Summary)

Between April 19 and April 24, an enterprise server system sustained a multi-stage compromise orchestrated by external attackers. Initially, the perimeter log layers capture thousands of high-frequency connection attempts executing a rapid password-guessing campaign targeting core administrative accounts. On April 19 at 05:41:44 AM, this automated attack successfully cracked the master system access credentials, enabling attackers to log directly into the high-privilege `root` account. 

Once inside the endpoint perimeter, the threat actors executed local system queries to discover the machine's current firewall protection matrix and system security rules. Finally, on April 24 at 07:38:15 PM, the attackers bypassed administrative guardrails to install an active network utility tool named `nmap`, effectively staging the machine to launch subsequent lateral scans against surrounding corporate sister networks.





Installing Sysmon
<img width="979" height="622" alt="image" src="https://github.com/user-attachments/assets/c2d7e58a-2794-4322-877b-8954cf73522f" />


<img width="961" height="566" alt="image" src="https://github.com/user-attachments/assets/0f44f22a-b6b7-4d5c-90fa-a5d89cf31794" />
<img width="962" height="562" alt="image" src="https://github.com/user-attachments/assets/96e9e1b5-37c9-4e68-8afa-35ac8c310d33" />

<img width="966" height="567" alt="image" src="https://github.com/user-attachments/assets/d7f01765-b22d-40e1-81f3-33716065eba6" />
<img width="547" height="690" alt="image" src="https://github.com/user-attachments/assets/99c47ea0-85e1-4cde-af01-a369d38df092" />

<img width="1365" height="719" alt="image" src="https://github.com/user-attachments/assets/6451571e-8b30-40ff-90aa-c70a006baaf7" />

<img width="1362" height="720" alt="image" src="https://github.com/user-attachments/assets/baad6995-ac51-4ac6-a485-ea86a2fb5bf9" />

<img width="759" height="509" alt="image" src="https://github.com/user-attachments/assets/0b0eacd5-f68a-4180-aa18-b689113313cb" />


## Additional Materials
* LetsDefend - Log analysis With Sysmon (Not started)
