# Windows-Server-Active-Directory-Lab


### Overview

This project documents the deployment and configuration of a Windows Server Active Directory environment within a virtual lab. The objective was to simulate a small enterprise network that provides centralized identity management, automated IP address assignment, internal DNS name resolution, and basic domain-based administration.

The lab environment consists of a Windows Server domain controller configured with dual network interfaces to support both external connectivity and an isolated internal network for domain services. DNS and DHCP services are hosted on the domain controller to provide name resolution and dynamic IP addressing for devices within the network.

A Windows 11 virtual machine was deployed as a domain client and successfully joined to the Active Directory domain, demonstrating the integration of client systems into a centralized authentication and management environment.

This project highlights hands-on experience configuring core Windows Server infrastructure services commonly used in enterprise environments.

### Key Technologies Used

* **Windows Server 2022** – Used to deploy and configure the Active Directory Domain Controller and core network services.
* **Active Directory Domain Services (AD DS)** – Provides centralized authentication, authorization, and identity management for domain users and devices.
* **DNS (Domain Name System)** – Enables internal name resolution for domain resources and supports Active Directory functionality.
* **DHCP (Dynamic Host Configuration Protocol)** – Automatically assigns IP addresses and network configuration to client devices within the internal network.
* **Group Policy** – Provides centralized management of user and computer configurations across the domain environment.
* **Windows 11** – Configured as a domain-joined client machine to test authentication and domain-based resource access.
* **UTM Virtualization Platform** – Used to create and manage the virtual machines that simulate an enterprise network environment.


### Architecture / Network Diagram
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/47251f68-38fb-49ca-b330-b5828b57e9c2" />

## NIC Configuration

The domain controller is configured with two network interfaces:

1. External NIC: 
 
    - Gets IP configuration settings from home's internet service provider (DHCP)

2. Internal NIC:

    - Gets static IP since it will be used to configure domain features

### Server network adapter configuration:

<p align="center">
  <img width="777" height="588" alt="image" src="https://github.com/user-attachments/assets/5b1a6029-ace9-45d8-8a9e-56ef13bf3962" />
  <br>
  <em>Dual network interface configuration showing external and internal adapters.The first adapter connects to the isolalted internal network used for the Active Directory Lab environment, while the second adapter connected to the external network to provide internet connectivity.</em>
</p>


### Internal NIC Settings:

<p align="center">
  <img width="395" height="451" alt="image" src="https://github.com/user-attachments/assets/d28d9e10-5f91-48e6-b17e-3cafc3d83e64" />
  <br>
  <em>This screenshot displays the configuration of the internal network adapter. The interface is assigned a static IP address and is used to host core domain services such as Active Directory, DNS, and DHCP. Client machines connected to the internal network communicate with the domain controller through this interface.</em>
</p>

## Active Directory Domain Services Deployment

Active Directory Domain Services was installed and the server was promoted to a domain controller. A new forest was created using the domain name Domain.local. The following 2 drop-downs document the process:

<details>
<summary><b>Active Directory Domain Services Role Installation</b></summary>
<br>

**Step 1 – Server Manager -> Manage -> Add Roles and Features**

<img width="1025" height="728" alt="Screenshot 2026-03-16 at 9 04 41 PM" src="https://github.com/user-attachments/assets/5334a634-e708-4aa1-820c-1e8cec2af73f" />

**Step 2 – Choose Role/Feature Based INstallation**

<img width="787" height="560" alt="Screenshot 2026-03-16 at 9 05 09 PM" src="https://github.com/user-attachments/assets/769d199e-53eb-4f6a-9e2f-a4de4f8968c3" />

**Step 3 – Select Server to install roles and features on**

<img width="843" height="603" alt="image" src="https://github.com/user-attachments/assets/62de7cce-3c78-442b-9847-464bbfd7b0e9" />

**Step 4 - Select Active Directory Domain Services**

<img width="786" height="559" alt="Screenshot 2026-03-16 at 9 06 30 PM" src="https://github.com/user-attachments/assets/e21f724a-8c35-4885-89f3-b1caa6024bbb" />

**Step 5 - Add Required Features for AD DS**

<img width="448" height="466" alt="image" src="https://github.com/user-attachments/assets/02e5cbb6-c91a-492c-bcce-4a0ed98ff45c" />

**Step 6 - Install the roles, services, and features**

<img width="843" height="599" alt="image" src="https://github.com/user-attachments/assets/df5e9545-2025-4fcb-89cf-8420f96912c5" />


</details>

<details>
<summary><b>Domain Controller Promotion Process</b></summary>
<br>

**Step 1 – Promote Server to Domain Controller**

<img src="https://github.com/user-attachments/assets/bbe71464-8a3c-4dc7-9c64-490777c3ff61" width="800"/>

**Step 2 – Configure Forest Root Domain**

<img src="https://github.com/user-attachments/assets/d278a5a0-1380-4554-abc4-225969c0019c" width="800"/>

**Step 3 – Set Domain Controller Password**

