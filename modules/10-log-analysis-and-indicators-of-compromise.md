# Day 10: Log Analysis & Indicators of Compromise

## Log Analysis Fundamentals
* **Log Definition:** A chronological, written record of system, network, or application events captured automatically with precise timestamps. It functions like an automated digital sign-in sheet for an operating environment.
* **Core Purpose:** Threat hunters analyze behavioral logs to find anomalies, track hacker intrusions, and uncover bypass mechanisms designed to breach network assets.

---

## 4️⃣ The Four Primary Corporate Log Sources

Security investigations require parsing multiple telemetry feeds to reconstruct an incident cleanly:

| Log Category | Core Recorded Parameters | Practical Investigative Use Case | Structural Data Line Example |
| :--- | :--- | :--- | :--- |
| **Auth Log** | Tracks access control assertions, tracking failed or successful login attempts, password updates, and privilege changes. | The primary triage engine for auditing brute-force vectors or compromised user credentials. | `Failed password for user admin from 41.2.3.10, 02:47 AM` |
| **System Log** | Captures kernel and OS events, service parameters, run states, application errors, and unexpected crashes. | Ideal for spotting unauthorized automated script installations or system services starting up unexpectedly. | `Service Update-Agent started successfully, 09:00 AM` |
| **Web Server Log** | Records every connection request delivered to a web application, exposing client source IPs, resource URLs, and response statuses. | Used heavily to audit directories for active vulnerability scanning or remote code exploitation attempts. | `GET /login.php from 102.44.5.6, response 200 (success)` |
| **Firewall Log** | Logs perimeter network boundary connections, tracking allowed blocks and dropped packet actions. | Critical for mapping out blocked attack vectors or identifying anomalous unauthorized outbound data connections. | `BLOCKED, inbound connection attempt on port 23 from 185.9.2.1` |

---

## OS Storage Directory & Access Quick-Reference

When performing local digital forensics on an endpoint rather than using a centralized SIEM dashboard [07-siem-concepts-and-alert-triage.md], log paths and access methods change based on the underlying Operating System.

### 1. Microsoft Windows Environments
Windows systems compile logs into structured binary formats (`.evtx`). These cannot be viewed with standard text editors and must be accessed via native diagnostic software.
* **Primary Desktop Utility:** Press `Win + R`, type **`eventvwr.msc`**, and press Enter to launch the **Windows Event Viewer**.
* **Core Event Paths:**
  * **Auth Logs:** Located under `Windows Logs ➔ Security`. (Physical file: `C:\Windows\System32\Winevt\Logs\Security.evtx`).
  * **System Logs:** Located under `Windows Logs ➔ System`. (Physical file: `C:\Windows\System32\Winevt\Logs\System.evtx`).
  * **Web Server Logs (IIS):** Default storage path is located at `C:\inetpub\logs\LogFiles\`.
  * **Firewall Logs:** Disabled by default. When enabled via the Windows Defender Firewall console, the plain text ledger is written directly to `C:\Windows\System32\LogFiles\Firewall\pfirewall.log`.

### 2. Linux Distributions (Ubuntu, Debian, Red Hat)
Linux systems utilize plaintext structures nested inside a central logging container. They are accessed using terminal paging controls such as `cat`, `less`, `tail -f`, or `grep` using administrative root privileges.
* **Core Diagnostic Path:** **`/var/log/`**
* **Core Event Paths:**
  * **Auth Logs:** 
    * *Ubuntu/Debian:* `/var/log/auth.log`
    * *RHEL/CentOS/Fedora:* `/var/log/secure`
  * **System Logs:** 
    * *Ubuntu/Debian:* `/var/log/syslog`
    * *RHEL/CentOS/Fedora:* `/var/log/messages`
  * **Web Server Logs (Apache / Nginx):** 
    * *Apache:* `/var/log/apache2/access.log`
    * *Nginx:* `/var/log/nginx/access.log`
  * **Firewall Logs (iptables / ufw):** System boundary events are directly injected straight into the core kernel log path at `/var/log/kern.log`.

### 3. Apple macOS Environments
macOS layers a Unix filesystem foundation beneath an Apple Unified Logging engine dashboard.
* **Primary Desktop Utility:** Launch the native **`Console.app`** application workspace from your Applications folder.
* **Core Event Paths:**
  * **Auth & Session Logs:** Modern macOS registers secure identity and cryptographic token mappings directly inside its unified system telemetry stream. You can audit active authorization events using the terminal command: `log show --predicate 'process == "authorizationhost"'`.
  * **System Logs:** Classical operating logs reside at `/var/log/system.log`. Modern diagnostics are actively queried in the terminal via the unified stream manager tool: `log show`.
  * **Web Server Logs:** If running localized open-source deployments, files default to `/var/log/apache2/access_log`.
  * **Firewall Logs (Application Firewall / socketfilterfw):** Security state records write out directly onto `/var/log/appfirewall.log`.

---

## 🛑 Defining Indicators of Compromise (IoCs)

An Indicator of Compromise is an artifact or behavior captured in log data that gives a strong clue that a system has been breached. It acts like smoke escaping from an active fire zone.

### 1. Command & Control (C2) Beaconing
* **The Mechanics:** Occurs when hidden malware quietly residing on an infected endpoint periodically phones home to an external malicious server to download instructions or leak assets.
* **The Telltale Indicator:** Extreme regularity. Connections consistently repeat at precise intervals (e.g., exactly every 60 seconds) to unfamiliar destination nodes.
* **Operational Challenge:** Individual beaconing packets look small and harmless, meaning they can persist undetected for years if data trends are not reviewed over a long timeline.

### 2. DNS Tunneling
* **The Mechanics:** Firewalls rarely block Domain Name System traffic because it is mandatory for normal web browsing. Threat actors abuse this by packing tiny pieces of stolen data inside subdomains to smuggle data past outer perimeters.
* **The Telltale Indicator:** Massive bursts of lookup queries directed at a single external domain containing unusually long, randomized alphanumeric subdomains.
* **Example:** `://attacker-domain.com` (Data encoded directly into the lookup parameter).

