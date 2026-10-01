# Phase 2 — DC01 Deployment

## Overview

The second stage of the Obsidian infrastructure project was to deploy the first Windows Server and configure it as the primary Domain Controller for the environment.

DC01 provides the core identity and DNS services required by the `obsidian.local` domain.

## VM Configuration

- Hostname: `DC01`
- Operating system: Windows Server 2022 Datacenter: Azure Edition
- Private IP: `172.16.10.10`
- Subnet: `SNET-Servers`
- RDP enabled
- Boot diagnostics enabled
- Auto-shutdown configured

## Server Roles Installed

- Active Directory Domain Services
- DNS Server

## Active Directory Forest

Domain:

`obsidian.local`

NetBIOS name:

`OBSIDIAN`

DC01 was promoted as the first Domain Controller in the new forest.

## Domain Controller Placement

DC01 remains in the built-in:

`Domain Controllers`

OU.

## Evidence

### DC01 Azure Overview

![DC01 Azure Overview](dc01-overview.png)

### Server Manager — AD DS and DNS

![Server Manager AD DS DNS](server-manager-ad-dns.png)

## Phase Outcome

DC01 was successfully deployed, configured with a static private IP and promoted as the first Domain Controller and DNS server for the Obsidian environment.
