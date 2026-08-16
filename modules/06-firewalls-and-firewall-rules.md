# Day 6: Firewalls & Firewall Rules (Perimeter Defense)

## 🛡️ Firewall Fundamentals & Rule Components
* **Firewall Definition:** A security system designed to monitor and control inbound and outbound network traffic based on predetermined security rules. It establishes a barrier between a trusted internal network and an untrusted external network.
* **The 4 Core Rule Components:** Every firewall rule must explicitly define these four variables:
  1. **Direction:** Inbound (ingress) or Outbound (egress).
  2. **Protocol:** The transport mechanism layer used (typically TCP or UDP).
  3. **Port:** The specific communication channel identifier (e.g., Port 80, Port 443).
  4. **Action:** What the firewall does when a packet matches the rule parameters (**Allow/Pass** or **Block/Deny**).

---

## 🔄 Filtering Mechanics: Stateful vs. Stateless

| Property | Stateful Inspection | Stateless Filtering |
| :--- | :--- | :--- |
| **Connection Tracking** | Tracks the operational state of active network connections. | Inspects individual packets in total isolation. |
| **Context Awareness** | Knows if an incoming packet is a legitimate reply to an outbound request you already made. | Has no memory or context of past packets or open sessions. |
| **Rule Overhead** | Requires only **one rule**. (If you allow outbound web traffic, the reply is automatically let back in). | Requires **two separate rules** (one to allow traffic out, and a second explicit rule to let return traffic in). |
| **Default Behavior** | **Windows Defender Defaults:** Inbound traffic is blocked by default; Outbound traffic is allowed by default. | Must be explicitly configured manually in both directions for every protocol. |

### 🛠️ Firewall Rule Design Best Practices
* **Deny by Default:** Lock down all network paths entirely, then only open doors for explicitly authorized services.
* **Be Specific:** Narrow down rules by limiting specific source/destination IP spaces rather than leaving parameters open to any network zone.
* **Document Every Rule:** Maintain strict documentation detailing exactly *why* a rule was created and who authorized it.
* **Review Regularly:** Audit rules continuously to delete obsolete permissions that increase attack surfaces.

---

## 🏢 Real-World Firewall Failures

### Case 1: Target Breach (2013)
* **The Root Cause:** Deficiencies in internal network segmentation. 
* **The Impact:** Attackers compromised a third-party HVAC vendor credential, gained entry to the perimeter, and moved laterally to retail Point-of-Sale (POS) systems due to loose internal filtering rules.

### Case 2: Capital One Breach (2019)
* **The Root Cause:** A single misconfigured Web Application Firewall (WAF) rule.
* **The Impact:** A cloud misconfiguration allowed a malicious actor to trick the firewall proxy, leading to server-side resource exploitation and massive database theft.

---
## Setup to use Windows Defender
* On Windows, use the search bar to search for Windows defender firewall and open
* On the pop-up menu select Advanced settings in the vertical menu
* Click on inbound rules to see all inbound rules and or set new rules and likewise for outbound rules.
  <img width="1041" height="385" alt="image" src="https://github.com/user-attachments/assets/314b2782-0dca-4f6e-9206-8afd1b939bd3" />
  <!--<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/23d4015c-7d4f-4c19-b774-366f0db080d1" />-->
---
## Class Exercise - Creating Inbound and Outbound Rules on Windows Defender
Translating plain business language instructions into strict technical firewall rule parameters:

### 1. Web Traffic Policy
* **Plain Language:** Allow outbound web traffic on ports 80 and 443.
* **Technical Translation:** 
  * **Direction:** Outbound | **Protocol:** TCP | **Port:** 80, 443 | **Action:** Allow
<img width="481" height="359" alt="Create new rule" src="https://github.com/user-attachments/assets/49dd0f1e-1159-4c67-b410-7f4677cc635e" />
Then click **Next**
<!--<img width="891" alt="choose port " src="https://github.com/user-attachments/assets/22d7fd66-6528-47e1-bdb2-384ca576980a" />-->
<img width="653" height="403" alt="image" src="https://github.com/user-attachments/assets/59649dac-407b-47b2-87f0-68f268679655" />
Then click **Next**
<img width="705" height="370" alt="image" src="https://github.com/user-attachments/assets/14067cdf-fade-4695-a62b-dd88ebd8330b" />
Then click **Next**
<img width="679" height="367" alt="image" src="https://github.com/user-attachments/assets/2cdf81ca-2684-4e2c-ab58-e508842f01b1" />
Then click **Next**
<img width="632" height="332" alt="image" src="https://github.com/user-attachments/assets/d939bde6-9763-4bbe-b19c-58b0cbe8c575" />
Then click **Next**
<img width="654" height="386" alt="image" src="https://github.com/user-attachments/assets/151ca141-31c4-49da-bd77-51c59f967c1c" />
Then click **Finish**
#### *NEW OUTBOUND RULE CREATED*
* <img width="1038" height="208" alt="image" src="https://github.com/user-attachments/assets/921e4c81-cbb2-4004-bcde-53034f48b259" />


