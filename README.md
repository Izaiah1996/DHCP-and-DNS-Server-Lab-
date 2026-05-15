# DHCP-and-DNS-Server-Lab-
In this lab, I expanded my existing Active Directory home lab by configuring DHCP and DNS services on a Windows Server domain controller inside VirtualBox.

The goal of this project was to simulate a small enterprise network where client systems automatically receive IP configuration from a DHCP server and communicate through internal DNS resolution.

This lab focused heavily on troubleshooting and infrastructure validation rather than just installation. During the build process, I diagnosed and corrected DHCP lease issues, VirtualBox networking problems, incorrect gateway assignments, and stale client adapter configurations.

This environment now includes:

• Active Directory Domain Services  
• DNS Services  
• DHCP Services  
• Domain joined Windows client  
• Internal VirtualBox enterprise style network  



Technologies Used

• Windows Server  
• Active Directory Domain Services (AD DS)  
• DHCP  
• DNS  
• Windows 11 Client VM  
• Oracle VirtualBox  
• Command Prompt  
• Internal Virtual Networking  



Lab Environment

Domain Controller

Hostname: `DC01`  
IP Address: `192.168.1.10`  
Roles Installed:

• Active Directory Domain Services  
• DNS Server  
• DHCP Server  

Client Machine

Hostname: `CLIENT01`  
Joined Domain: `zaylab.local`  
IP Assignment Method: DHCP  



Network Topology

```text
DC01 (192.168.1.10)
│
├── Active Directory
├── DNS Server
├── DHCP Server
│
└── Internal VirtualBox Network (LABNET)
        │
        └── CLIENT01
                └── Receives DHCP Lease Automatically




DHCP Configuration

A DHCP scope was created to dynamically assign IP addresses to client systems connected to the internal LABNET network.

DHCP Scope Settings

| Setting | Value |
|---|---|
| Scope Name | Office LAN Scope |
| IP Range | 192.168.1.100 - 192.168.1.200 |
| Subnet Mask | 255.255.255.0 |
| Default Gateway | 192.168.1.1 |
| DNS Server | 192.168.1.10 |
| Domain Name | zaylab.local |



DNS Configuration

DNS services were configured directly on the domain controller to allow internal name resolution for systems connected to the lab environment.

The Windows client automatically received DNS settings through DHCP and successfully communicated with the domain controller.



Client Verification

After correcting the network adapter configuration and DHCP settings, the client machine successfully received a dynamic IP lease from the DHCP server.

Verification steps included:

• Confirming DHCP assigned addressing using `ipconfig`  
• Verifying proper gateway assignment  
• Confirming DNS suffix resolution  
• Testing connectivity to the domain controller using `ping`  



Troubleshooting Process

This project required significant troubleshooting during deployment.

Issue 1: Incorrect Default Gateway

The client machine initially received:

```text
Default Gateway: 192.168.1.0
```

This was invalid because `.0` represents the network address and cannot function as a usable gateway.

The gateway configuration was corrected to:

```text
192.168.1.1
```



Issue 2: Stale Static Adapter Configuration

Even after DHCP was configured correctly, the client machine continued holding old network settings.

To resolve this:

• IPv4 settings were reset to automatic addressing  
• DNS settings were reset to automatic assignment  
• The Ethernet adapter was disabled and re-enabled  
• DHCP lease validation was retested  



Issue 3: VirtualBox Internal Networking

The client VM initially failed to communicate properly with the DHCP server because of networking inconsistencies between the virtual machines.

The following settings were validated:

• Internal Network enabled  
• Both VMs connected to `LABNET`  
• Virtual cable connected enabled  
• Matching adapter configurations across VMs  



Successful DHCP Lease

<img width="868" height="506" alt="Screenshot 2026-05-14 164128" src="https://github.com/user-attachments/assets/7e02b20f-737e-432a-b857-002d9448b33d" />


Successful Domain Controller Connectivity

<img width="741" height="364" alt="Screenshot 2026-05-14 170702" src="https://github.com/user-attachments/assets/e2cdb60b-7c39-437a-a59f-eb459e030a9f" />


DHCP Scope Configuration

<img width="873" height="401" alt="Screenshot 2026-05-13 153436" src="https://github.com/user-attachments/assets/565a094e-beb6-41b2-a7d9-ce9534dc34ca" />



Scope Options Configuration

<img width="648" height="401" alt="Screenshot 2026-05-14 200909" src="https://github.com/user-attachments/assets/72b28db0-ec0c-43bd-a384-682bd9d3fa47" />


Lessons Learned

This lab reinforced several important enterprise networking concepts:

• DHCP scope management  
• Internal DNS resolution  
• Client lease assignment  
• Network troubleshooting methodology  
• Virtualized infrastructure management  
• Importance of validating gateway and adapter settings  
• Enterprise style client/server communication  

This project also demonstrated the importance of troubleshooting layered infrastructure dependencies rather than assuming installation alone equals functionality.



Related Project

This lab was built on top of my existing Active Directory Home Lab environment.

The DHCP and DNS services configured in this project were deployed using the previously configured domain controller infrastructure.