### 3. Lateral Movement
* **The Mechanics:** Once an attacker compromises an initial low-value machine, they immediately look to pivot inside the environment to escalate access toward high-value corporate nodes (like domain controllers or financial servers).
* **The Telltale Indicator:** A single internal terminal account suddenly initiating a high volume of internal network connection handshakes across multiple sibling endpoints within a short timeframe.

---

## 🛠️ Log Analysis Best Practices & Pitfalls

### Strategic Execution Habits
* **Establish a Baseline First:** You must thoroughly document what normal employee traffic and login routines look like before you can accurately spot abnormal threat signatures.
* **Correlate Across Sources:** Never evaluate logs in silos. An event that looks safe on a firewall log might show up as a clear account compromise when checked against authentication logs.
* **Watch for Regularity:** Highly consistent, automated timing intervals are often far more dangerous than sudden large surges of random network volume.
* **Document as You Go:** Instantly record timestamps, sources, and destinations when hunting anomalies so evidence remains clear for incident reports.

### Common Analytical Blindspots
* **Mistake 1:** Declaring an incident based on a single weird log row. Contextual patterns across time matter far more than single spikes.
* **Mistake 2:** Dismissing DNS traffic as safe or uninteresting background noise.
* **Mistake 3:** Confusing lateral movement pivots with normal IT system administrative tasks. The differentiator is matching the user profile to known, authorized operations.

---

## 🏢 Real-World Strategic Case: SolarWinds (2020)
* **The Incident:** Attackers successfully poisoned a vendor software update mechanism, placing malicious code on thousands of client systems.
* **The Threat Design:** The resulting malicious outbound connections were intentionally engineered to blend perfectly with normal periodic update check-in routines, delaying detection for months.
* **The Lesson:** Modern hackers design malicious traffic patterns specifically to pass casual log inspection. This makes understanding your network's baseline habits mandatory.

---

## 💻 Lab Simulation: Triage and Correlation

### Part 1: Wireshark Suspicious Event Hunting
* **Lab Environment:** Analysing pcap malware files sourced from training databases at `malware-traffic-analysis.net`.
* **Triage Blueprint:** Full investigation credit requires capturing specific timestamps and technical payload properties for **5 distinct suspicious events**, ensuring findings are organized like a standard incident response record.
* **Key Filter Application:** Sorting packet traffic by `dns` strings and evaluating subdomain character string lengths to catch active data tunneling channels.

### Part 2: Hypothetical Cross-Source Correlation Proofs
To validate a finding, an analyst must look for supporting evidence across surrounding system domains:

* **Scenario A (Strengthening a C2 Finding):** If we track an outbound periodic beaconing flow, discovery of an entry inside the local **Auth Log** showing an unauthorized login from an unfamiliar country right before the traffic began drastically increases our threat confidence.
* **Scenario B (Strengthening a Lateral Pivot Finding):** If we observe an endpoint connecting to multiple sibling nodes, locating a line in the target's local **System Log** confirming a new scheduled task was deployed concurrently confirms exploitation.
* **Scenario C (Weakening a Threat Finding):** If an apparent beaconing signature aligns with a **System Log** entry showing a verified corporate patch tool running a scheduled update check, the finding is safely downgraded to a benign background action.

---

## 📝 Assessment Reference
* **Format:** Semi-Guided Team Presentation and Report Submission.
* **Milestone:** Graded report feeds directly into the upcoming **Week 3 MCQ Knowledge Check on Day 4**.


## Additional Materials
* LetsDefend - Event Log (Not started)
* LetsDefend - Useful Log files (Not started)
* LetsDefend - Log management (Not started)
* LetsDefend - Log collection (Not started)



