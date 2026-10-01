# Phase 3 — Active Directory Structure

## Overview

The third stage of the project was to design and build the Active Directory organisational structure for Obsidian.

The objective was to organise users, computers and administrative accounts into clear organisational units and use security groups to control access and administration.

## Organisational Units

The following OUs were created:

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
- `Servers` — member servers such as FS01
- `Workstations` — client devices such as CLIENT01
- `Admin Accounts` — privileged administrator accounts

Domain Controllers remain in the built-in:

`Domain Controllers`

OU.

## Security Groups

Department groups:

- `GG-IT-Users`
- `GG-Finance-Users`
- `GG-HR-Users`
- `GG-Sales-Users`

Administration groups:

- `GG-ServiceDesk`
- `GG-Server-Admins`
- `GG-RDP-Servers`

## IT Accounts

### System Administrators

Normal accounts:

- `anil.barot`
- `daniel.reed`

Privileged accounts:

- `adm.anil.barot`
- `adm.daniel.reed`

### Service Desk

- `maya.patel`
- `aaron.smith`

## Department Users

### Finance

- Sarah Green
- Liam Brown
- Emma Wilson

### HR

- James Taylor
- Nina Shah
- Olivia Moore

### Sales

- Ethan Clark
- Chloe Adams
- Ryan Scott

## Group Membership

All normal IT users are members of:

`GG-IT-Users`

Service Desk staff are members of:

`GG-ServiceDesk`

Privileged administrator accounts are members of:

- `GG-Server-Admins`
- `GG-RDP-Servers`

Department users are members of their respective department security groups.

## Evidence

### Active Directory Structure

![Active Directory Structure](active-directory-structure.png)

## Phase Outcome

The Active Directory organisational structure was created with departmental OUs, users, security groups and separate privileged administrator accounts.
