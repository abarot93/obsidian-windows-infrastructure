# Phase 8 — File Server Storage

## Overview

The eighth stage of the Obsidian infrastructure project was to move departmental file storage away from the Windows operating system disk and onto a dedicated data disk.

The objective was to separate the operating system from business data, create a dedicated NTFS data volume and migrate the existing departmental SMB shares onto the new storage.

This provides a cleaner and more realistic Windows File Server design.

---

## Azure Data Disk

A new managed disk was created and attached to `FS01`.

### Disk Details

- Server: `FS01`
- Disk name: `FS01-Data01`
- Disk type: Standard SSD
- Disk size: 64 GB

The disk is used specifically for departmental file storage.

---

## Disk Initialisation

After the disk was attached in Azure, it was configured inside Windows Server.

The disk was:

- brought online
- initialised using GPT
- formatted using NTFS
- assigned drive letter `D:`
- labelled `Data`

The final volume configuration is:

`D:\`

Volume label:

`Data`

---

## Storage Structure

A dedicated folder structure was created on the new data volume.

The departmental folders are stored under:

`D:\Shares`

The final structure is:

- `D:\Shares\Finance`
- `D:\Shares\HR`
- `D:\Shares\Sales`
- `D:\Shares\IT`
- `D:\Shares\Public`

This separates business data from the Windows operating system stored on `C:`.

---

## Share Migration

The departmental folders were originally created under:

`C:\Shares`

After the dedicated data disk was introduced, the folders were moved to:

`D:\Shares`

The SMB shares were then recreated so that the existing network paths remained unchanged.

### Network Shares

- `\\FS01\Finance`
- `\\FS01\HR`
- `\\FS01\Sales`
- `\\FS01\IT`
- `\\FS01\Public`

Users can therefore continue using the same network paths even though the underlying storage location changed.

---

## NTFS Permissions

Because the folders were moved between different volumes, the NTFS permissions were reviewed and reapplied.

The following departmental permissions were configured:

### Finance

`GG-Finance-Users`

Permission:

`Modify`

### HR

`GG-HR-Users`

Permission:

`Modify`

### Sales

`GG-Sales-Users`

Permission:

`Modify`

### IT

`GG-IT-Users`

Permission:

`Modify`

### Public

`Domain Users`

Permission:

`Modify`

The administrative group:

`GG-Server-Admins`

was given:

`Full Control`

on all departmental folders.

---

## Share Permissions

The SMB share permissions were also reviewed after the migration.

Department groups were given:

- Change
- Read

`GG-Server-Admins` was given:

- Full Control

This maintains the same access model used before the storage migration.

---

## Permission Validation

Both permission layers were checked after the folders were moved:

- Share permissions
- NTFS permissions

This was important because moving folders between different Windows volumes can affect NTFS inheritance.

The permissions were therefore validated and reapplied where required.

---

## SMB Validation

The configured shares were verified using PowerShell:

`Get-SmbShare`

This confirmed that all departmental shares were now pointing to the new data volume.

Examples:

`Finance` → `D:\Shares\Finance`

`HR` → `D:\Shares\HR`

`Sales` → `D:\Shares\Sales`

`IT` → `D:\Shares\IT`

`Public` → `D:\Shares\Public`

---

## File Share Access Testing

SMB access was tested using the Finance share:

`\\FS01\Finance`

Initial testing using the setup account was denied because the account was not a member of an authorised file-access group.

The share permissions were reviewed using:

`Get-SmbShareAccess -Name Finance`

This confirmed that access was restricted to:

- `GG-Finance-Users`
- `GG-Server-Admins`

The share was then tested using an authorised server administrator account.

The following command successfully returned the contents of the Finance share:

`dir \\FS01\Finance`

This confirmed that:

- SMB access was working
- the share was pointing to the `D:` volume
- the security groups were being enforced correctly
- the permissions were operating as designed

---

## Storage Design

The final FS01 storage model is:

### Operating System

`C:`

Used for:

- Windows Server
- installed roles
- system files

### Business Data

`D:`

Used for:

- departmental folders
- SMB shares
- company file storage

Separating these workloads makes the server easier to manage and provides a more appropriate design for a dedicated file server.

---

## Evidence

### Dedicated Data Volume

[FS01 Data Volume](https://github.com/abarot93/obsidian-windows-infrastructure/blob/main/08-storage/fs01-data-volume.png) ([image](https://github.com/abarot93/obsidian-windows-infrastructure/raw/main/08-storage/fs01-data-volume.png))

### Shares Using the Data Disk

[FS01 Shares Data Disk](https://github.com/abarot93/obsidian-windows-infrastructure/blob/main/08-storage/fs01-shares-data-disk.png) ([image](https://github.com/abarot93/obsidian-windows-infrastructure/raw/main/08-storage/fs01-shares-data-disk.png))

---

## Phase Outcome

FS01 was successfully configured with a dedicated data disk for departmental file storage.

The new disk was initialised using GPT, formatted with NTFS and mounted as the `D:` drive.

The existing Finance, HR, Sales, IT and Public shares were migrated from the operating system drive to `D:\Shares` while maintaining their existing network paths.

NTFS and SMB permissions were reviewed and reapplied after the migration, and file access was successfully validated using an authorised domain account.

The Obsidian file server now has a cleaner separation between the Windows operating system and business data.
