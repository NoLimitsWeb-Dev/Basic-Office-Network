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

###  Let's Add and Cable the Workstation
💻 Step 1:
1. Look at the bottom-left corner of Packet Tracer, click on End Devices (the computer icon), and select the generic PC or Laptop.
2. Drag it onto your workspace near your network switch.
3. Select Connections (the lightning bolt icon), click on the Copper Straight-Through cable (the solid black line).
4. Click on your new PC, choose FastEthernet0, then click on your Network Switch and choose any available port (e.g., FastEthernet0/3).
   <img width="681" height="470" alt="image" src="https://github.com/user-attachments/assets/be26996e-c34e-4c51-b27f-87d3cfb9638b" />


###  Let's Give the Workstation an IP Address
🌐 Step 2:
Before the PC can talk to the NAS, it needs to be on the same network subnet:
1. Click on the new PC to open its options.
2. Go to the Desktop tab at the top and click on IP Configuration.
3. Ensure Static is selected.
4. Input the following settings:
   * IP Address: 192.168.1.10 (Since my  NAS is 192.168.1.50)
   * Subnet Mask: 255.255.255.0
   * Default Gateway: 192.168.1.1
   * Close the IP Configuration window.
  <img width="1137" height="663" alt="image" src="https://github.com/user-attachments/assets/047dc022-84c4-4e03-82c5-a1bec67bc6e6" />

### Create a Test File on the PC
📝 Step 3:
To test your Write to (Upload) permission, you need a local file on the laptop to send over to the NAS:
1. Still inside the PC's Desktop tab, scroll down and open the Text Editor app.
  <img width="682" height="599" alt="image" src="https://github.com/user-attachments/assets/9b5d32d4-08fd-4178-af97-653663ca0172" />

2. Type a short message inside the file (e.g., Hello this is a backup test).
3. Click File -> Save (or press Ctrl + S).
4. Name the file test.txt and click OK. Close the Text Editor.
   <img width="903" height="335" alt="image" src="https://github.com/user-attachments/assets/527df10f-ad2d-4678-a944-44e6a3f3df4d" />
---
### Let's add the switch right now to bridge your PC and your Smart_NAS.
In networking, computers and servers cannot plug directly into each other without a central junction box—that is what the Network Switch is for. It acts like a power strip for network cables, allowing all your local devices to exchange data.
### 🎛️ Step 1: Add the Network Switch
1. Look at the bottom-left menu of Packet Tracer and click on Network Devices (the icon looks like a router).
2. Just below that row, a sub-menu will appear. Click on Switches (the icon looks like a small rectangular box with arrows pointing left and right).
3. Select the 2960 switch model (this is a standard Cisco Catalyst 24-port switch).
4. Drag it into the center of your workspace, right between your PC and your Smart_NAS.
### 🔌 Step 2: Cable Everything to the Switch
Now we will run cables from your individual devices into the switch ports:
1. Click on the Connections icon (the lightning bolt) in the bottom-left corner.
2. Select the Copper Straight-Through cable (the solid black line).
3. Connect the PC: Click on your PC, select FastEthernet0, then click on the Switch and select FastEthernet0/1.
4. Connect the NAS: Click the cable tool again. Click on your Smart_NAS, select FastEthernet0, then click on the Switch and select FastEthernet0/2.

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

## 🧪 Verification: Run the Live FTP Test
Now let’s log in and push that file over to your simulated NAS:
1. Still inside the PC's Desktop tab, click to open the Command Prompt.
2. Type ping 192.168.1.50 (NAS IP) and press Enter. 
<img width="1146" height="386" alt="image" src="https://github.com/user-attachments/assets/54fe5715-1c61-46f5-9967-ed668cfe1da9" />

* Since I got successful replies. It means i don't need to check my cabling: Physical cabling and IP configurations are fully operational.

3. Type ftp 192.168.1.50 and press Enter.
4. It will say Welcome to Packet Tracer FTP server. Enter your mock username (manager) and password (Pass123!). Your prompt will change to ftp>.
<img width="417" height="187" alt="image" src="https://github.com/user-attachments/assets/afa29f36-c618-4303-b545-eb4ee3bef32c" />

5. Type put test.txt and press Enter.
<img width="411" height="144" alt="image" src="https://github.com/user-attachments/assets/c3a6ecb3-5bd4-489c-8266-68da73d4b1e8" />


This proves your Write permission is successfully active over your simulated network architecture.



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
<img width="582" height="121" alt="image" src="https://github.com/user-attachments/assets/45e5a607-3dda-45b3-8ce8-145b610718eb" />

---

## 🏠Let's Test My Security Restrictions
Before moving on, let's verify that our permission settings actually block unauthorized actions. This simulates real-world security access control:
1. The Unauthorized Command Test: Log back into the FTP server from your PC using your manager account. Try to run the rename command (e.g., rename test.txt backup.txt). Because you unchecked the Rename permission earlier, the server should explicitly deny you.
<img width="770" height="106" alt="image" src="https://github.com/user-attachments/assets/26a6e198-b994-413c-9046-e4de416bb007" />

