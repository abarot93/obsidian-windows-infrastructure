# Phase 7 — FS01 File Server Deployment

## Overview

The seventh stage of the Obsidian infrastructure project was to deploy a dedicated Windows Server for centralised file storage.

The objective was to create `FS01`, join it to the `obsidian.local` domain and configure departmental file shares with access controlled through Active Directory security groups.

The server provides shared storage for Finance, HR, Sales, IT and general company use.

---

## FS01 Configuration

A new Windows Server 2022 virtual machine was deployed in Microsoft Azure.

### Server Details

- Hostname: `FS01`
- Operating System: Windows Server 2022 Datacenter: Azure Edition
- Virtual Network: `VNET-Obsidian`
- Subnet: `SNET-Servers`
- Private IP: `172.16.10.20`
- Domain: `obsidian.local`

The Azure network interface was configured with a static private IP to ensure the file server maintains a consistent address.

---

## DNS Configuration

Before joining the server to the domain, FS01 was configured to use DC01 as its DNS server:

`172.16.10.10`

This allowed FS01 to resolve the internal Active Directory domain:

`obsidian.local`

DNS resolution and connectivity were tested using:

`nslookup obsidian.local`

`ping dc01.obsidian.local`

and:

`nltest /dsgetdc:obsidian.local`

These tests confirmed that FS01 could locate and communicate with the Domain Controller.

---

## Domain Join Troubleshooting

During the initial attempt to join FS01 to `obsidian.local`, the domain join failed even though DNS resolution and network connectivity were working correctly.

Further troubleshooting confirmed:

- FS01 could locate DC01
- DNS resolution was working
- Kerberos port 88 was reachable
- LDAP port 389 was reachable
- SMB port 445 was reachable
- RPC port 135 was reachable

The Active Directory domain join log was then reviewed:

`C:\Windows\debug\NetSetup.log`

The log showed that Windows could contact DC01 but failed while attempting to create the FS01 computer account.

The following Active Directory diagnostic was then run on DC01:

`dcdiag /test:ridmanager /v`

The test identified a problem with the RID allocation pool on DC01.

The RID Manager reported an invalid previous allocation pool and was unable to allocate new RIDs.

A RID is part of the unique security identifier used when Active Directory creates security objects such as users, groups and computer accounts.

The RID pool was invalidated and regenerated.

Replication between DC01 and DC02 was also checked using:

`repadmin /replsummary`

After the RID pool was repaired, DC01 passed the RID Manager diagnostic and FS01 successfully joined the domain.

This troubleshooting demonstrated how a domain join issue can originate from Active Directory itself rather than from DNS or basic network connectivity.

---

## Active Directory Placement

After joining the domain, the FS01 computer account was moved into the custom:

`Servers`

OU.

This keeps member servers separated from:

- Domain Controllers
- Workstations
- User accounts

---

## File Server Role

The Windows File Server role was installed on FS01 using Server Manager.

This provides the server functionality required for SMB file sharing and centralised departmental storage.

---

## Departmental Share Structure

The following folders were created:

- `C:\Shares\Finance`
- `C:\Shares\HR`
- `C:\Shares\Sales`
- `C:\Shares\IT`
- `C:\Shares\Public`

The folders were shared as:

- `\\FS01\Finance`
- `\\FS01\HR`
- `\\FS01\Sales`
- `\\FS01\IT`
- `\\FS01\Public`

---

## Share Permissions

Departmental access is controlled through Active Directory security groups.

### Finance

`GG-Finance-Users`

Permissions:

- Change
- Read

### HR

`GG-HR-Users`

Permissions:

- Change
- Read

### Sales

`GG-Sales-Users`

Permissions:

- Change
- Read

### IT

`GG-IT-Users`

Permissions:

- Change
- Read

### Public

`Domain Users`

Permissions:

- Change
- Read

The following administrative group has Full Control:

`GG-Server-Admins`

---

## NTFS Permissions

NTFS permissions were also configured on each departmental folder.

Department users were given:

`Modify`

This allows users to:

- create files
- edit files
- read files
- delete files
- create folders

without allowing them to change folder permissions or take ownership.

`GG-Server-Admins` was given:

`Full Control`

This allows authorised server administrators to manage:

- permissions
- ownership
- file access
- folder configuration

---

## Permission Model

Access to each departmental file share requires both:

- Share permissions
- NTFS permissions

The effective network access is determined by the most restrictive combination of the two permission layers.

This provides a more controlled and structured approach to file access.

---

## Service Desk Access

Service Desk users are not automatically given access to Finance, HR or Sales file shares.

Membership of:

`GG-ServiceDesk`

provides delegated Active Directory support permissions only.

File access is controlled separately through the departmental security groups.

This means Service Desk staff can support user accounts without automatically receiving access to departmental data.

---

## Evidence

### Finance NTFS Permissions

[Finance NTFS Permissions](https://github.com/abarot93/obsidian-windows-infrastructure/blob/main/07-file-server/finance-ntfs-permissions.png) ([image](https://github.com/abarot93/obsidian-windows-infrastructure/raw/main/07-file-server/finance-ntfs-permissions.png))

### FS01 Shares

[FS01 Shares](https://github.com/abarot93/obsidian-windows-infrastructure/blob/main/07-file-server/fs01-shares.png) ([image](https://github.com/abarot93/obsidian-windows-infrastructure/raw/main/07-file-server/fs01-shares.png))

---

## Phase Outcome

FS01 was successfully deployed as the dedicated file server for the Obsidian environment.

The server was joined to the Active Directory domain, placed inside the Servers OU and configured with departmental SMB shares.

Share and NTFS permissions were applied using Active Directory security groups to provide controlled access for Finance, HR, Sales, IT and company-wide users.

A domain join failure was also diagnosed and resolved by identifying and repairing an Active Directory RID allocation issue on DC01.

This phase demonstrated Windows File Server administration, Active Directory integration, permission management and real-world infrastructure troubleshooting.
