# Day 23: Ethical Hacking & Reconnaissance

## Foundations of Ethical Hacking & Legality
* **Ethical Hacking:** The broad defensive practice of leveraging security tools and exploit methodologies to identify network and application flaws with full authorization. It encompasses activities such as research, vulnerability hunting, and bug bounty challenges.
* **The Locksmith Analogy:** Ethical hacking functions exactly like a property owner hiring a professional locksmith to test their front door lock. The locksmith attempts to manipulate the cylinder to find structural defects before a criminal does.
* **The Authorization Rule:** Documented, explicit **written authorization** from the asset's actual legal owner is the single boundary parameter that separates a security service from a severe cybercrime. Good intentions or professional profiles do not make an unauthorized scan legal; the law looks strictly at formal authorization.

---

## The Five Distinct Phases of Offensive Security

Offensive security engagements move systematically through a five-stage operational workflow lifecycle:

```text
+---------------------------------------------------------------------------------+
|                                                                                 |
|                          THE ETHICAL HACKING LIFECYCLE                          |
+---------------------------------------------------------------------------------+
|                                                                                 |
|  1. RECONNAISSANCE ----> 2. SCANNING -----------> 3. EXPLOITATION ------------> |
|                                                                                 | 
|     Passive gathering       Active footprinting        Loophole weaponization   |
|     (OSINT Scrapes)         (Nmap Port Probes)         (Access Authorization)   |
+---------------------------------------------------------------------------------+
|                                                                                 |
|                                                                                 |                                                                                  
|  5. REPORTING <---------------   4. POST-EXPLOITATION <-------------------------|
|                                                                                 |
|     Documentation                   Privilege Pivot Log                         |
|   (Triage Executive summary).       ("I got in, now what?")                     |
+---------------------------------------------------------------------------------+
```

1. **Reconnaissance:** Gathering baseline operational data about the target infrastructure without touching the assets directly, focusing heavily on public channels.
2. **Scanning:** Actively probing network boundaries to locate live systems, open port vectors, and running service version strings.
3. **Exploitation:** Weaponizing a discovered flaw or logic loophole to establish initial access. Exploits fall into three vector categories:
   * *Remote Exploits:* Launched across network routing layers to target a listening service.
   * *Local Exploits:* Executed inside a host terminal container using an existing user account to escalate privileges.
   * *Client-Side Exploits:* Placing a malicious file trap (such as phishing links or macro forms) that requires user execution.
4. **Post-Exploitation:** Evaluating the scope of the initial compromise by answering the operational question: *"I got in, now what?"* This includes mapping surrounding corporate directories and tracking data assets without inflicting system damage.
5. **Reporting:** Aggregating all findings into a structured, formal business document featuring an Executive Summary, risk tables, and remediation blueprints to guide patching efforts.

---

## Strategic Concept Engineering: Scope vs. Rules

### 1. Reconnaissance vs. Scanning
* **Reconnaissance (Passive Layer):** It scrapes open social media data, company web directories, public news, and corporate job postings to build a blueprint without triggering automated alerts.
* **Scanning (Active Layer):** Involves hitting target host firewalls and ports directly using discovery packets (such as Nmap sweeps). Scanning leaves a highly visible, permanent forensic footprint that is easily logged by network defense monitors.
* **Analyst Insight:** Real-world threat actors spend up to 70% of their timeline constraints purely on Reconnaissance. Gathering rich information beforehand allows them to locate weak targets precisely, minimizing the time spent running loud, active scans that alert the incident response team.

### 2. Penetration Testing vs. Broad Ethical Hacking
* **Penetration Testing:** A formal, contracted type of ethical hacking engagement. It operates under a rigid timeline, adheres to explicit boundary parameters, follows strict rules of engagement, and demands a comprehensive final report deliverable.
* **Broad Ethical Hacking:** An overarching umbrella category that includes independent bug bounty hunting, academic vulnerability research, open-source security engineering, and red-team simulation scenarios.
* **The Verdict:** Every certified penetration tester functions as an ethical hacker, but not every ethical hacker works as a formal contracted penetration tester.

### 3. Scope and Rules of Engagement (RoE)
* **The Scope:** The definitive checklist of explicit host domain names, numerical IP address strings, and application portals that are legally allowed to be tested. Treating scope as a loose suggestion is a severe professional violation; hitting an unlisted system is a crime.
* **Rules of Engagement:** The operational bounds of the execution cycle. It specifies the allowed testing time windows, explicitly defines which exploitation scripts are forbidden, and provides urgent system contacts if a production server drops offline.

