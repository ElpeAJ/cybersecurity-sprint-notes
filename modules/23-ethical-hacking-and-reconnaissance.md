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
* **Job Posting Reconnaissance:** Open corporate hiring listings explicitly reference internal toolsets and software they use. Attackers scrape these postings because discovering an outdated application name tells them exactly which CVE databases to query for public exploits before firing a single packet.

### Part 2: Recon & Scanning - Active Endpoint Profiling (Command Line Execution)
To cross-examine the target's boundary settings, the following terminal commands were executed inside our local shell environment:

#### 1. Boundary Header Inspection
* **Linux/macOS Script:**
  ```bash
  curl -I https://greenleaf-logistics.onrender.com
  ```
  
<!--<img width="697" height="301" alt="Screenshot 2026-09-14 at 11 39 13 AM" src="https://github.com/user-attachments/assets/1a50a47d-faea-4711-b526-4dc686b9d6e7" />-->
<img width="1003" height="301" alt="Screenshot 2026-09-14 at 11 57 22 AM" src="https://github.com/user-attachments/assets/df2ab8bf-fe53-4d04-9983-2fd896d67987" />

<br><br>
* **Windows PowerShell Script:**
  ```powershell
  (Invoke-WebRequest -Uri "https://greenleaf-logistics.onrender.com" -Method Head -UseBasicParsing).Headers
  ```
<img width="955" height="524" alt="image" src="https://github.com/user-attachments/assets/942313da-e641-4c09-a0f3-e96570ec0c43" />

<br><br>
* **Forensic Finding:** Reveals the cloud routing signatures, server banner tokens, and reverse proxy layers handling incoming corporate connections.

#### 2. Inspecting Ports Status
* **Linux/macOS Script:**
  ```bash
  for port in 80 443 8080 8443 3000 5000; do nc -zv -G greenleaf-logistics.onrender.com $port 2>&1 | grep -E "succeeded|Connection to"; done
  ```
  
<!--<img width="1003" height="191" alt="Screenshot 2026-09-14 at 11 41 26 AM" src="https://github.com/user-attachments/assets/8294f05f-56d3-4318-b6cf-558341a9d8a8" />-->
<img width="1003" height="191" alt="Screenshot 2026-09-14 at 11 58 40 AM" src="https://github.com/user-attachments/assets/342a1f2c-ff08-493e-a009-f96800daff63" />

<br><br>
* **Windows PowerShell Script:**
  ```powershell
  @(80, 443, 8080, 8443, 3000, 5000) | ForEach-Object {
     $res = Test-NetConnection -ComputerName greenleaf-logistics.onrender.com -Port $_ -WarningAction SilentlyContinue
     [PSCustomObject]@{ Port = $_; Open = $res.TcpTestSucceeded }
  }
  ```

<img width="960" height="330" alt="image" src="https://github.com/user-attachments/assets/0418bf61-eb70-46a0-bb2e-06a92b427e32" />

<br><br>
* **Forensic Finding:** Map checking verifies exactly which transport sockets are open or closed, pinpointing alternative backend management ports (like 8080 or 3000) that expand the target area.

#### 3. Cleartext Protocol Redirection Audit
* **Linux/macOS Script:**
  ```bash
  curl -s -o /dev/null -w "StatusCode: %{http_code}\nLocation: %{redirect_url}\n" http://://onrender.com
  ```

<!--<img width="1003" height="135" alt="Screenshot 2026-09-14 at 11 42 55 AM" src="https://github.com/user-attachments/assets/379d90c8-804d-4b97-9e26-d46cc650d0b4" />-->
<img width="1003" height="136" alt="Screenshot 2026-09-14 at 11 59 24 AM" src="https://github.com/user-attachments/assets/65d1ddbe-40b1-4268-bd21-c622b01d55dc" />

<br><br>
* **Windows PowerShell Script:**
  ```powershell
  $res = Invoke-WebRequest -Uri "http://greenleaf-logistics.onrender.com" -MaximumRedirection 0 -UseBasicParsing
  [PSCustomObject]@{ StatusCode = $res.StatusCode; Location = $res.Headers.Location }
  ```

<img width="1128" height="350" alt="image" src="https://github.com/user-attachments/assets/48c10a8b-dfc7-45f8-9f08-be4a65954112" />

<br><br>
* **Forensic Finding:** Returns an explicit `HTTP 301 Moved Permanently` tracking code redirecting traffic straight to the encrypted `https://greenleaf-logistics.onrender.com` portal. Unencrypted port 80 traffic is actively blocked. However, the complete absence of **HSTS (HTTP Strict Transport Security)** headers confirms that initial browser connections still initiate in plaintext.

