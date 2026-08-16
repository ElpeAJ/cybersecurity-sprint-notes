# Day 4: Viva & MCQ Comprehensive Study Guide

This guide compiles the technical foundations, case studies, and a complete 18-question mock exam covering Days 1–3 of the Cybersecurity Sprint.

---

## The 3-Step Viva Reasoning Framework
When answering oral questions, do not just recite a definition. Follow this structure:
1. **Identify the Asset:** What data, system, or user is targeted?
2. **Isolate the Pillar/Vector:** Which CIA element or security mechanism is broken?
3. **Explain the Mechanics:** How did it happen step-by-step, and what is the impact?

---

## Master Case Studies (Oral Exam Scripts)

### Scenario 1: City Government Ransomware Attack
* **Your Answer:** "This represents an attack on **Availability**. Ransomware is malware that encrypts operational assets. By locking out legitimate users from their infrastructure, the system cannot be accessed when needed, stopping operations until it is recovered."

### Scenario 2: Unauthorized Modification of an Accounting Spreadsheet
* **Your Answer:** "This is a direct violation of **Integrity**. A breakdown in access controls allowed unauthorized modification of critical financial data. The information is no longer trustworthy or accurate, destroying data reliability."

### Scenario 3: The 2020 Twitter Pretexting Breach
* **Your Answer:** "This is **Social Engineering** via **Pretexting**. Threat actors used a fabricated persona over the phone to deceive employees. They bypassed formal **Authentication** mechanisms by manipulating human trust, gaining administrative controls to hijack accounts."

---

## Mock Practice Exam

#### Q1: Which element of the risk formula represents a weakness or flaw in code that could be exploited by an attacker?
* A) Threat
* B) Vulnerability
* C) Impact
* D) Asset
* *Answer: **B***

#### Q2: An attacker executes a Distributed Denial of Service (DDoS) attack against a web portal. Which pillar of the CIA Triad is directly broken?
* A) Confidentiality
* B) Integrity
* C) Availability
* D) Authentication
* *Answer: **C***

#### Q3: During a data breach, sensitive client records are stolen and leaked online, but the original database remains unedited and running. What was compromised?
* A) Integrity only
* B) Availability only
* C) Confidentiality only
* D) Confidentiality and Integrity
* *Answer: **C***

#### Q4: A security professional uses hashing (SHA-256) to ensure that log files have not been modified. Which security pillar does this control enforce?
* A) Confidentiality
* B) Availability
* C) Authentication
* D) Integrity
* *Answer: **D***

#### Q5: What is the primary difference between Authentication and Authorization?
* A) Authentication sets privileges; Authorization verifies identity.
* B) Authentication answers "Who are you?"; Authorization answers "What can you do?".
* C) Authentication handles availability; Authorization handles confidentiality.
* D) There is no difference; they are interchangeable terms.
* *Answer: **B***

#### Q6: Restricting user accounts to the absolute bare minimum permissions needed to complete daily work tasks is known as what?
* A) Advanced Persistent Threat (APT)
* B) Social Engineering Defense
* C) Principle of Least Privilege
* D) Risk Multiplication
* *Answer: **C***

#### Q7: Why does restricting user privileges reduce an organization's overall cybersecurity "blast radius"?
* A) It stops all phishing emails from entering the network inbox.
* B) It prevents an attacker from moving laterally if a single low-level account is breached.
* C) It automatically patches operating system flaws.
* D) It eliminates the threat of external nation-state actors entirely.
* *Answer: **B***

#### Q8: A nation-state group spends 14 months quietly extracting intelligence from a defense contractor's network without making noise. What threat type is this?
* A) Ransomware
* B) Smishing
* C) Advanced Persistent Threat (APT)
* D) Baiting
* *Answer: **C***

#### Q9: What specific vulnerability did the WannaCry ransomware exploit to spread through corporate networks so rapidly?
* A) Deceptive text messages targeting HR managers
* B) Unpatched operating system vulnerabilities
* C) Stolen hardware from an unencrypted laptop
* D) Weak passwords on the public cloud bucket
* *Answer: **B***

#### Q10: A Business Email Compromise (BEC) attack combines which two concepts to cause massive financial harm?
* A) Malware encryption and hardware theft
* B) Phishing/Social Engineering and Confidentiality breach
* C) DDoS traffic flooding and unpatched vulnerabilities
* D) Blind AI generation and data alteration
* *Answer: **B***

#### Q11: An attacker leaves a malware-loaded USB drive in a corporate parking lot, hoping a curious employee will plug it into a workstation. What is this tactic?
* A) Tailgating
* B) Pretexting
* C) Baiting
* D) Smishing
* *Answer: **C***

#### Q12: A person dressed in a delivery uniform follows closely behind an employee entering a secure data center without scanning a badge. This is:
* A) Phishing
* B) Tailgating
* C) Baiting
* D) APT
* *Answer: **B***

#### Q13: An employee receives an SMS text message claiming to be from HR requesting immediate confirmation of their banking details. What is this attack?
* A) Phishing
* B) Pretexting
* C) Smishing
* D) Malware
* *Answer: **C***

