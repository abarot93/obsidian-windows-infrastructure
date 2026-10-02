# Phase 6 — DNS Administration

## Overview

The sixth stage of the Obsidian infrastructure project was to review, validate and test Domain Name System (DNS) services across the Windows environment.

The objective was to confirm that both Domain Controllers could resolve internal domain names, provide DNS redundancy and forward external DNS requests correctly.

DNS is a critical part of Active Directory because domain-joined systems rely on it to locate Domain Controllers and other network resources.

---

## DNS Infrastructure

DNS is installed on both Domain Controllers:

- `DC01`
- `DC02`

The Active Directory domain is:

`obsidian.local`

The Domain Controllers use the following private IP addresses:

- `DC01` → `172.16.10.10`
- `DC02` → `172.16.10.11`

Both Domain Controllers host DNS services for the `obsidian.local` domain.

---

## Forward Lookup Zone

The DNS Manager console was used to review the following Forward Lookup Zone:

`obsidian.local`

The zone contains DNS records for the Domain Controllers.

### DC01

Hostname:

`dc01.obsidian.local`

IPv4 address:

`172.16.10.10`

### DC02

Hostname:

`dc02.obsidian.local`

IPv4 address:

`172.16.10.11`

These records allow systems on the network to resolve server names to their corresponding IP addresses.

---

## A Records

The DNS records for DC01 and DC02 are A records.

An A record maps a hostname to an IPv4 address.

Examples:

`dc01.obsidian.local` → `172.16.10.10`

`dc02.obsidian.local` → `172.16.10.11`

This allows users and systems to communicate using server names instead of remembering IP addresses.

---

## Internal DNS Testing

Internal DNS resolution was tested using `nslookup`.

The following queries were tested:

`nslookup dc01.obsidian.local`

`nslookup dc02.obsidian.local`

The tests confirmed that both Domain Controller hostnames resolved to the correct private IP addresses.

---

## DNS Redundancy Testing

Both Domain Controllers were tested directly as DNS servers.

Queries were sent to DC01:

`nslookup dc01.obsidian.local 172.16.10.10`

`nslookup dc02.obsidian.local 172.16.10.10`

Queries were also sent to DC02:

`nslookup dc01.obsidian.local 172.16.10.11`

`nslookup dc02.obsidian.local 172.16.10.11`

Both DNS servers successfully resolved both Domain Controller hostnames.

This confirms that DNS services are available across both Domain Controllers and provides redundancy for internal name resolution.

---

## DNS Forwarding

The DNS forwarder configuration on DC02 was reviewed.

The configured forwarder is:

`168.63.129.16`

This is the Azure DNS resolver used by the virtual network.

The forwarder allows the internal DNS server to resolve external names that are not part of the `obsidian.local` domain.

For example:

Internal request:

`dc01.obsidian.local`

is answered by the Obsidian DNS server.

External request:

`microsoft.com`

is forwarded to the Azure DNS resolver.

---

## External DNS Testing

External name resolution was tested using:

`nslookup microsoft.com`

This confirmed that DC02 could resolve external DNS names through the configured forwarder.

A ping test was also used to confirm that external hostnames could be translated into IP addresses.

The important part of the test was successful name resolution, as external systems may block ICMP ping responses.

---

## DNS Redundancy Design

The DNS environment now contains two DNS servers:

### DC01

- Active Directory-integrated DNS
- Internal name resolution
- Domain Controller DNS records
- External DNS forwarding

### DC02

- Active Directory-integrated DNS
- Internal name resolution
- Domain Controller DNS records
- External DNS forwarding
- Secondary DNS availability

This means internal DNS is not dependent on a single Domain Controller.

---

## Evidence

### DNS Zone Records

[DNS Zone Records](https://github.com/abarot93/obsidian-windows-infrastructure/blob/main/06-dns/dns-zone-records.png) ([image](https://github.com/abarot93/obsidian-windows-infrastructure/raw/main/06-dns/dns-zone-records.png))

### DNS Redundancy Test

[DNS Redundancy Test](https://github.com/abarot93/obsidian-windows-infrastructure/blob/main/06-dns/dns-redundancy-test.png) ([image](https://github.com/abarot93/obsidian-windows-infrastructure/raw/main/06-dns/dns-redundancy-test.png))

---

## Phase Outcome

DNS services were successfully validated across both Domain Controllers.

DC01 and DC02 can resolve internal domain names, both servers can provide DNS responses for the `obsidian.local` domain and external DNS queries are forwarded successfully through the Azure DNS resolver.

This provides reliable and redundant name resolution for the Obsidian infrastructure.
