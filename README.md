# Basic-Office-Network
Small office network design simulation in Cisco Packet Tracer combining secure NAS storage and a repurposed utility server.# Network Design Simulation - 
---
Phase 1: Secure Local Storage Foundation
---

This repository documents the architecture, configuration, and validation of an on-premise local network infrastructure designed for a basic small office environment. Phase 1 focuses entirely on deploying a secure, centralized storage architecture using Cisco Packet Tracer.

## 📌 Project Overview
The objective of Phase 1 is to establish a secure data vault that consolidates company files, implements user access control, and ensures internal data integrity. This replaces insecure local workstation storage with a centralized Network-Attached Storage (NAS) model.

## 🏗️ Topology Architecture
The network uses a star topology where all endpoint traffic is centralized through a physical layer-2 switch.

[ 2960 Local Network Switch ]
/                         
/                           
[ Smart_NAS (Server) ]          [ Workstation (PC) ]IP: 192.168.1.50                
IP: 192.168.1.10 Role: FTP Storage Vault         
Role: Local Client End-Device

### Hardware Components Emulated:
*   **Switch:** 1x Cisco Catalyst 2960 (24-Port Layer 2 Switch)
*   **Storage Server (NAS):** 1x Generic Server Configured for FTP Services
*   **Client Node:** 1x Generic PC Workstation
*   **Cabling:** Copper Straight-Through (Ethernet)

---

### 🛠️ How to Simulate Smart NAT in Packet Tracer
<img width="883" height="479" alt="image" src="https://github.com/user-attachments/assets/31d6defe-8836-4a88-9940-96eee16ff168" />

To build this, open Packet Tracer and select the End Devices category (the icon looks like a desktop computer) in the bottom-left corner.
1. Simulating Device 1: The Smart NAS
Packet Tracer doesn't have an explicit icon labeled "NAS," but it has a generic Server device that handles storage protocols perfectly.
* Add the Device: Drag a generic Server onto your workspace and rename it Smart_NAS.
  <img width="1893" height="726" alt="image" src="https://github.com/user-attachments/assets/f3ac6528-f6b7-4f79-809f-48a9a454d601" />

* Configure Network Settings: Click on the server, go to the Desktop tab, open IP Configuration, and give it a Static IP address (e.g., 192.168.1.50, Subnet Mask: 255.255.255.0, Gateway: 192.168.1.1).
  <img width="1133" height="693" alt="image" src="https://github.com/user-attachments/assets/426a3c45-6eb3-4429-a9cc-2fbdce13531b" />

* Turn on Storage Services: Go to the Services tab.
     * Turn on the FTP service. This lets you simulate uploading and downloading company files.
     * You can create a few mock user accounts with specific permissions (Read, Write, Delete) right inside the FTP service settings.
       <img width="1146" height="582" alt="image" src="https://github.com/user-attachments/assets/4d58eb3b-0d1f-4e51-8db6-03cf32a793e7" />

## 💻 Step 1: Add and Cable the WorkstationLook at the bottom-left corner of Packet Tracer, click on End Devices (the computer icon), and select the generic PC or Laptop.Drag it onto your workspace near your network switch.Select Connections (the lightning bolt icon), click on the Copper Straight-Through cable (the solid black line).Click on your new PC, choose FastEthernet0, then click on your Network Switch and choose any available port (e.g., FastEthernet0/3).
## ⚙️ Device Configurations

### 1. Network Addressing Schema (Static IPv4)
To ensure consistent network routing and avoid IP lease expiration issues, all core devices are assigned static IP addresses within the `192.168.1.0/24` subnet.

| Device Name | Interface | IP Address | Subnet Mask | Default Gateway |
| :--- | :--- | :--- | :--- | :--- |
| `Smart_NAS` | FastEthernet0 | **192.168.1.50** | 255.255.255.0 | 192.168.1.1 |
| `PC-A` | FastEthernet0 | **192.168.1.10** | 255.255.255.0 | 192.168.1.1 |

### 2. NAS Storage & Access Control Settings (FTP Service)
The File Transfer Protocol (FTP) engine on the `Smart_NAS` was activated to simulate network file management. Access control lists (ACLs) were simulated via role-based user permissions.

*   **Service Status:** Enabled (ON)
*   **User Account Configured:**
    *   **Username:** `manager`
    *   **Password:** `Pass123!`
    *   **Active Privileges:** Read (**R**), Write (**W**), Delete (**D**)
    *   **Restricted Privileges:** Rename (**N**), List (**L**) *[Note: List can be toggled depending on absolute obscurity requirements]*

---

## 🧪 Verification & Testing Procedures

To validate the deployment, end-to-end integration tests were performed from the workstation command line interface (CLI).

### Test 1: Layer 3 ICMP Connectivity (Ping)
Executed to ensure the client machine could successfully reach the NAS across the switch fabric.
```bash
PC> ping 192.168.1.50

Pinging 192.168.1.50 with 32 bytes of data:
Reply from 192.168.1.50: bytes=32 time=1ms TTL=128
Reply from 192.168.1.50: bytes=32 time=1ms TTL=128

Ping statistics for 192.168.1.50:
    Packets: Sent = 4, Received = 4, Lost = 0 (0% loss)
```
*   **Result:** **SUCCESS**. Physical cabling and IP configurations are fully operational.

### Test 2: Active Directory Simulation (FTP Authentication & Write)
A test payload file named `test.txt` was generated locally using the workstation Text Editor. An FTP connection was initiated to verify authorization boundaries and file system ingest capabilities.

```bash
PC> ftp 192.168.1.50
Connected to 192.168.1.50.
220 Welcome to Cisco Packet Tracer FTP server
User: manager
331 Password required for manager
Password: 
230 User manager logged in.

ftp> put test.txt
200 PORT command successful.
150 Opening BINARY mode data connection for test.txt
226 Transfer complete.
```
*   **Result:** **SUCCESS**. The `manager` account successfully authenticated, and the network successfully permitted the **Write** payload execution.

### Test 3: Data Retrieval Validation (FTP Read)
Verified that data stored in the central repository can be successfully called down back to local endpoints.
```bash
ftp> get test.txt
200 PORT command successful.
150 Opening BINARY mode data connection for test.txt
226 Transfer complete.
```
*   **Result:** **SUCCESS**. Read privileges verified.

---

## 🏁 Phase 1 Conclusion
Phase 1 confirms that the local network fabric is correctly configured to support secure, authenticated file transfers over a local area network (LAN). Data isolation and access control mechanisms behave as predicted by design specifications.

***
**Next Milestone:** Phase 2 - Deploying a Repurposed PC as a localized Intranet Web Server and internal Domain Name System (DNS) utility.