### 2. Inbound Shell & Web Secure Policy
* **Plain Language:** Block all inbound traffic except SSH (22) and HTTPS (443).
* **Technical Translation:**
  * **Rule A:** Direction: Inbound | Protocol: TCP | Port: 22 | Action: Allow
  * **Rule B:** Direction: Inbound | Protocol: TCP | Port: 443 | Action: Allow
  * **Rule C (Catch-All):** Direction: Inbound | Protocol: Any | Port: Any | Action: Block
<img width="376" height="346" alt="image" src="https://github.com/user-attachments/assets/e037403f-7e40-4120-bd8a-b1576bcde6af" />
<img width="501" height="346" alt="image" src="https://github.com/user-attachments/assets/b1f06a48-f2a8-4026-8428-4653f5fdd296" />
<img width="587" height="303" alt="image" src="https://github.com/user-attachments/assets/e3b3385d-1ca4-4e68-bdc2-65c1e11353ad" />
<img width="649" height="318" alt="image" src="https://github.com/user-attachments/assets/37ff0fb6-65f3-4c51-9964-fdcf324a1180" />
<img width="623" height="281" alt="image" src="https://github.com/user-attachments/assets/b873df9b-7f40-4829-bc48-69d87edf8b1f" />
<img width="557" height="308" alt="image" src="https://github.com/user-attachments/assets/a3cc9d57-110e-4e7b-83d8-9dc2304545d0" />
<img width="1034" height="226" alt="image" src="https://github.com/user-attachments/assets/79231d76-08f3-4a4b-95fd-abf8bb7a6526" />






### 3. Perimeter Isolation Policy
* **Plain Language:** Block a specific external malicious network range (`203.0.113.0/24`) in both directions.
* **Technical Translation:**
  * **Rule A:** Direction: Inbound | Source IP: `203.0.113.0/24` | Port: Any | Action: Block
  * **Rule B:** Direction: Outbound | Destination IP: `203.0.113.0/24` | Port: Any | Action: Block

### 4. Legacy Protocol Blacklisting
* **Plain Language:** Block unencrypted legacy Telnet connections.
* **Technical Translation:**
  * **Direction:** Inbound/Outbound | Protocol: TCP | Port: 23 | Action: Block

---

## 💻 Practical Lab Walkthrough: macOS Packet Filter (PF)
*Note: While Windows uses Windows Defender graphical user interface controls, macOS uses a powerful terminal tool called **PF (Packet Filter)** to manage network connections.*

### Step 1: Initialize local server space
Open Terminal Window 1 and spin up a mock target web server listening locally on port 5555:
```bash
nc -l 5555
```

### Step 2: Test basic connectivity
Open Terminal Window 2 and connect to your mock web server instance. Type a test phrase and hit enter; it should appear across Window 1 instantly, confirming no firewall rules are interfering yet:
```bash
nc localhost 5555
```
*Close out both connections inside their terminals using `Ctrl + C` before continuing.*

### Step 3: Audit default firewall state
Check the system's underlying firewall configuration status. By default, macOS has this backend utility disabled:
```bash
sudo pfctl -s info
```

### Step 4: Write a stateful rule syntax
Open an anchor configuration workspace file inside your command line text editor:
```bash
sudo nano /etc/pf.anchors/demo
```
Paste this explicit filtering statement into the file space, then save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`):
```text
block in all
pass in proto tcp to port 5555 keep state
```
> 🧠 **Key Concept:** The `keep state` parameter commands the network card to track this session. Once it allows an incoming request through port 5555, it automatically allows the outbound response to pass back through without a second rule.

### Step 5: Load and initialize rule definitions
Load the custom anchor rules definition list and explicitly trigger the engine:
```bash
sudo pfctl -a demo -f /etc/pf.anchors/demo
sudo pfctl -e
```

### Step 6: Verify connection tracking stability
Repeat the test from Steps 1 and 2. The connection succeeds completely because the rule explicitly passes port 5555 traffic and tracks its operational state continuously.

### Step 7: Transition to stateless validation
Re-open the layout workspace file (`sudo nano /etc/pf.anchors/demo`) and adjust the rule layout to drop memory capabilities:
```text
block in all
pass in proto tcp to port 5555 no state
```
Reload your runtime settings configuration:
```bash
sudo pfctl -a demo -f /etc/pf.anchors/demo
```
> 🧠 **Key Concept:** Changing the instruction parameter to `no state` shifts the firewall into a stateless layout. The literal absence or inclusion of the `state` keyword is the visible syntax switch between these two architectures.

### Step 8: Post-Lab Environment Cleanup
Disable the rule engine backend fully to restore your machine to normal operating states:
```bash
sudo pfctl -d
```
