# Phase 4 — Administrative and Service Desk Delegation

## Overview

The fourth stage of the Obsidian infrastructure project was to configure role-based administrative access for the IT team.

The objective was to separate full System Administrator permissions from routine Service Desk responsibilities using separate privileged accounts, Active Directory security groups and delegated permissions.

This follows the principle of least privilege by ensuring users only receive the access required for their role.

---

## IT Administration Model

### System Administrators

The System Administrators are:

- Anil Barot
- Daniel Reed

Each System Administrator has:

- a normal day-to-day IT account
- a separate privileged administrator account

Privileged accounts:

- `adm.anil.barot`
- `adm.daniel.reed`

These accounts are used for elevated administrative tasks.

### Privileged Responsibilities

The System Administrator accounts are responsible for:

- Active Directory administration
- Domain Controller administration
- DNS management
- Group Policy administration
- server administration
- file server administration
- RDP access to authorised servers
- PowerShell administration
- user and group management
- permissions management
- infrastructure troubleshooting

---

## Service Desk Team

The Service Desk users are:

- `maya.patel`
- `aaron.smith`

These users are members of:

`GG-ServiceDesk`

Service Desk accounts are not given unrestricted Domain Administrator access.

Instead, permissions are delegated for routine user administration.

---

## Service Desk Delegated Permissions

The Service Desk team is allowed to perform tasks such as:

- create new user accounts
- reset user passwords
- unlock user accounts
- update routine user information
- disable user accounts
- manage approved group membership
- support new starter processes
- support leaver processes

The Service Desk team is not permitted to:

- administer Domain Controllers
- manage DNS
- make unrestricted Group Policy changes
- change domain-wide security configuration
- assign themselves Domain Administrator privileges
- receive unrestricted server administration access

---

## Administrative Security Groups

The following groups are used to control IT administrative access:

### `GG-ServiceDesk`

Contains Service Desk users who require delegated user-management permissions.

Members:

- `maya.patel`
- `aaron.smith`

### `GG-Server-Admins`

Contains privileged System Administrator accounts used for server administration.

Members:

- `adm.anil.barot`
- `adm.daniel.reed`

### `GG-RDP-Servers`

Controls which privileged users are allowed to remotely access authorised Windows servers.

Members:

- `adm.anil.barot`
- `adm.daniel.reed`

---

## Domain Administration

The privileged System Administrator accounts are given the required domain-level permissions for the lab environment.

Normal day-to-day accounts remain standard accounts and are not used for privileged administration.

This creates separation between:

**Standard account → everyday IT activity**

and

**Privileged account → administrative activity**

---

## Least-Privilege Design

The administrative model was designed so that:

- normal users have no administrative rights
- Service Desk users receive only delegated support permissions
- System Administrators use separate privileged accounts
- privileged access is controlled through security groups
- routine work is not performed using highly privileged accounts

This reduces unnecessary administrative access and creates a clearer separation of responsibilities within the IT team.

---

## Evidence

### Administrative Groups and Membership

![Administrative Groups](admin-groups.png)

### Service Desk Delegation

![Service Desk Delegation](service-desk-delegation.png)

---

## Phase Outcome

A role-based IT administration model was successfully implemented.

System Administrators have separate privileged accounts for infrastructure administration, while Service Desk users have delegated permissions for routine account-management tasks without receiving unrestricted Domain Administrator access.