#### Disable Dynamic Shell Token History Expansion *Only for Mac, so skip if using Powershell*
* **Linux/macOS Script:**
  ```bash
  unsetopt banghist
  ```

* **Essence:** No output in the terminal but Disables the native Zsh feature that treats exclamation marks (`!`) as special history commands, preventing terminal syntax crashes when handling complex password character strings. 

#### 4. Download Public Frontend Asset Bundle Into System Memory Download and Map the Frontend JS Asset Bundle
* **Linux/macOS Script:**
  ```bash
  bundle_url="https://greenleaf-logistics.onrender.com$(curl -s "https://greenleaf-logistics.onrender.com" | grep -oE '/assets/index-[a-zA-Z0-9_-]+\.js' | head -n 1)";
  bundle=$(curl -s "$bundle_url")
  ```

<img width="1003" height="71" alt="Screenshot 2026-09-14 at 1 48 44 PM" src="https://github.com/user-attachments/assets/6a125362-7c38-452c-a3da-64ac297a1200" />

<br><br>
* **Windows PowerShell Script:**
  ```powershell
  $bundleUrl = "https://greenleaf-logistics.onrender.com/assets/index-BjFx252d.js" 
  $bundle = (Invoke-WebRequest -Uri $bundleUrl -UseBasicParsing).Content
  ```
<img width="1130" height="52" alt="image" src="https://github.com/user-attachments/assets/152663ed-e485-4fd2-9a8c-cd799ff1a660" />

<br><br>
* **Essence:** No output in the terminal but 
This downloads the target web server's entire compiled, client-side JavaScript execution logic straight into your active PowerShell session memory without cluttering your storage drives. First, Extract and Store the Bundle URLThis fetches the main page, finds the dynamic asset filename, builds the full URL, and stores it as a regular string variable.Then, Download the Bundle Content into Memory. This downloads the actual JavaScript code from that URL and stores it into the $bundle variable.

  
#### 5. Extract Hardcoded Credentials and Internal Product Identifiers
* **Linux/macOS Script:**
  ```bash
  echo -e "\n=== CRITICAL CREDENTIALS & IDENTIFIERS ==="; echo "$bundle" | grep -oEi '(password|username|secret|api|token|GL-[0-9]{4}-[0-9]+|demo-[a-z0-9_-]+|DemoOnly[a-zA-Z0-9!]+)' | sort -u
  ```

<img width="1003" height="212" alt="Screenshot 2026-09-14 at 2 04 33 PM" src="https://github.com/user-attachments/assets/c2731783-94db-4c29-af07-26edb054a088" />

<br><br>
* **Windows PowerShell Script:**
  ```powershell
  [regex]::Matches($bundle, '(?i)(password|username|secret|api|token|GL-\d{4}-\d+|demo-[a-z0-9_-]+|DemoOnly[a-zA-Z0-9!]+)')| ForEach-Object {$_.Value } | Select-Object -Unique
  ```
  
<img width="1124" height="223" alt="image" src="https://github.com/user-attachments/assets/194f0a8a-c208-42a0-b536-687412bcc528" />

<br><br>
* **Forensic Finding** Pipes the downloaded script block through `grep` using regular expressions to print all unique, hardcoded system configurations and credentials on your screen. This scans the downloaded code for sensitive hardcoded tokens, passwords, and identifiers.

#### 6. Extract Exposed Employee Email Addresses
* **Linux/macOS Script:**
  ```bash
  echo -e "\n=== EXPOSED EMPLOYEE EMAILS ==="; echo "$bundle" | grep -oEi '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}' | sort -u; echo -e "\n================================\n"
  ```
  
<img width="1003" height="167" alt="Screenshot 2026-09-14 at 2 05 29 PM" src="https://github.com/user-attachments/assets/43db40d9-cf72-4a76-8a6c-2a205d9ba509" />

<br><br>
* **Windows PowerShell Script:**
 ```powershell
  [regex]::Matches($bundle, '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}') | ForEach-Object {$_.Value } | Select-Object -Unique
  ```

<img width="1121" height="106" alt="image" src="https://github.com/user-attachments/assets/acaf686b-308e-48e9-b7c2-dbc8260c61bd" />

<br><br>
* **Forensic Findings** Automatically scrapes all corporate email addresses left inside the public JavaScript code, providing an attacker with a target list for spear-phishing campaigns. Scanning and Printing Exposed Emails pulls out all structural corporate email addresses hidden inside the bundle.

---

## Part 3: Advanced Frontend JavaScript Code Auditing (Asset Scrape)
To uncover hidden secrets buried within the public distribution folder layer, we bypassed standard browser views to download, filter, and extract hardcoded parameters directly from the application's compiled assets file using regex string matching.

