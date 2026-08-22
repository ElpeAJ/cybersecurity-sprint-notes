# Day 9: Wireshark & Tcpdump Fundamentals

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

<img width="1210" height="901" alt="Wireshark Panes" src="https://github.com/user-attachments/assets/1b9f1e7e-92f2-42b2-854d-6d793160ea97" />

---

## ⚙️ Filter Engineering: Capture vs. Display Filters

| Parameter | Capture Filters | Display Filters |
| :--- | :--- | :--- |
| **Underlying Language** | Utilizes strict **BPF (Berkeley Packet Filter)** syntax engines. | Utilizes precise **protocol field syntax** references. |
| **Execution Time** | Applied **before** the capture session begins. | Applied **after** the capture session completes. |
| **Data Preservation** | Non-matching traffic is **permanently discarded** by the network interface and cannot be recovered. | Merely hides or shows traffic already recorded; zero data is lost, and filters can be altered freely. |
| **Syntax Example** | `host 192.168.1.10` | `ip.addr == 192.168.1.10` |
| **Common Pitfall** | Using display filter syntax (like `==`) inside the capture filter box causes an instant engine error. | Forgetting double equal signs (`==`) or missing dots between field properties (e.g., typing `ipaddr`) turns the filter bar pink. |

---

## 🛠️ Display Filters vs. TCP Stream Following
* **Display Filtering:** Limits your current pane view to isolated packets matching a specific search parameter (e.g., matching a particular protocol or IP address). It does not piece conversations together; it only displays the isolated packets matching that flag.
* **Following a TCP Stream:** Right-clicking an IP packet and selecting `Follow -> TCP Stream` actively strips out transport noise and reconstructs the entire fragmented conversation between two endpoints into a single, cohesive, human-readable text block. 

### 📁 Lab Reflection: HTTP vs. Follow Stream
* **The HTTP Filter:** Narrowed the busy packet list pane down to show row summaries of cleartext web traffic, explicitly highlighting resource path lookups like `GET /index.html` inside the Info column.
* **The Follow TCP Stream Action:** Revealed the full hidden conversational data exchange, exposing complete raw HTTP request headers, browser User-Agent strings, cookie tokens, and the raw text response returned by the server.

---

## 💻 Independent Practice Task Solutions

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
