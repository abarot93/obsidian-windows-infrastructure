# Phase 9 — Build CLIENT01

## Overview

The ninth stage of the Obsidian infrastructure project focused on deploying and configuring the first Windows client device within the environment.

`CLIENT01` was deployed as a Windows 11 Pro virtual machine, connected to the dedicated client subnet, configured to use the Obsidian DNS infrastructure, joined to the `obsidian.local` Active Directory domain and moved into the correct organisational unit.

A normal domain user was then used to successfully sign in to the workstation to confirm domain authentication was working correctly.

---

## CLIENT01 Configuration

The workstation was deployed with the following configuration:

- Hostname: `CLIENT01`
- Operating System: Windows 11 Pro
- Virtual Network: `VNET-Obsidian`
- Subnet: `SNET-Clients`
- Client subnet range: `172.16.20.0/24`
- Private IP address: `172.16.20.10`
- DNS Server: `172.16.10.10`
- Domain: `obsidian.local`

The workstation was placed on a separate client subnet from the server infrastructure to provide clearer separation between server and workstation systems.

---

## DNS Configuration

Before joining the domain, `CLIENT01` was configured to use `DC01` as its DNS server.

DNS configuration:

`CLIENT01` → `172.16.10.10`

This allowed the workstation to resolve the Obsidian Active Directory domain and locate the Domain Controller.

DNS and network connectivity were tested using:

`nslookup obsidian.local`

`ping dc01.obsidian.local`

The successful results confirmed that `CLIENT01` could resolve and communicate with the Obsidian domain infrastructure.

---

## Active Directory Domain Join

`CLIENT01` was successfully joined to:

`obsidian.local`

The domain join confirmed that the workstation could:

- communicate with the Domain Controller
- resolve the Obsidian domain through DNS
- authenticate against Active Directory
- create a computer account within the domain

---

## Workstation OU Placement

After the successful domain join, the `CLIENT01` computer object was moved from the default Active Directory Computers container into the custom:

`Workstations`

Organisational Unit.

This provides a dedicated location for Obsidian workstation devices and will allow workstation-specific Group Policy settings to be applied during the next stage of the project.

---

## Domain User Login

A normal Obsidian domain user account was used to sign in to `CLIENT01`.

Account tested:

`OBSIDIAN\anil.barot`

Remote Desktop access was added to the workstation for the domain user so the login could be tested remotely within the lab environment.

The following commands were used to verify the active user and workstation:

`whoami`

`hostname`

The expected output was:

`obsidian\anil.barot`

`CLIENT01`

This confirmed that the workstation was successfully authenticating a normal user account against the Obsidian Active Directory domain.

---

## Security Design

The workstation was tested using a normal domain user account rather than a privileged administrator account.

This maintains separation between:

- standard user accounts
- privileged administrative accounts
- server administration
- domain administration

Privileged accounts are only used when elevated administrative permissions are required.

---

## Evidence

### CLIENT01 Domain Login

![CLIENT01 Domain Login](client01-domain-login.png)

The screenshot shows a normal Obsidian domain user successfully logged into `CLIENT01`.

The `whoami` command confirms that the session is using the `OBSIDIAN\anil.barot` domain account, while `hostname` confirms that the workstation is `CLIENT01`.

---

## Phase Outcome

`CLIENT01` was successfully deployed as the first Windows workstation within the Obsidian environment.

The workstation:

- uses the dedicated `SNET-Clients` subnet
- uses Obsidian DNS
- is joined to the `obsidian.local` domain
- is located in the `Workstations` OU
- successfully authenticates normal Active Directory users
- is ready to receive centralised workstation configuration

The Obsidian infrastructure is now ready for the next stage of the project: implementing Group Policy.
