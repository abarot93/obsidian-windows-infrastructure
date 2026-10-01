# Obsidian Windows Infrastructure Project

## Project Overview

I was hired by a company called **Obsidian** to design and build its IT infrastructure from the ground up.

My responsibility was to create a structured, secure and manageable Windows environment capable of supporting the company’s users, departments, authentication, file access, administration and day-to-day IT operations.

The environment was built using Microsoft Azure virtual machines to provide the underlying compute platform, while the Windows infrastructure itself was designed and managed as a traditional business server environment.

The project covers the full infrastructure build, including:

- Windows Server deployment
- Active Directory Domain Services
- DNS
- Organisational Units
- User and group administration
- Privileged administrator accounts
- Service Desk delegation
- Domain Controller redundancy
- File services
- NTFS and SMB permissions
- Group Policy
- Domain-joined Windows clients
- Backup and recovery
- Monitoring
- PowerShell administration
- Infrastructure troubleshooting
- Least-privilege access control

---

## Company Structure

Obsidian is organised into four main departments:

### IT

The IT team is responsible for supporting users and managing the company infrastructure.

#### Domain Administrator

**Anil Barot**

Normal account:

`anil.barot`

Privileged account:

`adm.anil.barot`

The privileged account is used for domain-level administration, including:

- Active Directory administration
- Domain Controller administration
- DNS management
- Group Policy administration
- Domain-wide security changes
- Privileged user and group management
- Server administration
- RDP access to authorised infrastructure
- PowerShell administration
- Infrastructure troubleshooting

#### Server Administrator

**Daniel Reed**

Normal account:

`daniel.reed`

Privileged account:

`adm.daniel.reed`

The privileged account is used for server-level administration, including:

- Member server administration
- File server administration
- Windows service management
- RDP access to authorised servers
- Server troubleshooting
- Server-level PowerShell administration

Daniel does not have unrestricted Domain Administrator access.

#### Service Desk

**Maya Patel**

`maya.patel`

**Aaron Smith**

`aaron.smith`

The Service Desk team has delegated permissions for routine user administration, including:

- Creating new user accounts
- Resetting passwords
- Unlocking accounts
- Updating user details
- Managing approved group membership
- Disabling accounts
- Supporting new starters
- Supporting leavers and offboarding

Service Desk staff do not have unrestricted Domain Administrator or server administration access.

---

### Finance

- Sarah Green
- Liam Brown
- Emma Wilson

Finance users have access to department-specific resources and file shares.

---

### Human Resources

- James Taylor
- Nina Shah
- Olivia Moore

HR users have access to department-specific resources and permissions.

---

### Sales

- Ethan Clark
- Chloe Adams
- Ryan Scott

Sales users have access to department-specific resources and mapped drives.

---

## Infrastructure Design

The Windows environment consists of four core systems.

### DC01

Primary Domain Controller providing:

- Active Directory Domain Services
- DNS
- User authentication
- Domain services
- Group Policy processing

### DC02

Secondary Domain Controller providing:

- Active Directory replication
- DNS redundancy
- Authentication resilience
- Additional Domain Controller availability

### FS01

Dedicated Windows File Server providing:

- Departmental SMB shares
- NTFS permissions
- Group-based access
- Centralised file storage

### CLIENT01

Windows client used to validate:

- Domain joining
- User authentication
- Group Policy
- Mapped drives
- File permissions
- Standard user access
- Service Desk access

---

## Network Design

### Virtual Network

`VNET-Obsidian`

Address space:

`172.16.0.0/16`

### Server Subnet

`SNET-Servers`

Address range:

`172.16.10.0/24`

Planned server addresses:

- `DC01` — `172.16.10.10`
- `DC02` — `172.16.10.11`
- `FS01` — `172.16.10.20`

### Client Subnet

`SNET-Clients`

Address range:

`172.16.20.0/24`

`CLIENT01` is deployed within the client subnet.

---

## Active Directory Design

Domain:

`obsidian.local`

NetBIOS domain name:

`OBSIDIAN`

### Organisational Units

- IT
- Finance
- HR
- Sales
- Servers
- Workstations
- Admin Accounts

### OU Purpose

- `IT` — normal IT staff accounts
- `Finance` — Finance users
- `HR` — HR users
- `Sales` — Sales users
- `Servers` — member servers such as `FS01`
- `Workstations` — client devices such as `CLIENT01`
- `Admin Accounts` — privileged administrator accounts

### Computer Placement