### 7. Project Dependency Vulnerability Auditing
Download & extract the Zipped file (Website codes) as provided by the owner, change your path to the location of extracted file (folder), then type pnpm audit. If you do not have pnpm installed, follow the steps on Mac(bash terminal) or Windows (powershell with Admin rights) to do so

#### 7a. Download & Extract the Zipped file (Website codes)

#### 7b. Change your path to extracted folder location
* **Linux/macOS Script:**
  ```bash
  cd /path/to/extracted/greenleaf-logistics
  ```
  
  <img width="1003" height="21" alt="Screenshot 2026-09-14 at 3 14 20 PM" src="https://github.com/user-attachments/assets/02b6c8b5-e829-44cc-b496-fdc535e6363c" />

<br><br>
* **Windows PowerShell Script:**
 ```powershell
 cd /path/to/extracted/greenleaf-logistics
  ```
<img width="1127" height="34" alt="image" src="https://github.com/user-attachments/assets/31349fbf-c0dd-4fbd-9dfe-8256336c2e24" />

#### 7c. Install ppm
* **Linux/macOS Script:**
  ```bash
  sudo npm install -g pnpm
  ```
  ```bash
  pnpm -v
  ```

  <img width="1003" height="138" alt="Screenshot 2026-09-14 at 3 15 35 PM" src="https://github.com/user-attachments/assets/7b608888-5801-4748-ba82-87791b443eed" />

<br><br>
* **Windows PowerShell Script:**
 ```powershell
 sudo npm install -g pnpm
  ```

 ```powershell
 Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
  ```

```powershell
 pnpm -v
  ```

<img width="1128" height="227" alt="image" src="https://github.com/user-attachments/assets/37bf9cdc-0293-440b-a010-4abadccfa23b" />

