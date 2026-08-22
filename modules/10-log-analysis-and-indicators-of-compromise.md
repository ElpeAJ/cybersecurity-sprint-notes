# Day 10: Log Analysis & Indicators of Compromise

## Log Analysis Fundamentals
* **Log Definition:** A chronological, written record of system, network, or application events captured automatically with precise timestamps. It functions like an automated digital sign-in sheet for an operating environment.
* **Core Purpose:** Threat hunters analyze behavioral logs to find anomalies, track hacker intrusions, and uncover bypass mechanisms designed to breach network assets.

---

## 📊 The Four Primary Corporate Log Sources

Security investigations require parsing multiple telemetry feeds to reconstruct an incident cleanly:

| Log Category | Core Recorded Parameters | Practical Investigative Use Case | Structural Data Line Example |
| :--- | :--- | :--- | :--- |
| **Auth Log** | Tracks access control assertions, tracking failed or successful login attempts, password updates, and privilege changes. | The primary triage engine for auditing brute-force vectors or compromised user credentials. | `Failed password for user admin from 41.2.3.10, 02:47 AM` |
| **System Log** | Captures kernel and OS events, service parameters, run states, application errors, and unexpected crashes. | Ideal for spotting unauthorized automated script installations or system services starting up unexpectedly. | `Service Update-Agent started successfully, 09:00 AM` |
| **Web Server Log** | Records every connection request delivered to a web application, exposing client source IPs, resource URLs, and response statuses. | Used heavily to audit directories for active vulnerability scanning or remote code exploitation attempts. | `GET /login.php from 102.44.5.6, response 200 (success)` |
| **Firewall Log** | Logs perimeter network boundary connections, tracking allowed blocks and dropped packet actions. | Critical for mapping out blocked attack vectors or identifying anomalous unauthorized outbound data connections. | `BLOCKED, inbound connection attempt on port 23 from 185.9.2.1` |

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