The built-in `Domain Controllers` OU contains:

- `DC01`
- `DC02`

The custom `Servers` OU contains:

- `FS01`

The custom `Workstations` OU contains:

- `CLIENT01`

---

## Security Groups

### Department Groups

- `GG-IT-Users`
- `GG-Finance-Users`
- `GG-HR-Users`
- `GG-Sales-Users`

### IT Administration Groups

- `GG-ServiceDesk`
- `GG-Server-Admins`
- `GG-RDP-Servers`

The environment uses security groups and delegated administration to provide access based on job responsibilities and least-privilege principles.

---

## Administrative Model

### Standard IT Accounts

Normal IT accounts are used for everyday activity.

Examples:

- `anil.barot`
- `daniel.reed`
- `maya.patel`
- `aaron.smith`

These accounts are not used for unrestricted privileged administration.

---

### Domain Administrator

Anil Barot uses the privileged account:

`adm.anil.barot`

This account is a member of:

- `Domain Admins`
- `GG-Server-Admins`
- `GG-RDP-Servers`

This provides the required access for:

- Domain administration
- Domain Controller administration
- DNS management
- Group Policy administration
- Domain-wide security changes
- Server administration
- RDP
- Privileged PowerShell administration

---

### Server Administrator

Daniel Reed uses the privileged account:

`adm.daniel.reed`

This account is a member of:

- `GG-Server-Admins`
- `GG-RDP-Servers`

Daniel is not a member of:

`Domain Admins`

This allows server administration to be separated from unrestricted domain administration.

---

### Service Desk Accounts

Service Desk users:

- `maya.patel`
- `aaron.smith`

are members of:

`GG-ServiceDesk`

They receive delegated permissions for routine user administration.

Their responsibilities include:

- New starter creation
- Password resets
- Account unlocks
- Routine account changes
- Approved group membership management
- User disablement
- Offboarding support

They do not receive unrestricted Domain Administrator or server administration rights.

---

## Least-Privilege Model

The IT access model separates administrative responsibilities into three levels:

### Domain Administration

`adm.anil.barot`

Full domain-level administrative access.

### Server Administration

`adm.daniel.reed`

Administration of authorised member servers without unrestricted domain-wide access.

### Service Desk

`maya.patel`

`aaron.smith`

Delegated user-management permissions only.

This separation helps ensure users only receive the level of access required for their responsibilities.

---

## File Services

FS01 provides centralised departmental storage.

Planned shares include:

`\\FS01\Finance`

`\\FS01\HR`

`\\FS01\Sales`

`\\FS01\IT`

`\\FS01\Public`

Access is controlled using:

- Active Directory security groups
- NTFS permissions
- SMB share permissions

Example:

`GG-Finance-Users`

receives access to the Finance share while users from other departments are restricted.

---

## Group Policy

Group Policy is used to centrally manage user and workstation settings.

Planned policies include:

- Mapped network drives
- Account and password settings
- Desktop restrictions
- Security settings
- Screen lock policies
- Windows Defender configuration
- User restrictions

Example mapped drives:

- Finance — `F:` → `\\FS01\Finance`
- HR — `H:` → `\\FS01\HR`
- Sales — `S:` → `\\FS01\Sales`

Domain-wide Group Policy administration is performed using the Domain Administrator account.

---

## Remote Administration

Remote administration is controlled according to role.

### Domain Administrator

`adm.anil.barot`

Can administer:

- `DC01`
- `DC02`
- `FS01`

using tools including:

- RDP
- Server Manager
- Active Directory Users and Computers
- DNS Manager
- Group Policy Management
- PowerShell

### Server Administrator

`adm.daniel.reed`

Can administer authorised member servers such as:

- `FS01`

using:

- RDP
- Server Manager
- PowerShell
- Windows administrative tools

Daniel does not automatically receive unrestricted Domain Controller administration.

### Service Desk

Maya and Aaron use delegated Active Directory tools for routine support tasks and do not receive unrestricted server RDP access.

---

## Backup and Recovery

The environment includes backup and recovery exercises covering:

- Windows Server Backup
- Scheduled backups
- File recovery
- Deleted file restoration
- Recovery testing

A key objective is to demonstrate the difference between:

- Backup
- Restore
- VM snapshot/checkpoint concepts

---

## Monitoring and Administration

Server health and administration are performed using tools including:

