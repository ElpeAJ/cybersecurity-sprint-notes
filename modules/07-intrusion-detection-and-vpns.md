<!--<img width="699" height="469" alt="Screenshot 2026-08-16 at 5 09 31 PM" src="https://github.com/user-attachments/assets/bd445832-3d2b-4356-a9e8-d5abb37ab968" />-->
# Day 7: Intrusion Detection & VPN Implementations

## Perimeter Monitoring: IDS vs. IPS Architectures

Intrusion Detection Systems (IDS) and Intrusion Prevention Systems (IPS) are automated security solutions deployed to audit network traffic behavior:

* **Intrusion Detection System (IDS):** An inline monitoring utility that analyzes packet flows and fires an administrative warning upon spotting anomalies or threats, but **does not** modify traffic directly. (Primary Role: Traffic Visibility).
* **Intrusion Prevention System (IPS):** An inline reactive utility situated directly within the traffic pipeline that actively interrupts, drops, or blocks malicious activity automatically.

### Operational Analogy Matrix

| Parameter | Smoke Alarm (IDS) | Sprinkler System (IPS) |
| :--- | :--- | :--- |
| **Core Action** | Detects threat indicators and triggers an alarm. Takes zero direct action on the fire. | Detects threat thresholds and discharges water instantly without human intervention. |
| **Response Agent** | A human analyst must review the alert context to decide on the mitigation. | The system self-executes a programmatic response with no human-in-the-loop delay. |
| **False Positive Cost** | Generates temporary noise. Mildly irritating but zero asset damage occurs. | Inflicts actual water damage to healthy property. The remediation action itself carries operational cost. |

---

## Detection Logic: Signatures vs. Anomalies

* **Signature-Based Detection:** References incoming files or traffic strings against a local database of known threat fingerprints or malicious patterns.
  * *Analogy:* A security guard holding a physical poster displaying faces of documented criminals.
* **Anomaly-Based Detection:** Models a strict behavioral "baseline" of normal system traffic over a learned period. Anything that deviates significantly from this baseline flags a violation.
  * *Analogy:* A seasoned guard who identifies suspicious, out-of-character behavior without relying on a pre-printed list.

---

## Virtual Private Network (VPN) Architecture

A VPN abstracts your traffic by establishing an encrypted connection over an untrusted network environment to a remote transit server, mapping directly back to the **Confidentiality** pillar from Week 1.

### The 4 Core VPN Vocabulary Terms
1. **Tunnel:** The secure, logical channel created across the public internet to isolate your data stream.
2. **Encryption:** Cryptographically encoding the contents of your packets to prevent interception or sniffing on public networks.
3. **Endpoint:** The connection termination units on either side of the tunnel (e.g., your local machine and the remote Proton VPN server).
4. **Handshake:** The initial negotiation sequence where the two endpoints authenticate each other and securely exchange session encryption keys.

---

## Real-World Incidents & Case Studies

### Case 1: Marriott Breach (2018)
* **The Vulnerability:** An **Anomaly Detection Failure**.
* **The Impact:** Malicious actors remained inside the network completely undetected for **4 years**, slowly exfiltrating millions of guest records because internal behavior tracking failed to flag the abnormal data movements.

### Case 2: Pulse Secure Breach (2019)
* **The Vulnerability:** A critical software flaw in a corporate VPN system.
* **The Impact:** Threat actors weaponized the VPN vulnerability directly as their entry attack path, bypassing traditional outer perimeter defenses to exploit corporate internal assets.

---

## 💻 Practical Labs Walkthrough

### Part 1: Simulating Detection & Prevention Scripts

#### Step 1: Create a mock traffic log file
Open your local terminal and use an EOF block to write a dummy log dataset tracking traffic ports and payloads:
```bash
cat << 'EOF' > ~/Desktop/traffic.log
09:14 10.0.0.15 443 12KB
09:15 10.0.0.15 443 8KB
02:47 10.0.0.22 4444 2MB
09:20 10.0.0.18 22 1KB
EOF
```

#### Step 2: Simulate an IDS warning logic
Search the log space for anomalous port activity (`4444`). The script echoes an alert to the terminal panel without modifying the source log or tracking file:
```bash
grep "4444" ~/Desktop/traffic.log && echo "ALERT: suspicious port 4444 detected, notifying analyst"
```

#### Step 3: Simulate an IPS automated blocking routine
Execute the detection logic, but append an active structural change that drops malicious packets by feeding the offending IP string directly to the macOS firewall block table:
```bash
grep "4444" ~/Desktop/traffic.log && echo "BLOCKED: adding 10.0.0.22 to firewall deny list" && sudo pfctl -t blocklist -T add 10.0.0.22
```

---

### Part 2: Guided Tunnel Configuration via WireGuard & Proton VPN
### 💡 Lab Concept: How Our Setup Actually Works
Instead of downloading and installing the full Proton VPN desktop software application onto the PC, we split the service provider parameters from the connection engine:
1. **The Configuration File:** We log into Proton VPN online to generate a specialized configuration file (`.conf`). This file acts as our permission slip and mapping guide to use Proton's secure global infrastructure.
2. **The WireGuard Engine:** We feed this configuration file directly into the WireGuard client. WireGuard reads the file instructions to build the tunnel and change our virtual location whenever we activate it.
3. **The Activation Verification:** We navigate to a public address inspector site (like `WhatIsMyIP`) before and after clicking activate to visually confirm our public network footprint has safely shifted to a remote server.

