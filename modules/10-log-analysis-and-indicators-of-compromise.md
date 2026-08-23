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

When performing local digital forensics on an endpoint rather than using a centralized SIEM dashboard [08-siem-concepts-and-alert-triage.md], log paths and access methods change based on the underlying Operating System.

### 1. Microsoft Windows Environments
Windows systems compile logs into structured binary formats (`.evtx`). These cannot be viewed with standard text editors and must be accessed via native diagnostic software.
* **Primary Desktop Utility:** Press `Win + R`, type **`eventvwr.msc`**, and press Enter to launch the **Windows Event Viewer**.
* **Core Event Paths:**
  * **Auth Logs:** Located under `Windows Logs ➔ Security`. (Physical file: `C:\Windows\System32\Winevt\Logs\Security.evtx`).
  * **System Logs:** Located under `Windows Logs ➔ System`. (Physical file: `C:\Windows\System32\Winevt\Logs\System.evtx`).
  * **Web Server Logs (IIS):** Default storage path is located at `C:\inetpub\logs\LogFiles\`.
  * **Firewall Logs:** Disabled by default. When enabled via the Windows Defender Firewall console, the plain text ledger is written directly to `C:\Windows\System32\LogFiles\Firewall\pfirewall.log`.

<br>
<blockquote><em>Auth Logs on Windows</em></blockquote><br>
<img width="1031" height="551" alt="image" src="https://github.com/user-attachments/assets/4dfe9835-a096-4ee4-9468-ba393dbdf361" />

<br><br>
<blockquote><em>System Logs on Windows </em></blockquote><br>


<br><br>
<blockquote><em>Web Server Logs on Windows </em></blockquote><br>



<br><br>
<blockquote><em>Web Server Logs on Windows </em></blockquote><br>



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

## Log Analysis Best Practices & Pitfalls

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

## Real-World Strategic Case: SolarWinds (2020)
* **The Incident:** Attackers successfully compromised a vendor software update mechanism, placing malicious code on thousands of client systems.
* **The Threat Design:** The resulting malicious outbound connections were intentionally engineered to blend perfectly with normal periodic update check-in routines, delaying detection for months.
* **The Lesson:** Modern hackers design malicious traffic patterns specifically to pass casual log inspection. This makes understanding your network's baseline logs/habits mandatory.

---

## Lab Simulation and Activities: Triage and Correlation

### Part 1: Wireshark Suspicious Event Hunting
* **Lab Environment:** Analysing pcap malware files sourced from training databases at `malware-traffic-analysis.net` *(Downloaded 2026-08-09-traffic-analysis-exercise.pcap)*.
* **Triage Blueprint:** Full investigation credit requires capturing specific timestamps and technical payload properties for **5 distinct suspicious events**, ensuring findings are organized like a standard incident response record.
* **Key Filter Application:** Sorting packet traffic by `dns` strings and evaluating subdomain character string lengths to catch active data tunneling channels.

<br><br>
<blockquote><em>Using the sample pcap file downloaded in Wireshark </em></blockquote><br>
<img width="1210" height="755" alt="Screenshot 2026-08-23 at 6 35 13 PM" src="https://github.com/user-attachments/assets/a3f0d25b-6ad0-4b9b-88df-1c66f61b5248" />

<br><br>
<blockquote><em>Using display filter by dns first (displayed 485 packets out of the total 22 473 packets) </em></blockquote><br>
<img width="1210" height="755" alt="Screenshot 2026-08-23 at 6 55 36 PM" src="https://github.com/user-attachments/assets/b4d7e35a-82ef-4cc6-b6d5-4032ea9b2de2" />

<br><br>
<blockquote><em>Follow UDPStream for the first packet on the dns filter </em></blockquote><br>
<img width="1210" height="755" alt="Screenshot 2026-08-23 at 7 01 29 PM" src="https://github.com/user-attachments/assets/5e00ec20-2542-4577-a216-7510b9df1986" />

<br><br>
<blockquote><em>One Conversation Thread between 172.16.8.53(Client) and 172.16.8.8(Host)</em></blockquote><br>
<img width="1210" height="755" alt="Screenshot 2026-08-23 at 7 05 47 PM" src="https://github.com/user-attachments/assets/af47ee9e-ecff-453d-b4cb-c075ce0aede1" />

<br>
<blockquote><em>The One-to-One Conversation Rule <br>
My previous view showed a list of every single DNS query across your whole network (485 packets).
However, clicking Follow Stream forces Wireshark to act like a private investigator focusing on just one conversation thread, by filtering out the noise. The moment the client machine finishes asking those specific questions and closes that temporary local port (62757), that specific UDP stream is over. Any other DNS requests on your network are part of a completely separate stream number. Hence why the above stream only displayed 6 packets.</em></blockquote>

<br><br>
<blockquote><em>Another Convo stream between 172.16.8.9 and 172.16.8.8 </em></blockquote>![Uploading Screenshot 2026-08-23 at 8.02.41 PM.png…]()

<img width="1210" height="755" alt="Screenshot 2026-08-23 at 7 32 24 PM" src="https://github.com/user-attachments/assets/dfec9b42-1233-46d9-820f-ddb80c7b8359" />

<br><br>
<blockquote><em>Using display filter by tcp next. (Displayed 20 859 packets out of the total 22 473 packets) </em></blockquote><br>
<img width="1210" height="755" alt="Screenshot 2026-08-23 at 7 42 37 PM" src="https://github.com/user-attachments/assets/febef477-e54c-4c4a-bbf6-bcb1266b379f" />

### Analysis of Layer 4/7 TCP Traffic Sequences From Above image

#### A. The TCP 3-Way Handshake (Packets 27, 28, 29)
* **What is happening:** Host `172.16.8.53` is establishing a formal connection with Server `172.16.8.8` on port 389. 
* **The Sequence:** 
  1. `49667 -> 389 [SYN]`: The client requests to synchronize a connection.
  2. `389 -> 49667 [SYN, ACK]`: The server acknowledges the request and sends its own sync flag.
  3. `49667 -> 389 [ACK]`: The client acknowledges the server, establishing a stable, reliable pipeline.

#### B. LDAP - Lightweight Directory Access Protocol (Packets 30 & 32)
* **What is happening:** Running over Port 389. This is active Windows Active Directory communication.
* **The Sequence:** The client asks the domain controller to search for system objects (`searchRequest(2) "<ROOT>"`). The server successfully answers back and finishes the query (`searchResDone(2) success`).

#### C. HTTP Cleartext Web Request (Packets 54 through 62)
* **What is happening:** Host `172.16.8.53` connects to an external public IP address `23.33.85.235` on web port 80.
* **The Sequence:** 
  * After a 3-way handshake (Packets 54-56), the client sends an application request: `GET /connecttest.txt HTTP/1.1`.
  * The web server responds back with `HTTP/1.1 200 OK (text/plain)`, confirming a successful page download.
  * The conversation terminates gracefully in Packet 61 with a `[FIN, ACK]` (Finish/Acknowledge) flag sequence.

#### D. SMB / SMB2 - Server Message Block (Packets 68 through 74)
* **What is happening:** Running over Port 445. This protocol handles local corporate file sharing, network folders, and remote printing.
* **The Sequence:** The client initiates a `Negotiate Protocol Request` to ask the server what version of file sharing it supports. The server answers with `Negotiate Protocol Response` utilizing the modern `SMB2` dialect standard.

<br><br>
<blockquote><em>More Protocols with tcp filter </em></blockquote><br>
<img width="1210" height="755" alt="Screenshot 2026-08-23 at 8 18 44 PM" src="https://github.com/user-attachments/assets/5dc087b8-22de-4f65-906d-c53be4e86dd3" />

#### E. SAMR - Security Account Manager Remote Protocol (Packets 2931-2958)
* **What is happening:** Host `172.16.8.49` initiates a systematic lookup against the domain controller (`172.16.8.8`) to enumerate domain objects.
* **The Sequence:**
  * `LookupNames request`: The client verifies targeted user account strings against the database.
  * `OpenUser request` & `QueryUserInfo`: Exfiltrates profile metadata regarding account security.
  * `GetGroupsForUser request`: Identifies structural group memberships to discover privilege escalation routes.
* **Analyst Verdict:** This dense, automated account enumeration pattern via SAMR is a primary indicator of active host-level reconnaissance preceding lateral movement maneuvers.

<br><br>
<blockquote><em>More Protocols with tcp filter (KRB5, DCERPC, EPM) </em></blockquote><br>
<img width="1210" height="755" alt="Screenshot 2026-08-23 at 8 04 41 PM" src="https://github.com/user-attachments/assets/79fa2118-c767-4e85-8380-b08ab60cc2c1" />

#### F. KRB5 — Kerberos Version 5 Protocol (Authentication)
* **What is happening:** Host `172.16.8.53` is authenticating against the central domain controller (`172.16.8.8`) using encrypted security tokens (tickets).
* **The Sequence:** 
  * `AS-REQ` (Authentication Service Request): The client workstation requests an initial Ticket Granting Ticket (TGT).
  * `KRB Error: KRB5KDC_ERR_PREAUTH_REQUIRED`: The server drops the initial request. *Analyst Note: This is standard behavior indicating that Kerberos Pre-Authentication is actively enforced; the client must encrypt its local timestamp using its password hash to successfully authenticate.*

#### G. DCERPC — Distributed Computing Environment / Remote Procedure Call
* **What is happening:** Client workstation `172.16.8.53` uses this protocol pipeline to execute software routines and system management tasks directly on the remote server (`172.16.8.8`) as if they were running locally.
* **The Sequence:** `Bind: call_id: 2, Fragment: Single` loops are initiated by the client to attach to internal programmatic interfaces within the Active Directory architecture.

#### H. EPM — Endpoint Mapper Protocol
* **What is happening:** Operates on standard **Port 135** to serve as a traffic coordinator or network switchboard for remote DCERPC calls.
* **The Sequence:** 
  * `Map request`: The client machine queries EPM over Port 135 to ask which dynamic high-range port a specific Active Directory subsystem is currently running on.
  * `Map response`: EPM transmits the dynamic port identifier back, allowing the client to shift its traffic pipeline to that specific target port socket.

<br><br>
<blockquote><em>More Protocols with tcp filter </em></blockquote><br>
<img width="1024" height="638" alt="image" src="https://github.com/user-attachments/assets/5092edba-ef06-46b6-9d1e-8c481b54d707" />

#### I. DRSUAPI — Directory Replication Service Remote Protocol
* **What is happening:** Host `172.16.8.53` initiates a structural handshake to bind to the directory database replication engine (`DSBind request`).
* **The Threat Hunter's Context:** While legitimate between sibling domain controller servers for database synchronization, an ordinary user workstation executing a DRSUAPI bind is a high-severity **Indicator of Compromise (IoC)**. This pattern matches a **DCSync Attack (e.g., weaponized via Mimikatz)**, where an attacker impersonates a domain controller to force the server to replicate and leak its entire database of user password hashes.


<br><br>
<blockquote><em>Follow TCPStream for the first packet on the tcp filter </em></blockquote><br>
<img width="1210" height="755" alt="Screenshot 2026-08-23 at 7 48 42 PM" src="https://github.com/user-attachments/assets/56eef530-1784-4f6a-b84d-6bf866de32bb" />










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



