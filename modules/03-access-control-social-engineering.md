# Day 3: Access Control & Social Engineering

## 🔐 Access Control Architecture (AAA)
Defensive frameworks deployed to verify corporate identities and strictly isolate organizational assets:

* **Authentication:** Validates identity assertions. Answers the question: **"Who are you?"** (e.g., via passwords, biometric markers, or MFA tokens).
* **Authorization:** Configures individual permission mapping. Answers the question: **"What are you allowed to do?"** (e.g., read, write, or execute administrative parameters).
* **Access Control Enforcement:** The technical system or process that actively executes and maintains these authorization choices across data resources.

### 🛡️ Core Defensive Strategy: Least Privilege
> **Principle of Least Privilege (PoLP):** Restricting user identities and computing accounts to the absolute bare minimum privileges required to execute their specific job functions. 
> 
> * **Blast Radius Reduction:** If an identity profile is compromised by an external threat actor, restricting its privileges prevents lateral network movement and limits the scope of total data destruction.

---

## 🧠 Social Engineering & Human Hacking
Psychological manipulation techniques engineered to deceive employees into willingly transferring security credentials or sensitive administrative parameters.

### The Four Primary Tactics
1. **Phishing:** Broadly distributed, deceptive communication vectors (most frequently via email) used to collect credentials or distribute malicious file payloads.
2. **Pretexting:** Developing a fabricated background scenario or fake persona to manipulate a target into disclosing sensitive, out-of-bounds information.
3. **Baiting:** Leaving infected physical media (like USB drives) or offering highly desirable free software downloads to exploit user curiosity and introduce malware.
4. **Tailgating:** Following an authorized employee physically through a secured building entrance or biometric checkpoint without presenting valid entry credentials.

### 📊 Real-World Case Studies
* **RSA Incident (2011):** Executed using sophisticated **Phishing** vectors to gain an initial foothold into internal secure network resources.
* **Twitter/X Incident (2020):** Attackers utilized targeted employee **Pretexting** over the phone to systematically hijack high-profile verified user accounts.

### 🧠 Psychological Triggers vs. Defenses

| Psychological Trigger | Threat Actor Mechanism | Your Defensive Protocol |
| :--- | :--- | :--- |
| **Urgency** | Fabricating immediate artificial deadlines to force panic and bypass logical thinking. | **Slow down.** Disconnect from the interaction and verify using an alternate channel. |
| **Authority** | Impersonating executives, law enforcement, or IT support managers to command compliance. | **Verify independently.** Contact the individual directly using an internal company directory. |
| **Trust** | Spoofing known brands, colleague names, or relying on long-term relationships to lower suspicion. | **Check headers.** Inspect inbound addresses thoroughly and flag external cross-domain indicators. |
| **Curiosity / Greed** | Promising unreleased financial updates, gift awards, or special executive files to entice clicks. | **Limit public info.** Avoid posting internal project titles online and report suspicious links immediately. |

---

## ⚖️ Professional Ethics & AI Security
* **The Authorization Rule:** Acquiring specialized offensive testing skills gives you substantial technical power. These tools and techniques must **never** be executed against any infrastructure without direct, written authorization from the system owners.
* **AI Tool Validation:** Artificial Intelligence utilities used for log synthesis or defense verification require constant human technical validation. Never deploy or trust AI recommendations blindly without looking over the code manually.

---

## 📝 Assessment Reference
* **Format:** Self-study reference material.
* **Milestone:** Review thoroughly alongside Day 1 and Day 2 before taking your **Day 4 Viva (Oral Exam)** and **Week 1 MCQ**.