<img src="https://github.com/user-attachments/assets/bd4a9b60-68d1-4ddb-b362-58f98f0bfe0c" width="800"/>

**Step 4 - Assign Path**

<img src="https://github.com/user-attachments/assets/d1859c65-ccf9-4fa3-a177-5adad1910594" width="800"/>

**Step 5 - Complete Precheck Before Installation**

<img src="https://github.com/user-attachments/assets/41b14477-bef2-42c7-9303-2ef95feb1676" width="800"/>
</details>


## DNS Configuration
<p align="center">
<img width="968" height="530" alt="image" src="https://github.com/user-attachments/assets/42540950-32a1-49f3-a85b-fdb15f9250ab" />
  <br>
  <em>This screenshot shows the Forward Lookup Zone configured in DNS Manager for the domain.</em>
</p>

The zone contains DNS records that map hostnames to IP addresses, allowing devices within the network to locate domain resources such as the domain controller and other services. In an Active Directory environment, the DNS zone is typically integrated with Active Directory, enabling automatic record registration and replication across domain controllers.

<p align="center">
 <img width="965" height="530" alt="image" src="https://github.com/user-attachments/assets/400222d8-913b-44d3-bd7a-edff2457758e" />
  <br>
  <em>This screenshot shows the Service (SRV) records located in the _tcp folder of the domain's DNS zone.</em>
</p>

These records are automatically created by Active Directory and are used by domain clients to locate critical services such as domain controllers, Kerberos authentication servers, and LDAP services. Proper registration of these records is essential for domain authentication and overall Active Directory functionality.

## DHCP Configuration

DHCP was configured on the internal network to dynamically assign IP addresses to client machines.

Scope details:

- Address range: 172.16.0.100 – 172.16.0.200
- Subnet Mask: /24
- Default Gateway: 192.168.64.9 (External Facing NIC)
- DNS server: 172.16.0.1

Scope authorized in Active Directory

DHCP scope configuration
<p align="center">
 <img width="755" height="572" alt="image" src="https://github.com/user-attachments/assets/9e23f727-3d6c-4025-968a-6409622c18d0" />
  <br>
  <em>This is the DHCP address pool configured for the internal network.</em>
</p>

The scope defines the range of IP addresses that the DHCP server can dynamically assign to client devices within the lab environment. By managing the address pool centrally, the server can automatically provide IP configuration to domain clients without requiring manual network setup.

<p align="center">
 <img width="753" height="568" alt="image" src="https://github.com/user-attachments/assets/8d2f8ae8-15ac-4bad-b7b4-9b7f0b2e0fa5" />
 <br>
  <em>The configured DHCP scope options for the internal network.</em>
</p>

Scope options provide additional network settings to DHCP clients, such as the default gateway, DNS server, and domain name. These settings ensure that domain-joined systems automatically receive the correct network configuration required to communicate with Active Directory services.

<p align="center">
<img width="772" height="554" alt="Screenshot 2026-03-15 at 6 45 07 PM" src="https://github.com/user-attachments/assets/349b47d1-94a1-4d6f-93f8-77ec49483b1a" />
  <br>
  <em>The active DHCP address leases issued by the server.</em>
</p>

This screenshot shows the active DHCP address leases issued by the server. Each lease represents an IP address that has been dynamically assigned to a client device on the network. Viewing DHCP leases allows adminis to verify that clients are successfully receiving network configurations and connecting to the domain environment.

## Windows 11 Domain Join

The Windows 11 client was configured to obtain an IP address automatically from DHCP and use the domain controller for DNS. After connectivity and name resolution were verified, the machine was successfully joined to the Domain.local domain.

**Step 1 - Set Client Machines DNS as the Domain Controller's IP address**

<img width="440" height="509" alt="image" src="https://github.com/user-attachments/assets/2f25c630-530c-448a-a9a6-c7b636ae3e79" />

**Step 2 - Settings -> System -> Domain or Workgroup -> Change**

1. <br>
   <img width="921" height="624" alt="Screenshot 2026-03-16 at 11 36 15 PM" src="https://github.com/user-attachments/assets/85c13a86-f0d1-434b-bdbc-490644ebd035" />

2. <br>
    <img width="411" height="465" alt="Screenshot 2026-03-16 at 11 36 32 PM" src="https://github.com/user-attachments/assets/71c0b761-e6a6-464d-89c8-d0e023b560e0" />
    
3. <br>
    <img width="411" height="465" alt="image" src="https://github.com/user-attachments/assets/2542fc77-5f41-48dd-a07f-473d8c1d91a7" />

**Step 3- Join Domain and Restart PC**
<img width="1021" height="771" alt="Domain Join 3" src="https://github.com/user-attachments/assets/44bd97d7-f532-4c4b-87b5-9d9c8fffe12f" />

**Step 4 - Verify IP settings w/ ipconfig command**
<img width="1111" height="627" alt="image" src="https://github.com/user-attachments/assets/464e868b-20ba-4e68-ae84-14b167d54cf1" />