*  **Result:** **ERROR**. Permission Denied
---
2. The Malicious User Test: Let's Go back to your Smart_NAS FTP service settings and create a second user account named guest with a password of 123. Check only the Read and List boxes (leave Write and Delete blank). Go to your PC, log in as guest, and try to run put test.txt. The network should block it, demonstrating how a NAS protects files from low-privilege users.
<img width="1139" height="541" alt="image" src="https://github.com/user-attachments/assets/3b3cbb34-038e-4521-8a3c-1585c13095a1" />
<img width="983" height="249" alt="image" src="https://github.com/user-attachments/assets/9ff9898c-8213-44b6-81ba-f37f26b3db4c" />

*  **Result:** **ERROR**. Permission Denied
## 🏁 Phase 1 Conclusion
Phase 1 confirms that the local network fabric is correctly configured to support secure, authenticated file transfers over a local area network (LAN). Data isolation and access control mechanisms behave as predicted by design specifications.

***
**Next Milestone:** Phase 2 - Deploying a Repurposed PC as a localized Intranet Web Server and internal Domain Name System (DNS) utility.
---
Let’s configure our Repurposed_PC Server to act as my internal local application and intranet server.
In this phase, we will turn on the HTTP/HTTPS services, customize the webpage text, and test the access from your client workstation.
---
### 🌐 Step 1: Customize Your Local Web App / PortalPacket Tracer servers have a mini HTML editor built into them, allowing you to simulate actual web applications:

1. Add a new PC and rename as Repurposed_PC
2. Click on your Repurposed_PC server to open its dashboard.
3. Go to the Services tab at the top.
4. Select HTTP from the left-hand column menu.
5. Ensure that both HTTP and HTTPS radio buttons are set to On.
6. Look at the file manager list below. Find the file named index.html and click the edit link on the far right of that row.
<img width="1129" height="565" alt="image" src="https://github.com/user-attachments/assets/bf0085ae-f3d3-43c6-97cf-e732d6c25d70" />

7. Delete everything inside the editor, or clear it out and copy-paste this simple custom company landing page:
<img width="1144" height="440" alt="image" src="https://github.com/user-attachments/assets/389c6274-7342-4aa9-b52b-e1e0d6e90a35" />

8. Click the Save button at the top right of the editor box, and click Yes to overwrite the existing file. Close the server window.
---

### 🧪 Step 2: Test the Intranet from Your Workstation
Now let's verify that the local client computer can pull up this web application across the network switch:
1. Click on your Workstation PC (the same one you used for the FTP test).
2. Go to the Desktop tab at the top.
3. Find and click on the Web Browser application icon.
4. In the URL bar at the top, type the IP address of your application server: 192.168.1.50 and press Enter (or click Go).
<img width="1130" height="447" alt="image" src="https://github.com/user-attachments/assets/3ccd542f-3cd1-44e8-a60a-8c89452101cd" />

Your customized webpage should immediately load up inside the simulation browser!

🧠 Step 3: Upgrading Phase 2 (Optional)

Right now, I have to type the clunky IP address 192.168.1.50 to see your portal. Real users like to use names.

If I want to take this a step further, I can turn on the DNS (Domain Name System) service right inside the same Repurposed_PC so that typing a clean name like company.local automatically opens up my webpage.

In the real world, this mirrors a Linux server running both Apache (Web) and BIND (DNS) at the same time to save space and hardware costs.

Here is exactly how to set up the DNS routing on your current network:

🧩 Step 1: Turn on DNS on the Repurposed_PC
1. Click on your Repurposed_PC server.
2. Go to the Services tab and click on DNS in the left column.
3. Turn the DNS Service ON using the radio button.
4. In the Name box, type your desired shortcut URL (e.g., company.local).
5. In the Address box, type the IP address of the web server itself: 192.168.1.60.
6. Click the Add button. You will see the record appear in the table below.
<img width="1140" height="574" alt="image" src="https://github.com/user-attachments/assets/607d5c54-6d2b-45d3-b80c-92336702dbaa" />

🔌 Step 2: Tell the Workstation Where to Find DNS

For the client workstation to use this new feature, you must tell it which device handles domain names:
1. Click on your client Workstation PC.
2. Go to Desktop -> IP Configuration.
3. Locate the DNS Server field at the bottom and type: 192.168.1.50.
<img width="1138" height="698" alt="image" src="https://github.com/user-attachments/assets/0e776cd4-5556-46e6-9951-ba77fc1a9575" />

4. Close out of the configuration window.

🧪 Step 3: Test the Friendly Name URL
1. Still on the Workstation PC, open the Web Browser application again.
2. Instead of typing the numbers, type your clean address: mycompany.local and hit Enter.
<img width="1140" height="406" alt="image" src="https://github.com/user-attachments/assets/f32ead52-f5cb-4774-8802-ae8a9eb66420" />

The internal company portal should immediately open up, proving that your repurposed desktop is now successfully handling both web traffic and local network directory services.