- Server Manager
- Event Viewer
- Task Manager
- Resource Monitor
- Performance Monitor
- PowerShell
- Azure VM metrics
- Active Directory Users and Computers
- DNS Manager
- Group Policy Management

---

## Troubleshooting

The project includes deliberate troubleshooting scenarios such as:

- Incorrect DNS configuration
- Domain join failures
- Group Policy not applying
- File share access problems
- NTFS permission issues
- Incorrect group membership
- Domain Controller replication issues
- Windows service failures
- RDP problems
- Firewall restrictions
- Account lockouts
- Low disk space

Tools used include:

- `ipconfig`
- `ping`
- `nslookup`
- `Resolve-DnsName`
- `Test-NetConnection`
- `gpresult`
- `dcdiag`
- `repadmin`
- Event Viewer
- Services
- PowerShell

Troubleshooting exercises will also demonstrate the differences between:

- Domain Administrator responsibilities
- Server Administrator responsibilities
- Service Desk responsibilities

---

## PowerShell Administration

PowerShell is used to support routine infrastructure administration.

Examples include:

- `Get-ADUser`
- `Get-ADGroupMember`
- `Get-ADComputer`
- `Get-Service`
- `Get-SmbShare`
- `Get-WinEvent`
- `Test-NetConnection`

Later exercises include:

- Bulk user creation
- Group membership automation
- Service checks
- User reports
- Administrative scripting

Domain-wide PowerShell administration is performed using the Domain Administrator account.

Server-level PowerShell administration can be performed by the Server Administrator on authorised member servers.

---

## Security Hardening

The project includes security controls such as:

- Separate standard and privileged accounts
- Limited Domain Admin membership
- Separation of domain and server administration
- Delegated Service Desk permissions
- Restricted RDP access
- Account lockout policies
- Password policies
- Firewall rules
- Least privilege
- Auditing
- Privileged group reviews

### Privileged Group Model

#### Domain Admins

- `adm.anil.barot`

#### GG-Server-Admins

- `adm.anil.barot`
- `adm.daniel.reed`

#### GG-RDP-Servers

- `adm.anil.barot`
- `adm.daniel.reed`

#### GG-ServiceDesk

- `maya.patel`
- `aaron.smith`

---

## User Lifecycle Administration

The environment is used to simulate common IT support processes including:

- New starters
- Password resets
- Account unlocks
- Department transfers
- Group membership changes
- Access requests
- Account disablement
- Leavers and offboarding

Routine lifecycle tasks are delegated to the Service Desk where appropriate.

Higher-level administrative changes remain restricted to privileged accounts.

---

## Project Phases

1. Azure Foundation
2. DC01 Deployment
3. Active Directory Structure
4. Administrative and Service Desk Access
5. DC02 Deployment and Replication
6. DNS Administration
7. FS01 Deployment
8. File Server Storage
9. CLIENT01 Deployment
10. Group Policy
11. RDP and Remote Administration
12. User Lifecycle Administration
13. Backup and Recovery
14. Troubleshooting
15. Monitoring and Server Health
16. PowerShell Administration
17. Security Hardening
18. Final Infrastructure Documentation

---

## Project Objectives

- Design a structured Windows domain environment
- Deploy and configure Windows Server infrastructure
- Implement Active Directory Domain Services
- Implement DNS
- Create users, groups and organisational units
- Separate standard and privileged accounts
- Restrict Domain Admin membership
- Separate domain administration from server administration
- Delegate Service Desk permissions
- Implement Domain Controller redundancy
- Configure centralised file services
- Apply NTFS and SMB permissions
- Deploy and manage Group Policy
- Join and administer Windows clients
- Implement backup and recovery
- Monitor server health
- Perform PowerShell administration
- Troubleshoot common infrastructure faults
- Apply least-privilege administration

---

## Skills Demonstrated

This project demonstrates hands-on experience with:

- Windows Server 2022
- Microsoft Azure Virtual Machines
- Active Directory Domain Services
- DNS
- Group Policy
- Active Directory replication
- Organisational Units
- Security groups
- Delegated administration
- Least privilege
- Role-based administration
- Privileged administrator accounts
- Windows File Server
- SMB
- NTFS permissions
- Domain-joined Windows clients
- RDP
- RSAT
- PowerShell
- Backup and recovery
- Monitoring
- Infrastructure troubleshooting
- User lifecycle administration

---

## Documentation

This repository documents the build process, configuration decisions, screenshots, commands, troubleshooting exercises and lessons learned throughout the implementation of the Obsidian Windows infrastructure environment.
