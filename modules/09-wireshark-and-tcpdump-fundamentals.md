# Day 9: Wireshark & Tcpdump Fundamentals

## Introduction to Wireshark & Installation Setup
* **What is it?:** Wireshark is a powerful graphical network protocol analyzer that allows you to inspect and evaluate live or pre-recorded network traffic. If a computer terminal visits a website, initiates an IP connection, queries a DNS server, or communicates with another device, Wireshark logs and displays that transaction packet by packet.
* **Official Download Resource:** Acquire installers exclusively from the official portal at [wireshark.org/download](https://wireshark.org).
  * *Windows:* Select the standalone `.exe` installer.
  * *macOS:* Select the logical volume `.dmg` disk image.
  * *Linux:* Deploy directly via native package managers (e.g., `sudo apt install wireshark`).

### Step-by-Step Installation Process
1. Open the verified local system installer package. https://www.wireshark.org/download.html 
2. Advance sequentially through the baseline setup configuration wizard.
3. Review and formally accept the software license agreement parameters.
4. Leave all default core utility components checked and selected.
5. **Critical Driver Prompt:** When prompted to configure **Npcap**, check the box to allow its installation. This engine is mandatory because it grants Wireshark deep access to capture raw network packets.
6. Finalize the installation process, restart the interface if prompted, and launch the Wireshark console workspace.

---

## Analyzing the Wireshark Home Screen Interface

When launching Wireshark prior to opening a workspace packet stream, the landing dashboard provides the primary ingestion controls split into three structural operational zones:

### 1. The Open / Recent Files Panel
* **Purpose:** Provides a rapid tracking workspace link log of historical packet inspection sessions (`.pcap`, `.pcapng`, or `.cap` extensions).
* **Lab Context:** Displays your specific class files, such as your upcoming exercise target: `/Users/elped/Downloads/Cybersecurity/Day 10/2026-08-09-traffic-analysis-exercise.pcap`.

### 2. The Capture Filter Ingestion Bar
* **Purpose:** Located directly beneath the capture header (`...using this filter:`). This input box handles initial traffic optimization configuration rules.
* **Syntax Rule:** This field strictly accepts **BPF (Berkeley Packet Filter)** configurations to discard unneeded protocol noise before saving raw frame bits to the local system storage space.

### 3. The Interface Monitor & Sparkline Activity Graph
* **Purpose:** Dynamically lists every internal and external hardware connection adapter currently accessible to the host machine's capture drivers (`Npcap`/`Libpcap`).
* **Key Components:**
  * **Interface Labels:** Identifies hardware adapter names (e.g., `Wi-Fi: en0` for the primary wireless network card, or `Loopback: lo0` for localized host testing).
  * **Traffic Sparklines:** The jagged waveform line charts displayed next to each adapter label provide a real-time, visual activity pulse indicator. An active waveform confirms live packets are moving across that hardware boundary, showing the analyst exactly which network interface card should be selected to initiate a live capture session.
<br>
<br>
<img width="1210" height="901" alt="Wireshark Interface" src="https://github.com/user-attachments/assets/8ef3be73-0d04-4e87-8a50-906fcd36004d" />

---

## Packet Capture Architecture & Engines
* **Packet Definition:** A small, structured unit of data transmitted across a routing computer network.
* **Core Interception Mechanics:** Packet capturing is the active interception and recording of network traffic at the networking layer.
* **Driver Architecture:** Wireshark does not capture raw data independently. Instead, it interfaces directly with low-level capture engines to intercept raw streams directly from the network interface card (network adapter):
  * **Windows Environment:** Communicates via the **Npcap** driver packet sniffing engine.
  * **macOS / Linux Environments:** Communicates via the **Libpcap** library backend.
* **Security Constraints:** Because capturing network traffic requires tapping directly into the network hardware interface layers, initiating a capture session strictly **requires administrative/root privileges**.

---

## Core Wireshark Interface Navigation

Wireshark breaks down individual packets using three distinct workspace panes:

1. **Packet List Pane (Top):** Displays a row-by-row sequence of captured packets, summarizing metrics like time, source/destination IP, protocol, and basic info parameters.
2. **Packet Details Pane (Middle):** Provides an expandable tree structure mapping the packet field by field across the network architecture layers, from Ethernet frames up through the application layer. Clicking on a row maps the details directly to this view.
3. **Packet Bytes Pane (Bottom):** Shows the raw, unedited hexadecimal and ASCII content of the packet, allowing analysts to verify exact data payloads manually.

> **Color Coding Insight:** Wireshark automatically applies colors to rows to highlight specific protocols or anomalies. Black and red lines typically indicate network anomalies like packet retransmissions or malformed data, rather than guaranteed malicious activity.
<br>
<img width="1210" height="901" alt="Wireshark Panes" src="https://github.com/user-attachments/assets/1b9f1e7e-92f2-42b2-854d-6d793160ea97" />

---

## Filter Engineering: Capture vs. Display Filters

| Parameter | Capture Filters | Display Filters |
| :--- | :--- | :--- |
| **Underlying Language** | Utilizes strict **BPF (Berkeley Packet Filter)** syntax engines. | Utilizes precise **protocol field syntax** references. |
| **Execution Time** | Applied **before** the capture session begins. | Applied **after** the capture session completes. |
| **Data Preservation** | Non-matching traffic is **permanently discarded** by the network interface and cannot be recovered. | Merely hides or shows traffic already recorded; zero data is lost, and filters can be altered freely. |
| **Syntax Example** | `host 192.168.1.10` | `ip.addr == 192.168.1.10` |
| **Common Pitfall** | Using display filter syntax (like `==`) inside the capture filter box causes an instant engine error. | Forgetting double equal signs (`==`) or missing dots between field properties (e.g., typing `ipaddr`) turns the filter bar pink. |

---

## Display Filters vs. TCP Stream Following
* **Display Filtering:** Limits your current pane view to isolated packets matching a specific search parameter (e.g., matching a particular protocol or IP address). It does not piece conversations together; it only displays the isolated packets matching that flag.
* **Following a TCP Stream:** Right-clicking an IP packet and selecting `Follow -> TCP Stream` actively strips out transport noise and reconstructs the entire fragmented conversation between two endpoints into a single, cohesive, human-readable text block. 

Understanding how to transition from individual packet views to reconstructed structural network text sessions is a mandatory skill for alert triage:

```text
+-------------------------------------------------------------------------+
|                                                                         |
|                          THE PANE VIEW DIFFERENCE                       |
+-------------------------------------------------------------------------+
|                                                                         |
| DISPLAY FILTERS (e.g., "tcp")        | FOLLOW TCP STREAM                |
| - Shows isolated, individual rows.   | - Reconstructs whole sessions.   |
| - Chronological time snapshot.       | - Merges fragments into text.    |
| - Excellent for scanning traffic tags| - Excellent for reading code,    |
|   and network flags (SYN/ACK).       |   chat logs, and data transfers. |
+-------------------------------------------------------------------------+
```

* **How to run Display Filters:** Type the target protocol (e.g., `tcp`) into the top entry bar and hit Enter. The interface highlights only matching rows, leaving the remaining streams hidden.
* **How to run TCP Streams:** Locate any packet in a sequence, **Right-Click -> Follow -> TCP Stream**. A separate pop-up text window initializes, displaying the full dialogue exchange between the client and server.
<br>
<blockquote><em>Filter by tcp</em></blockquote>
<img width="1210" height="901" alt="Screenshot 2026-08-22 at 2 18 15 AM" src="https://github.com/user-attachments/assets/659c7826-1f54-47f5-841d-8f1d82d9663c" />
<br>
<blockquote><em>Right click on one packet hover over Follow and click TCPstream</em></blockquote> 
<img width="1210" height="901" alt="Screenshot 2026-08-22 at 2 19 35 AM" src="https://github.com/user-attachments/assets/87d38c2d-dcfe-4531-b353-03c9ecd70a2b" />
<br>
<blockquote><em>Separate pop-up window with full dialogue btn client and server shows up <br>
Note: You can also change the format of the dialogue using **Show as ** option at the bottom*</em></blockquote>
<img width="625" height="608" alt="Screenshot 2026-08-22 at 2 21 21 AM" src="https://github.com/user-attachments/assets/b686f662-74a0-4290-b949-64c46b0681b0" />

<!-- <img width="1210" height="901" alt="Screenshot 2026-08-22 at 2 18 58 AM" src="https://github.com/user-attachments/assets/41fc4425-84cc-4ddf-ba81-45e4a56ed950" />-->

---

### Lab Reflection: HTTP vs. Follow Stream
* **The HTTP Filter:** Narrowed the busy packet list pane down to show row summaries of cleartext web traffic, explicitly highlighting resource path lookups like `GET /index.html` inside the Info column.
* **The Follow TCP Stream Action:** Revealed the full hidden conversational data exchange, exposing complete raw HTTP request headers, browser User-Agent strings, cookie tokens, and the raw text response returned by the server.

### 2. The HTTP Filter vs. Following the HTTP Stream
When investigating unencrypted web activity, an analyst must understand what metadata is exposed at each analysis stage:

* **Applying the `http` Filter:** This string narrows down the top pane view to isolate web traffic, showing raw summaries in the Info column (such as `GET /index.html HTTP/1.1`). It gives you a quick checklist of *what* web resources were requested across the timeline.
* **Executing "Follow TCP Stream" on an HTTP Packet:** This bypasses the single-line row summary entirely to print the raw application-layer headers and content payloads. 
  * **What it exposes:** The full browser configurations via the `User-Agent` string, tracking tokens inside the `Cookie:` fields, language constraints, host headers, and the literal server output (such as HTML code, text documents, or hidden malware download code).

<br>
<blockquote><em>Filter by tcp*</em></blockquote>
<img width="1210" height="901" alt="Screenshot 2026-08-22 at 2 33 54 AM" src="https://github.com/user-attachments/assets/4dd306c5-d5f3-4e16-9c2c-bc976f1b172e" />
<br>
<blockquote><em>Right click on one packet hover over Follow and click TCPstream*</em></blockquote>
<img width="1210" height="901" alt="Screenshot 2026-08-22 at 2 35 47 AM" src="https://github.com/user-attachments/assets/ae245103-0e57-42ee-91ed-c25c34bf2c0d" />
<br>
<blockquote><em>Separate pop-up window with full dialogue btn client and server shows up <br>
Note: You can also change the format of the dialogue using **Show as ** option at the bottom </em></blockquote>
<img width="1210" height="901" alt="Screenshot 2026-08-22 at 2 36 58 AM" src="https://github.com/user-attachments/assets/62bee899-91c5-40bd-b414-1305c4d48daa" />

### 3. Follow TCP Stream vs. Follow HTTP Stream
When dealing with HTTP traffic, Wireshark exposes two different "Follow" rendering engines:

* **Follow TCP Stream:** Focuses on the Transport Layer (Layer 4). It outputs the raw data exactly as it flew across the wires. If the web server used compression (`Content-Encoding: gzip`), the text in the window will display as unreadable binary clutter.
* **Follow HTTP Stream:** Focuses on the Application Layer (Layer 7). Wireshark strips away transport headers and acts like a mini web browser. It automatically decompress zipped web packets to output clean, readable HTML text or script data.

Inspecting Packet Details: expanding a single packet in the lower pane to see every header field, from Ethernet up through the application layer.
<img width="716" height="21" alt="image" src="https://github.com/user-attachments/assets/004da85a-41c1-40d4-88e1-a58af8a756f5" />

---

## Independent Practice Task Solutions

Review notes and behavioral descriptions recorded after clearing the filter workspace and downloading targets from the official repository (`wiki.wireshark.org/SampleCaptures`):

### 1. `dns` (Domain Name System) Filter
* **Observed Traffic Data:** Displays transactions mapping application layer requests to port 53.
* **Analytical Description:** This traffic captures your local endpoints communicating with name servers to look up and translate alphanumeric web domain URLs into machine-routable numerical IP addresses.

### 2. `tcp` (Transmission Control Protocol) Filter
* **Observed Traffic Data:** Displays dense transport layer streams tracking sequence flags (SYN, ACK, FIN).
* **Analytical Description:** This traffic highlights the structural backbone of reliable network communication, showing the operational connection handshakes and packet tracking states used to move application data smoothly.

### 3. `arp` (Address Resolution Protocol) Filter
* **Observed Traffic Data:** Displays local layer 2 broadcasts asking "Who has this IP?" to find hardware locations.
* **Analytical Description:** This traffic shows devices announcing or querying the local physical network map to translate a known software Layer 3 IP address into a unique physical Layer 2 MAC address stamped on a network interface card.

---

## 🏢 Real-World Packet Analysis Context
* **The Stuxnet Discovery (2010):** Security analysts studying infected industrial networks relied heavily on patient packet and protocol analysis to discover the worm's hidden lateral movement and unique targeting vectors.
* **SOC Operations:** Modern Security Operations Centers pull packet captures to confirm or reject SIEM alerts, since raw packet payloads tell the final truth about network activity.

---

## 📝 Assessment Reference
* **Format:** Reference material for hands-on filter proficiency.
* **Milestone:** Prepares for the upcoming **Week 3 MCQ Quiz on Day 4** and the **Day 2 Wireshark Investigation Lab**.





## Additional Materials
* LetsDefend - TCPdump(Not started)
* LetsDefend - Wireshark (In progress)