> Simply put, in actuality the Proton VPN is not Working on my PC because I haven't downloaded it. So I created a configuration file so that I will give it to WireGuard, then WireGuard will use the configuration to change my location as and when I activate it. Then I will use the WhatIsMy IP site to check the IP Address on the net as and when I activate it?

#### Step 1: Secure Server Configuration Provisioning
1. Authenticate into your personal **Proton VPN** dashboard account.
<img width="282" height="305" alt="Screenshot 2026-08-16 at 4 38 05 PM" src="https://github.com/user-attachments/assets/9d9c11b7-d441-409d-bbd0-7ab1ef872f71" />
<p>2. Select your targeted operational platform deployment option (e.g., Windows, macOS, or Linux).</p>
<img width="940" height="556" alt="Screenshot 2026-08-16 at 4 38 50 PM" src="https://github.com/user-attachments/assets/6c52b618-3544-44eb-b0cb-c0f20794b687" />
<p>3. Generate and download the customized cryptographic connection profile (`.conf` file configuration string).</p>
<img width="804" height="698" alt="Screenshot 2026-08-16 at 4 40 11 PM" src="https://github.com/user-attachments/assets/8310a132-4fac-4fe9-9edd-f5a0213e4df4" />

#### Step 2: Core WireGuard Integration
1. Initialize the open-source **WireGuard** interface console client.
<img width="699" height="469" alt="Screenshot 2026-08-16 at 5 09 59 PM" src="https://github.com/user-attachments/assets/38105ca3-f296-4207-bc87-7006bb39adb5" /> <br><br>

<p>2. Select **Import Tunnel(s) from File** and point it directly to the downloaded Proton VPN `.conf` profile.</p>
<!--<img width="699" height="337" alt="Screenshot 2026-08-16 at 5 11 37 PM" src="https://github.com/user-attachments/assets/b4f2fb69-871e-44db-a734-152570e85b0d" />-->
<img width="699" height="337" alt="Screenshot 2026-08-16 at 5 12 43 PM" src="https://github.com/user-attachments/assets/69c99fba-e990-49c6-80f2-f50f5e8d1b04" /><br><br>

3. Toggle the active routing switches to enable the internal DNS mapping protection parameters.
<img width="699" height="520" alt="Activating WireGuard Tunnels" src="https://github.com/user-attachments/assets/afcdba02-5892-4764-a372-d7a67b574658" /><br>

<img width="699" height="520" alt="Wireguard Tunnel Activation" src="https://github.com/user-attachments/assets/6bf5eeb5-c7db-4347-b0a2-c3695953201c" /><br><br>

#### Step 3: Verifying Traffic Redirection Boundaries

```text
====================================================================
METRIC               PRE-TUNNEL RESCUE          POST-TUNNEL ENCRYPTED
====================================================================
Public IP Address    [Record Local ISP IP]      [Record Remote Tunneled IP]
Geographic Location  [Your True City/Country]   [Target Server Country Location]
Traffic Route        Exposed Local ISP          Encrypted WireGuard Pipeline
====================================================================
```
* **Verification Step:** Navigate to a public address inspector site (e.g., `WhatIsMyIP`) before and after enabling WireGuard to ensure your real geographic origin is completely masked.
<img width="1051" height="420" alt="Screenshot 2026-08-16 at 5 25 46 PM" src="https://github.com/user-attachments/assets/dedefffc-a57f-4b7e-b65d-69cead7dc081" /><br><br>

<p><b>Before Activation</b></p>
<img width="1196" height="570" alt="Before Activation" src="https://github.com/user-attachments/assets/31e648d7-d6d5-4a64-8d66-378b132d6352" /><br><br>

<p><b>After Activation</b>(reload browser to see new IP details)</p>
<img width="1196" height="570" alt="After Activation" src="https://github.com/user-attachments/assets/db64ce35-b6bc-4b28-847c-cdf723526009" /><br>

---

## Additional Materials
* TryHackMe - Snort (2 hrs - Not started)

---

## 📝 Assessment Reference
* **Format:** Semi-guided to Independent review template.
* **Milestone:** No immediate standalone quiz today, but this material directly feeds into the upcoming **Week 2 MCQ Exam**.

<!--<img width="1051" height="420" alt="Screenshot 2026-08-16 at 5 26 30 PM" src="https://github.com/user-attachments/assets/d7723bdc-d64c-40f6-8a2c-778cbd32795b" />-->


<img width="845" height="533" alt="Screenshot 2026-08-21 at 10 49 30 PM" src="https://github.com/user-attachments/assets/f4347fbc-467e-40ad-aadb-8b60b9bebe98" />

<img width="845" height="826" alt="Screenshot 2026-08-21 at 10 50 04 PM" src="https://github.com/user-attachments/assets/fea6fb41-2f7d-4519-ace3-13166d13fea4" />

<img width="845" height="807" alt="Screenshot 2026-08-21 at 10 51 02 PM" src="https://github.com/user-attachments/assets/2be39d82-667b-4f83-aa25-f95e5395331b" />