<br><br>
> ***Note*** On Windows, first go to Developer Settings in the Settings app to enable Sudo. Additionally you will need to modify execution policy to allow locally created scripts to run without signature as explained by [codecity.com](https://www.thecodecity.com/vs-code/fixed-running-scripts-is-disabled-on-this-system-in-vs-code/)
>  On Mac, you will need to enter your password during the installation which will not be visible while you type

#### 7d. Scan project dependencies for known vulnerabilities and print report
* **Linux/macOS Script:**
  ```bash
  pnpm audit
  ```
<img width="578" height="724" alt="Screenshot 2026-09-14 at 4 04 42 PM" src="https://github.com/user-attachments/assets/30981724-2a3d-41f6-aefc-bd97f98e7319" />

<br>

<img width="578" height="724" alt="Screenshot 2026-09-14 at 4 05 46 PM" src="https://github.com/user-attachments/assets/5b04777f-93be-44f0-8017-69d94406a1eb" />

<br><br>
* **Windows PowerShell Script:**
 ```powershell
 pnpm audit
  ```
<img width="737" height="718" alt="image" src="https://github.com/user-attachments/assets/8271300e-2497-4560-abe8-a6fa85d92050" />

<br>

<img width="739" height="716" alt="image" src="https://github.com/user-attachments/assets/b7cf8adf-cf4a-4a8a-a7aa-83655b1f7410" />

<br><br>
> ***Note*** pnpm audit scans your project dependencies for known security vulnerabilities and prints a report of affected packages and severity levels. pnpm audit can only be used for Node.js projects that use pnpm as their package manager. It cannot be used for non-JavaScript programming languages or JavaScript projects using other package managers.

#### 7e. Interrogate Source Files for Authentication Loop-Bypasses(Mac) & Target Account Exploitation (Windows)
* **Linux/macOS Script:**
  ```bash
  grep -rEi "(password|username|secret|token|flag|credential|DemoOnly|GL-[0-9]{4})" client/src/ shared/
  ```

<img width="1001" height="398" alt="Screenshot 2026-09-14 at 4 10 52 PM" src="https://github.com/user-attachments/assets/2a85edfe-85d8-47b0-a44b-3d166735b98e" />

<br><br>
* **Windows PowerShell Script:** 
  ```powershell
  [regex]::Matches($bundle, '(?s)(demo-user.*?DemoOnly123!|username\s*===.*?password\s*===.*?\})') | ForEach-Object {$_.Value } | Select-Object -Unique
  ```
<img width="1122" height="100" alt="image" src="https://github.com/user-attachments/assets/f3d57539-71c2-45ca-b8ac-074c2c13e6c2" />

* **Essence:** Code on Mac recursively searches the internal source code directories (`client/src/` and `shared/`) to pinpoint the exact code line where credentials were hardcoded into the project logic.
* Code on Windows focuses regex checking rules explicitly on finding hardcoded conditional logic strings, confirming the presence of cleartext admin login bypass rules (`demo-user` / `DemoOnly123!`). 

---

## Real-World Legal Precedent Case: Aaron Swartz (2011)
* **The Incident:** A prominent technology advocate connected a laptop to an academic network node and ran an automated script to download a massive database of premium research papers.
* **The Core Issue:** Despite having access to the building and holding arguments for public educational benefits, the network administrators did not grant formal **written authorization** for automated mass extraction scripts.
* **The Legal Outcome:** The federal judicial system bypassed arguments regarding the user's intent, treating the unauthorized access as a serious computer crime under the Computer Fraud and Abuse Act (CFAA). This landmark case is cited globally to remind cybersecurity practitioners that authorization is absolute: performing a technical action without a signed contract is treated as a crime, regardless of your intentions.

---

## 📝 Assessment Reference & Verification
* **Format:** Laboratory Entry Logs and OSINT Recon Matrix.
* **Milestone:** Subsystem commands and credential extraction results directly populate the **Week 6 Lab Submission** and feed Wk6 MCQ exam.

---




<!--
*********************WINDOWS*********************
#### 4. Download Public Frontend Asset Bundle Into System Memory
* **Command Executed:**
  ```powershell
  $bundleUrl = "https://greenleaf-logistics.onrender.com/assets/index-BjFx252d.js" 
  $bundle = (Invoke-WebRequest -Uri $bundleUrl -UseBasicParsing).Content
  ```
* **Why It Helps:** This downloads the target web server's entire compiled, client-side JavaScript execution logic straight into your active PowerShell session memory without cluttering your storage drives.

#### 5. Extract Hardcoded Credentials and Internal Product Identifiers
* **Command Executed (wrong 1st, corrected 2nd):**
  ```powershell
  [regex]::Matches(\(bundle, '(?i)(password\vert{}username\vert{}secret\vert{}api\vert{}token\vert{}GL-\d{4}-\d+\vert{}demo-[a-z0-9_-]+\vert{}DemoOnly[a-zA-Z0-9!]+)') \vert{} ForEach-Object {\)_.Value } | Select-Object -Unique
  ```

   ```powershell
  [regex]::Matches($bundle, '(?i)(password|username|secret|api|token|GL-\d{4}-\d+|demo-[a-z0-9_-]+|DemoOnly[a-zA-Z0-9!]+)')| ForEach-Object {$_.Value } | Select-Object -Unique
  ```
* **Why It Helps:** Uses regular expressions to scan the downloaded code memory for sensitive strings like passwords, API keys, or tracking tokens, filtering out duplicate hits to present a clean vulnerability list.

#### 6. Extract Exposed Employee Email Formats
* **Command Executed (wrong 1st, corrected 2nd):**
  ```powershell
  [regex]::Matches(\(bundle, '[a-zA-Z0-9._\%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}') \vert{} ForEach-Object {\)_.Value } | Select-Object -Unique
  ```

  ```powershell
  [regex]::Matches($bundle, '[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}') | ForEach-Object {$_.Value } | Select-Object -Unique
  ```
* **Why It Helps:** Automatically scrapes all corporate email addresses left inside the public JavaScript code, providing an attacker with a target list for spear-phishing campaigns.

#### 7. Execute Local Node Package Security Audit
* **Command Executed:**
  ```powershell
  cd \(\path\to\extracted\greenleaf-\)logistics
  pnpm audit
  ```
* **Why It Helps:** Navigates into the unzipped website codebase directory and runs the `pnpm audit` package manager. This checks the site's project library dependencies straight against global vulnerability databases to find outdated third-party modules.

#### 8. Target Account Session Exploitation
* **Command Executed(Wrong, 2nd, 1st & Corrected):**
  ```powershell
  [regex]::Matches(\(bundle, '(?s)(demo-user.*?DemoOnly123!\vert{}username\s*===.*?password\s*===.*?\})') \vert{} ForEach-Object {\)_.Value } | Select-Object -Unique
  ```

  ```powershell
  [regex]::Matches($bundle, '(?s)(demo-user.*?DemoOnly123!|username\s*===.*?password\s*===.*?\})') | ForEach-Object {$_.Value } | Select-Object -Unique
  ```
* **Why It Helps:** Focuses regex checking rules explicitly on finding hardcoded conditional logic strings, confirming the presence of cleartext admin login bypass rules (`demo-user` / `DemoOnly123!`).


### Part 3: Advanced Frontend JavaScript Code Auditing (Asset Scrape)
To uncover hidden secrets buried within the public distribution folder layer, we bypassed standard browser views to download, filter, and extract hardcoded parameters directly from the application's compiled assets file using regex string matching.

#### The Combined OSINT Pipeline Snippet
*Note for Mac execution environments: Running `unsetopt banghist` disables default Zsh history expansion for exclamation tokens to prevent processing syntax errors.*

-->