---

## 💻 Lab Activity: OSINT & Code Audit (GreenLeaf Logistics)
*Target Profile:* GreenLeaf Logistics, a 30-person logistics and delivery provider running its primary public web presence on the Render cloud hosting infrastructure platform [greenleaf-logistics](https://greenleaf-logistics.onrender.com).

>***Note:*** The owner of the site gave us permission to use the site at the time of the Lab.
> Also have the site running these scripts in the terminal

### Part 1: Passive Web Footprint Discovery (Manual OSINT Layer)
* **Corporate Email Matrix Pattern:** `hello@greenleaf.com`
* **Internal Tracking Identifier Schema:** Compiled as `CompanyInitials_Year_Number` (e.g., `GL_2026_001234`).
* **Communications Boundary Vector:** Corporate contact routing is tied to phone line `+233 000000000`.
* **Platform Footprint Vulnerability:** The host asset relies on the Render hosting platform architecture, exposing the system to public cloud configuration vulnerabilities documented as of April 2026.
* **Job Posting Reconnaissance:** Open corporate hiring listings explicitly reference internal toolsets and software dependencies. Attackers scrape these postings because discovering an outdated application name tells them exactly which CVE databases to query for public exploits before firing a single packet.

### Part 2: Active Endpoint Profiling (Command Line Execution)
To cross-examine the target's boundary settings, the following terminal commands were executed inside our local shell environment:

#### 1. Boundary Header Inspection
* **Linux/macOS Script:**
  ```bash
  curl -I https://greenleaf-logistics.onrender.com
  ```
* **Windows PowerShell Script:**
  ```powershell
  (Invoke-WebRequest -Uri "https://greenleaf-logistics.onrender.com" -Method Head -UseBasicParsing).Headers
  ```
* **Forensic Finding:** Reveals the cloud routing signatures, server banner tokens, and reverse proxy layers handling incoming corporate connections.

#### 2. Inspecting Ports Status
* **Linux/macOS Script:**
  ```bash
  for port in 80 443 8080 8443 3000 5000; do nc -zv -G greenleaf-logistics.onrender.com $port 2>&1 | grep -E "succeeded|Connection to"; done
  ```
* **Windows PowerShell Script:**
  ```powershell
  @(80, 443, 8080, 8443, 3000, 5000) | ForEach-Object {
     $res = Test-NetConnection -ComputerName greenleaf-logistics.onrender.com -Port $_ -WarningAction SilentlyContinue
     [PSCustomObject]@{ Port = $_; Open = $res.TcpTestSucceeded }
  }
  ```
* **Forensic Finding:** Map checking verifies exactly which transport sockets are open or closed, pinpointing alternative backend management ports (like 8080 or 3000) that expand the target area.

#### 3. Cleartext Protocol Redirection Audit
* **Linux/macOS Script:**
  ```bash
  curl -s -o /dev/null -w "StatusCode: %{http_code}\nLocation: %{redirect_url}\n" http://://onrender.com
  ```
* **Windows PowerShell Script:**
  ```powershell
  $res = Invoke-WebRequest -Uri "http://greenleaf-logistics.onrender.com" -MaximumRedirection 0 -UseBasicParsing
  [PSCustomObject]@{ StatusCode = $res.StatusCode; Location = $res.Headers.Location }
  ```
* **Forensic Finding:** Returns an explicit `HTTP 301 Moved Permanently` tracking code redirecting traffic straight to the encrypted `https://greenleaf-logistics.onrender.com` portal. Unencrypted port 80 traffic is actively blocked. However, the complete absence of **HSTS (HTTP Strict Transport Security)** headers confirms that initial browser connections still initiate in plaintext.

---

### Part 3: Advanced Frontend JavaScript Code Auditing (Asset Scrape)
To uncover hidden secrets buried within the public distribution folder layer, we bypassed standard browser views to download, filter, and extract hardcoded parameters directly from the application's compiled assets file using regex string matching.

#### The Combined OSINT Pipeline Snippet
*Note for Mac execution environments: Running `unsetopt banghist` disables default Zsh history expansion for exclamation tokens to prevent processing syntax errors.*

```bash
