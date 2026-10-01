# Phase 5 — DC02 Deployment and Active Directory Replication

## Overview

In this phase, I deployed a second Windows Server 2022 Domain Controller for the Obsidian environment.

The purpose of `DC02` is to provide redundancy for Active Directory, DNS and user authentication.

Having multiple Domain Controllers means the environment is not completely dependent on `DC01` being available.

---

## DC02 Configuration

A second Windows Server virtual machine was deployed in Microsoft Azure.

### Server Details

- Hostname: `DC02`
- Operating System: Windows Server 2022 Datacenter: Azure Edition
- Virtual Network: `VNET-Obsidian`
- Subnet: `SNET-Servers`
- Private IP: `172.16.10.11`
- Domain: `obsidian.local`

The Azure network interface was configured with a static private IP so that DC02 maintains a consistent address.

---

## DNS Configuration

Before joining DC02 to the domain, its DNS configuration was changed to use the existing Domain Controller:

`172.16.10.10`

This is the IP address of `DC01`, which was already providing DNS for the `obsidian.local` domain.

DNS resolution was tested using:

`nslookup obsidian.local`

and:

`ping dc01.obsidian.local`

The tests confirmed that DC02 could resolve and communicate with DC01 before joining the domain.

---

## Domain Join

DC02 was joined to:

`obsidian.local`

Domain credentials were used to authorise the domain join.

After restarting the server, Server Manager confirmed that DC02 was successfully joined to the Obsidian domain.

---

## Active Directory Domain Services

The following Windows Server roles were installed on DC02:

- Active Directory Domain Services
- DNS Server

After installation, DC02 was promoted as an additional Domain Controller in the existing `obsidian.local` domain.

The promotion configuration included:

- DNS Server
- Global Catalog
- Active Directory replication
- SYSVOL replication

DC02 was not configured as a Read Only Domain Controller.

---

## Domain Controller Placement

After promotion, both Domain Controllers were confirmed inside the built-in:

`Domain Controllers`

OU.

The Domain Controllers are:

- `DC01`
- `DC02`

Both servers are also configured as Global Catalog servers.

---

## Active Directory Replication

Replication between DC01 and DC02 was tested using:

`repadmin /replsummary`

The replication summary returned:

- DC01 — 0 replication failures
- DC02 — 0 replication failures
- 0% replication errors

This confirmed that Active Directory replication was operating successfully between both Domain Controllers.

---

## Redundancy

The environment now contains two Domain Controllers.

### DC01

- Active Directory Domain Services
- DNS
- Global Catalog
- Authentication

### DC02

- Active Directory Domain Services
- DNS
- Global Catalog
- Authentication
- Replicated copy of the Active Directory database

This provides additional resilience for core domain services.

---

## Screenshots

### Domain Controllers

![DC01 and DC02 Domain Controllers](dc02-domain-controllers.png)

This screenshot shows both `DC01` and `DC02` inside the Domain Controllers OU and confirms that both are Global Catalog servers.

### Active Directory Replication

![Successful Active Directory Replication](dc02-replication-success.png)

This screenshot shows the output of `repadmin /replsummary` with zero replication failures between DC01 and DC02.

---

## Key Skills Demonstrated

- Windows Server 2022 deployment
- Azure Virtual Machines
- Azure virtual networking
- Static private IP configuration
- DNS client configuration
- Active Directory domain joining
- Active Directory Domain Services installation
- Additional Domain Controller deployment
- DNS Server installation
- Global Catalog configuration
- Active Directory replication
- `repadmin`
- Domain Controller redundancy
- Windows Server administration

---

## Outcome

DC02 was successfully deployed and promoted as the second Domain Controller for the Obsidian environment.

Active Directory and DNS services are now available across two Domain Controllers, with successful replication confirmed between `DC01` and `DC02`.

The next phase focuses on reviewing and administering DNS across the domain.
