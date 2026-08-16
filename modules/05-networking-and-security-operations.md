# Day 5: Networking & Security Operations

## 🌐 Networking Fundamentals
* **Definition:** Network is the framework that allows one computer or device to communicate with another.
* **Endpoints:** Client-facing terminal machines (e.g., laptops, mobile devices, servers) that initiate or receive network communication.
* **Nodes:** Connection points or redistribution hubs within a network infrastructure (e.g., switches, routers).
* **The OSI Model Scope:** While tracking all 7 layers, security operations focus heavily on **Layer 3 (Network Layer)** for IP addressing and **Layer 4 (Transport Layer)** for TCP/UDP traffic management.
* **IP Address Types:** 
  * **Public IP:** Unique globally across the internet.
  * **Private IP:** Reusable locally within internal home or office environments.
* **Core Network Services:** 
  * **DNS (Domain Name System):** Translates human-readable domain names into numerical IP addresses.
  * **DHCP (Dynamic Host Configuration Protocol):** Automatically assigns dynamic IP parameters to connecting endpoints.


### 🏷️ IP Addressing Schemas

| IP Type | Structural Function | Behavioral Property |
| :--- | :--- | :--- |
| **Public IP** | Completely unique across the entire global Internet landscape. | Can be **Static** (permanent manual allocation) or **Dynamic** (temporary rotation). |
| **Private IP** | Used locally inside separate home or office environments. | Many networks reuse the exact same local schemas without conflict because they are logically isolated. |

---

## 🗺️ The OSI Model Reference Layering
A 7-layer logical blueprint mapping out how data moves across a structural network space:

* **Layer 7: Application Layer** — User interaction interface handling software communication (e.g., WhatsApp, TikTok).
* **Layer 6: Presentation Layer** — Coordinates file formatting syntax (e.g., `.pdf`, video files) and manages data encryption.
* **Layer 5: Session Layer** — Creates, tracks, and maintains stable connections between localized host endpoints.
* **Layer 4: Transport Layer** — Manages end-to-end transport mechanics by breaking large raw data strings down into structured **segments** for safe transfer.
* **Layer 3: Network Layer** — Handles logical routing over networking cabling, Wi-Fi paths, or ISP infrastructures (e.g., MTN / Vodafone). 
  * *Note:* Internet Service Providers (ISPs) rely on **DHCP** servers deployed at this layer to dynamically allocate IP schemas.
* **Layer 2: Data Link Layer** — Coordinates physical hardware mapping via a factory-assigned **MAC Address** permanently stamped on device network interface components.
* **Layer 1: Physical Layer** — Evaluates the core physical transport medium, processing raw electrical cables or radio frequencies.

### 🔍 Practical Command Reference
* **DNS (Domain Name System):** Resolves human-readable alphanumeric strings into machine-routable IP strings.
* **Terminal Syntax:** Use this diagnostic control command inside a command shell to query DNS mapping targets:
  ```bash
  nslookup google.com
  ```

---

## 🧮 Practical Subnetting & Network Segmentation
> **Subnetting:** Segmenting a broad, single network footprint into tight, isolated pieces for simplified administrative control and departmental boundary management.

### 📌 CIDR Mask Block Halving Principle on the last octet
* `/24` = 256 Total Allocations
* `/25` = 128 Allocations
* `/26` = 64 Allocations
* `/27` = 32 Allocations
* `/28` = 16 Allocations
* `/29` = 8 Allocations

### 📌 CIDR Mask Block Halving Principle on the second to last octet
* `/16` = 256 Total Allocations
* `/17` = 128 Allocations
* `/18` = 64 Allocations
* `/19` = 32 Allocations
* `/20` = 16 Allocations
* `/21` = 8 Allocations

### 🖊️ Class Exercise Solutions
#### Group Class Exercise: Split `10.0.0.0/24` into 2 subnets
**Subnet mask** is 255.255.255.128
* Each block uses a `/25` mask constraint providing exactly 128 allocations per subnet:
  * **Subnet A:** `10.0.0.0` --- `10.0.0.127`
  * **Subnet B:** `10.0.0.128` --- `10.0.0.255`
  
#### Exercise 1: Divide `192.168.1.0/24` into 4 equal subnets
**Subnet mask** is 255.255.255.192
Each split uses a `/26` mask constraint to divide the 256 address block into 64 - 2(network & broadcast) reusable hosts:
* **Subnet A:** `192.168.1.0` to `192.168.1.63`
* **Subnet B:** `192.168.1.64` to `192.168.1.127`
* **Subnet C:** `192.168.1.128` to `192.168.1.191`
* **Subnet D:** `192.168.1.192` to `192.168.1.255`

#### Exercise 2: Segment `172.16.0.0/16` into 8 subnets
**Subnet mask** is 255.255.224.0
* **Subnet A (/19):** `172.16.0.0` to `172.16.31.255`
* **Subnet B (/19):** `172.16.32.0` to `172.16.63.255`
* **Subnet C (/19):** `172.16.64.0` to `172.16.95.255`
* **Subnet D (/19):** `172.16.96.0` to `172.16.127.255`
* **Subnet E (/19):** `172.16.128.0` to `172.16.159.255`
* **Subnet F (/19):** `172.16.160.0` to `172.16.191.255`
* **Subnet G (/19):** `172.16.192.0` to `172.16.223.255`
* **Subnet H (/19):** `172.16.224.0` to `172.16.255.255`

## 🏁 The Sequencing Challenge
When an endpoint boots up and attempts to access a service on the internet, protocols must be executed in this exact operational order:
1. **DHCP:** The host broadcasts to obtain a local IP address configuration.
2. **DNS:** The host queries a name server to translate the destination domain name into a routable IP target.
3. **Connect:** The host initiates the actual transport connection (TCP/UDP handshake) to send and receive data.

---

## 🏢 Real-World Networking Incidents

### Case 1: Facebook's 2021 Global Outage
* **The Root Cause:** A severe routing failure. 
* **The Impact:** BGP (Border Gateway Protocol) configuration errors disconnected Facebook’s data centers from the internet, breaking availability worldwide.

### Case 2: Small Office Malware Spread
* **The Root Cause:** Flat network architecture (complete absence of subnetting or internal segmentation).
* **The Impact:** When a single local machine was infected, the malware spread horizontally across the entire office network unhindered, maximizing the blast radius.

---

## 📝 Assessment Reference
* **Format:** Reference material.
* **Milestone:** Review thoroughly before the **Week 2 MCQ Exam**.
