# Windows-Server-Active-Directory-Lab


Overview

This project documents the deployment and configuration of a Windows Server Active Directory environment in a virtual lab. The goal was to simulate a small enterprise network that provides centralized identity management, dynamic IP addressing, internal DNS resolution, and Group Policy–based administration.

The lab environment includes:

- A Windows Server domain controller with dual network interfaces
- An internal network for domain services
- DHCP and DNS hosted on the domain controller
- A Windows 11 client joined to the domain
- Automated user creation using PowerShell
- Organizational Units designed for Group Policy application

This project demonstrates hands-on experience with core system administration and identity infrastructure tasks.


## Lab Environment

Domain Configuration

- Domain name: Domain.local

- Domain controller internal IP address: 172.16.0.1/24

- DNS server on domain controller: 127.0.0.1

DHCP Scope

- Scope range: 172.16.0.100 – 172.16.0.200

- Subnet mask: 255.255.255.0

- DNS server option: 172.16.0.1

## Network Architecture

The domain controller is configured with two network interfaces:

1. External NIC: Provides internet access for the server.

2. Internal NIC: Dedicated to the domain environment and used for:

    - Active Directory services

    - DNS

    - DHCP

    - Communication with domain-joined clients

Insert screenshot: Server network adapter configuration
screenshots/server-nics.png
<img width="1023" height="770" alt="Windows Server NICs" src="https://github.com/user-attachments/assets/91690208-4f41-44f7-8e7c-dfa439097f18" />
https://imgur.com/a/nBizaoR

Insert screenshot: Internal NIC static IP configuration
screenshots/internal-ip-config.png
<img width="1024" height="769" alt="Windows Serer Internal NIC config" src="https://github.com/user-attachments/assets/bb824040-ac48-4d84-a526-0b3bf4dbebe9" />
https://imgur.com/a/2AYExEu


## Active Directory Domain Services Deployment

Active Directory Domain Services was installed and the server was promoted to a domain controller. A new forest was created using the domain name Domain.local.

Insert screenshot: AD DS role installation
screenshots/adds-role.png

Insert screenshot: Domain controller promotion wizard
screenshots/domain-promotion.png

## DNS Configuration

DNS was installed as part of the AD DS deployment and configured to host the Domain.local zone. The domain controller registers the required SRV records, allowing clients to locate authentication and directory services.

Insert screenshot: DNS Manager with forward lookup zone
screenshots/dns-forward-lookup-zone.png

Insert screenshot: SRV records for the domain
screenshots/dns-srv-records.png

## DHCP Configuration

DHCP was configured on the internal network to dynamically assign IP addresses to client machines.

Scope details:

Address range: 172.16.0.100 – 172.16.0.200

DNS server: 172.16.0.1

Scope authorized in Active Directory

Insert screenshot: DHCP scope configuration
screenshots/dhcp-scope.png

Insert screenshot: DHCP lease issued to Windows 11 client
screenshots/dhcp-lease.png

## Windows 11 Domain Join

The Windows 11 client was configured to obtain an IP address automatically from DHCP and use the domain controller for DNS. After connectivity and name resolution were verified, the machine was successfully joined to the Domain.local domain.

Insert screenshot: Client IP configuration
screenshots/windows11-ipconfig.png

Insert screenshot: Domain join confirmation
screenshots/domain-joined.png

Insert screenshot: Domain user sign-in
screenshots/domain-user-login.png

## Active Directory User Creation with PowerShell

A PowerShell script was used to automate the creation of domain user accounts. This approach reflects real-world administrative practices for bulk provisioning.

The script performed the following tasks:

Created multiple user accounts

Set default passwords

Enabled the accounts

Placed users into the appropriate Organizational Units

Insert screenshot: PowerShell script execution
screenshots/user-creation-script.png

Insert screenshot: Newly created users in Active Directory Users and Computers
screenshots/ad-users-created.png

## Organizational Unit Structure

Organizational Units were created to mirror a production-style environment and allow targeted Group Policy deployment.

Example structure:

Domain.local
 └── HR
      ├── Users
      ├── Computers
      └── Groups

Insert screenshot: OU structure in Active Directory
screenshots/ou-structure.png

## Group Policy Configuration

Group Policy is used to enforce centralized configuration and security settings across domain-joined systems.

Planned and implemented policies include:

Password and account lockout policies

Desktop and user environment restrictions

Network drive mapping

Windows Update configuration

Insert screenshot: Group Policy Management Console
screenshots/gpmc.png

Insert screenshot: Example GPO settings
screenshots/gpo-example.png

## Validation and Testing

The following tests were performed to confirm proper functionality:

DHCP address assignment:

Client received an IP within the configured scope

DNS resolution:

nslookup domain.local

Group Policy application:

gpresult /r

Authentication:

Successful login using a domain user account

Insert screenshot: gpresult output
screenshots/gpresult.png

Skills Demonstrated

Active Directory deployment and administration

DNS and DHCP configuration

Domain join and authentication troubleshooting

PowerShell for administrative automation

Organizational Unit design

Group Policy management

Network segmentation using multiple NICs