#### Q14: Which psychological trigger is being exploited when an attacker masquerades as an executive demanding urgent action under a strict deadline?
* A) Curiosity and Trust
* B) Greed and Availability
* C) Authority and Urgency
* D) Integrity and Hashing
* *Answer: **C***

#### Q15: What is the most effective immediate defense when faced with an interaction leveraging artificial urgency?
* A) Click the link immediately to see if it is legitimate.
* B) Slow down, disconnect, and verify through a separate, trusted corporate channel.
* C) Ignore the message and disable the local firewall.
* D) Use an AI tool to reply to the message automatically.
* *Answer: **B***

#### Q16: If you use an AI tool to help write a script for network defense monitoring, what does "Responsible Use" require?
* A) Trust the script blindly because AI is highly accurate.
* B) Upload all your company's actual network credentials to the AI to get customized code.
* C) Perform human validation to verify the script's accuracy and safety before executing it.
* D) Avoid using AI altogether for technical writing.
* *Answer: **C***

#### Q17: Under what conditions is a cybersecurity analyst permitted to use offensive hacking tools against an external network asset?
* A) Whenever they suspect the external asset is hosting malware.
* B) Only after receiving explicit, documented authorization from the asset owner.
* C) If they are practicing for a timed course quiz or viva.
* D) If the external network has an unpatched vulnerability.
* *Answer: **B***

#### Q18: According to the slide summaries, real-world security incidents can often "blur categories." What should an analyst focus on?
* A) Relying strictly on generic textbook labels.
* B) Memorizing standard definitions without looking at logs.
* C) What actually happened operationally during the attack sequence.
* D) Avoiding any scenario that involves multi-stage threat vectors.
* *Answer: **C***

#### Q19: An engineer structures a precise, context-rich query for a Large Language Model to map out network vulnerabilities. This practice is known as:
* A) Generative AI Bypass
* B) Prompt Engineering
* C) Authentication Tuning
* D) Pretext Design
* *Answer: **B***

#### Q20: If an attacker modifies the DNS records of a business to route legitimate traffic to a malicious replica website, which CIA pillar is directly violated?
* A) Availability
* B) Confidentiality
* C) Integrity
* D) Accounting
* *Answer: **C***

#### Q21: Which core component of the AAA framework is responsible for enforcing the access limits decided during authorization?
* A) Access Control Enforcement
* B) Password Hashing
* C) Phishing Identification
* D) Perimeter Fencing
* *Answer: **A***

#### Q22: An executive's laptop is stolen out of their vehicle. The drive is unencrypted and contains unreleased intellectual property. What is the immediate consequence?
* A) Loss of Integrity due to data moving location.
* B) Loss of Confidentiality because unauthorized parties can read the data.
* C) Loss of Availability because the cloud server will drop.
* D) Increase in Threat Capability due to machine degradation.
* *Answer: **B***

#### Q23: An identity verification system requires a user to supply a password and a dynamic code from an authenticator app. This process is called:
* A) Discretionary Authorization
* B) Multi-Factor Authentication
* C) Symmetric Encryption Integrity
* D) Pretext Verification
* *Answer: **B***

#### Q24: A careless employee regularly writes down network passwords on paper notes left under their keyboard. How is this risk classified?
* A) Nation-state Advanced Persistent Threat (APT)
* B) External Malware Distribution
* C) Insider Threat
* D) Baiting Vector
* *Answer: **C***

#### Q25: Why is human validation mandatory when integrating AI tools into security operations logs?
* A) AI tools are unable to process text summaries.
* B) AI systems can hallucinate or output inaccurate data.
* C) To bypass the Principle of Least Privilege.
* D) Because AI tools cannot write syntax rules.
* *Answer: **B***

#### Q26: An attack campaign leverages an attractive web offer for a free cybersecurity PDF tool to trick IT staff into executing an unknown `.exe` payload. This is an example of:
* A) Tailgating
* B) Smishing
* C) Baiting
* D) Account Auditing
* *Answer: **C***

#### Q27: Which element of the Risk equation is modified when a company deploys a security control to fix a broken software policy?
* A) Threat
* B) Vulnerability
* C) Impact
* D) Asset Value
* *Answer: **B***

### Q28: In the 2011 RSA breach, attackers used targeted communication messages containing dangerous attachments. What was the vector?
* A) Tailgating
* B) Ransomware Encryption
* C) Phishing
* D) Unpatched Cloud Storage
* *Answer: **C***

### Q29: An attacker pretends to be a third-party auditor on a corporate call to extract server names from a new intern. This fake scenario is called:
* A) Hashing
* B) Pretexting
* C) Baiting
* D) Defending
* *Answer: **B***
  
### Q30: Why must security teams limit public technical information posted on platforms like LinkedIn or corporate blogs?
* A) To reduce the likelihood of attackers crafting highly targeted social engineering pretexts.
* B) It automatically guarantees system Confidentiality.
* C) To eliminate the possibility of unpatched vulnerabilities.
* D) To increase the operational efficiency of GenAI systems.
* *Answer: **A***
