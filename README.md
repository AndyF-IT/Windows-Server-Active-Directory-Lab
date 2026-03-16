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
* **Virtualization Platform** – Used to create and manage the virtual machines that simulate an enterprise network environment.


### Architecture / Network Diagram
<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/47251f68-38fb-49ca-b330-b5828b57e9c2" />

## NIC Configuration

The domain controller is configured with two network interfaces:

1. External NIC: Provides internet access for the server.

2. Internal NIC: Dedicated to the domain environment and used for:

    - Active Directory services

    - DNS

    - DHCP

    - Communication with domain-joined clients
Server network adapter configuration:

<img width="777" height="588" alt="image" src="https://github.com/user-attachments/assets/5b1a6029-ace9-45d8-8a9e-56ef13bf3962" />


Internal NIC static IP configuration:

<img width="395" height="451" alt="image" src="https://github.com/user-attachments/assets/d28d9e10-5f91-48e6-b17e-3cafc3d83e64" />


## Active Directory Domain Services Deployment

Active Directory Domain Services was installed and the server was promoted to a domain controller. A new forest was created using the domain name Domain.local.

Insert screenshot: AD DS role installation
screenshots/adds-role.png

https://imgur.com/a/3jxzd5l

Domain controller promotion wizard
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

DNS Manager with forward lookup zone
<img width="968" height="530" alt="image" src="https://github.com/user-attachments/assets/42540950-32a1-49f3-a85b-fdb15f9250ab" />


Active Directory automatically registers Service Location (SRV) records in DNS to allow domain clients to locate essential services such as LDAP, Kerberos authentication, and the Global Catalog.

The screenshot below shows the SRV records created under the `_tcp` folder for the domain controller.


<img width="965" height="530" alt="image" src="https://github.com/user-attachments/assets/400222d8-913b-44d3-bd7a-edff2457758e" />

## DHCP Configuration

DHCP was configured on the internal network to dynamically assign IP addresses to client machines.

Scope details:

Address range: 172.16.0.100 – 172.16.0.200
Subnet Mask: /24
Default Gateway: 192.168.64.9 (External Facing NIC)
DNS server: 172.16.0.1

Scope authorized in Active Directory

Insert screenshot: DHCP scope configuration
screenshots/dhcp-scope.png
<img width="744" height="549" alt="image" src="https://github.com/user-attachments/assets/9a57a0ec-6444-49df-a6ac-88b1c7a5cdd2" />
<img width="744" height="549" alt="image" src="https://github.com/user-attachments/assets/d77d5c5e-21f0-4922-ae88-7f81b0c186d4" />
<img width="742" height="544" alt="image" src="https://github.com/user-attachments/assets/cf480dad-be31-4c09-bbf7-fd690db9b65e" />

Insert screenshot: DHCP lease issued to Windows 11 client
screenshots/dhcp-lease.png
<img width="772" height="554" alt="Screenshot 2026-03-15 at 6 45 07 PM" src="https://github.com/user-attachments/assets/349b47d1-94a1-4d6f-93f8-77ec49483b1a" />


## Windows 11 Domain Join

The Windows 11 client was configured to obtain an IP address automatically from DHCP and use the domain controller for DNS. After connectivity and name resolution were verified, the machine was successfully joined to the Domain.local domain.

Insert screenshot: Client IP configuration
screenshots/windows11-ipconfig.png
<img width="1036" height="776" alt="Domain Join 1" src="https://github.com/user-attachments/assets/f0f77b8a-efdb-4685-948f-c0889a0aef52" />

Insert screenshot: Domain join confirmation
screenshots/domain-joined.png
<img width="1021" height="771" alt="Domain Join 3" src="https://github.com/user-attachments/assets/44bd97d7-f532-4c4b-87b5-9d9c8fffe12f" />

Insert screenshot: Domain user sign-in
screenshots/domain-user-login.png
<img width="987" height="511" alt="Screenshot 2026-03-15 at 6 46 44 PM" src="https://github.com/user-attachments/assets/c27fcccf-6e6a-4e09-99fc-48d49bd74c29" />
