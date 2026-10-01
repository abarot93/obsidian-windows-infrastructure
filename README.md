# Obsidian Windows Infrastructure Project

## Project Overview

I was hired by a company called **Obsidian** to design and build its IT infrastructure from the ground up.

My responsibility was to create a structured, secure and manageable Windows environment capable of supporting the company’s users, departments, authentication, file access, administration and day-to-day IT operations.

The environment was built using Azure virtual machines to provide the underlying compute platform, while the Windows infrastructure itself was designed and managed as a traditional business server environment.

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

The aim was to build an environment that could be managed and supported in a structured way, while applying principles such as least privilege, role-based access and separation between standard and administrative accounts.

---

## Company Structure

Obsidian is organised into four main departments:

### IT

The IT team is responsible for supporting users and managing the company infrastructure.

#### System Administrators

- Anil Barot
- Daniel Reed

The System Administrators use separate standard and privileged accounts.

Privileged responsibilities include:

- Active Directory administration
- DNS management
- Group Policy administration
- Windows Server administration
- File server management
- RDP access to authorised infrastructure
- PowerShell administration
- Infrastructure troubleshooting
- User and group administration
- Security and permissions management

#### Service Desk

- Maya Patel
- Aaron Smith

The Service Desk team has delegated permissions for routine user administration, including:

- Creating new user accounts
- Resetting passwords
- Unlocking accounts
- Updating user details
- Managing approved group membership
- Disabling accounts
- Supporting new starters and leavers

Service Desk staff do not have unrestricted Domain Administrator access.

### Finance

- Sarah Green
- Liam Brown
- Emma Wilson

Finance users have access to department-specific resources and file shares.

### Human Resources

- James Taylor
- Nina Shah
- Olivia Moore

HR users have access to department-specific resources and permissions.

### Sales

- Ethan Clark
- Chloe Adams
- Ryan Scott

Sales users have access to department-specific resources and mapped drives.

---

## Infrastructure Design

The Windows environment consists of four core systems:

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
- Service Desk access
- Standard user access

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

### Computer Placement

The built-in `Domain Controllers` OU contains:

- `DC01`
- `DC02`

The custom `Servers` OU contains member servers such as:

- `FS01`

The custom `Workstations` OU contains client devices such as:

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

System Administrators use normal accounts for everyday IT activity.

Examples:

- `anil.barot`
- `daniel.reed`

### Privileged Administrator Accounts

Separate privileged accounts are used for elevated administrative tasks.

Examples:

- `adm.anil.barot`
- `adm.daniel.reed`

These accounts are used for:

- Domain administration
- Server administration
- DNS
- Group Policy
- File services
- RDP
- Privileged PowerShell administration

### Service Desk Accounts

Service Desk users operate using delegated permissions rather than Domain Administrator access.

Examples:

- `maya.patel`
- `aaron.smith`

Their delegated responsibilities include:

- New starter creation
- Password resets
- Account unlocks
- Routine account changes
- Approved group membership management
- User disablement and offboarding support

---

## File Services

FS01 provides centralised departmental storage.

Planned shares include:

`\\FS01\Finance`

`\\FS01\HR`

`\\FS01\Sales`

`\\FS01\IT`

`\\FS01\Public`

Access is controlled using Active Directory security groups, NTFS permissions and SMB share permissions.

Example:

`GG-Finance-Users` receives access to the Finance share while users from other departments are restricted.

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

---

## Backup and Recovery

The environment includes backup and recovery exercises covering:

- Windows Server Backup
- Scheduled backups
- File recovery
- Deleted file restoration
- Recovery testing

A key objective is to demonstrate the difference between backup, recovery and temporary snapshot-style protection.

---

## Monitoring and Administration

Server health and administration will be performed using tools including:

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
- PowerShell

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

---

## Project Phases

1. Azure foundation
2. DC01 deployment
3. Active Directory structure
4. Administrative and Service Desk access
5. DC02 deployment and replication
6. DNS administration
7. FS01 deployment
8. File server storage
9. CLIENT01 deployment
10. Group Policy
11. RDP and remote administration
12. User lifecycle administration
13. Backup and recovery
14. Troubleshooting
15. Monitoring and server health
16. PowerShell administration
17. Security hardening
18. Final infrastructure documentation

---

## Project Objectives

- Design a structured Windows domain environment
- Deploy and configure Windows Server infrastructure
- Implement Active Directory Domain Services
- Implement DNS
- Create users, groups and organisational units
- Separate standard and privileged administrator accounts
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

This repository documents the build process, configuration decisions, screenshots, troubleshooting exercises, commands and lessons learned throughout the implementation of the Obsidian Windows infrastructure environment.
