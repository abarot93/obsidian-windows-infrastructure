# Phase 10 — Group Policy

## Overview

The tenth stage of the Obsidian infrastructure project focused on implementing centralised configuration and access control using Group Policy.

Group Policy was used to automatically map departmental network drives based on Active Directory security group membership and to apply a workstation security setting to `CLIENT01`.

This phase demonstrated how Active Directory, Group Policy, security groups, file-server permissions and workstation configuration can work together to centrally manage users and devices.

---

## Group Policy Management

Group Policy Management was configured from `DC01`.

Two Group Policy Objects were used during this phase:

- `GPO-Workstation-Mapped-Drives`
- `GPO-Workstation-Security`

The mapped-drive policy was used for user-based configuration, while the workstation security policy was used for computer-based configuration.

---

## Departmental Mapped Drives

The following departmental network drives were configured using Group Policy Preferences.

### Finance

Drive letter:

`F:`

Network path:

`\\FS01\Finance`

Target security group:

`GG-Finance-Users`

---

### HR

Drive letter:

`H:`

Network path:

`\\FS01\HR`

Target security group:

`GG-HR-Users`

---

### Sales

Drive letter:

`S:`

Network path:

`\\FS01\Sales`

Target security group:

`GG-Sales-Users`

---

## Item-Level Targeting

Item-level targeting was configured for each mapped drive.

This ensured that users only received the network drive associated with their department.

For example:

- Finance users receive `F:`
- HR users receive `H:`
- Sales users receive `S:`

A Finance user therefore does not automatically receive access to the HR or Sales network drives.

This provides centralised access control using Active Directory security group membership.

---

## GPO Linking

The mapped-drive GPO was linked to the departmental organisational units containing the user accounts:

- `Finance`
- `HR`
- `Sales`

This allowed the user-side Group Policy Preferences to apply to users located within those OUs.

The workstation security GPO was linked to:

`Workstations`

This allowed computer-based security settings to apply to devices such as `CLIENT01`.

---

## Finance User Testing

A Finance user account was used to validate the mapped-drive configuration.

Test account:

`OBSIDIAN\sarah.green`

The following command was used to refresh Group Policy:

`gpupdate /force`

The applied policies were then reviewed using:

`gpresult /r`

The results confirmed that:

`GPO-Workstation-Mapped-Drives`

was successfully applied to the Finance user.

The account was also confirmed as a member of:

`GG-Finance-Users`

---

## Finance Drive Validation

After Group Policy was applied, the Finance user successfully received:

`F:`

mapped to:

`\\FS01\Finance`

The mapped drive was opened successfully from `CLIENT01`.

The following command was also used to review active network mappings:

`net use`

The results confirmed that the Finance user received the Finance drive without receiving the HR or Sales mapped drives.

---

## HR and Sales Validation

Additional departmental users were tested to confirm that item-level targeting was functioning correctly.

HR users received:

`H:` → `\\FS01\HR`

Sales users received:

`S:` → `\\FS01\Sales`

Users did not receive drives belonging to other departments.

This confirmed that the Group Policy configuration was correctly using Active Directory group membership to determine access.

---

## Troubleshooting

During testing, the Finance mapped drive initially failed to appear.

Group Policy itself was confirmed as working because:

`gpresult /r`

showed:

`GPO-Workstation-Mapped-Drives`

under the applied user policies.

The Finance user was also confirmed as a member of:

`GG-Finance-Users`

A manual connection test was then performed against:

`\\FS01\Finance`

The connection failed because `FS01` was not running.

After starting `FS01` and refreshing Group Policy, the Finance mapped drive appeared successfully.

This demonstrated the importance of separating Group Policy problems from underlying server, network and service availability issues during troubleshooting.

---

## Workstation Security Policy

A second Group Policy Object was created:

`GPO-Workstation-Security`

This GPO was linked to the:

`Workstations`

OU.

The following security setting was configured:

`Interactive logon: Machine inactivity limit`

Value:

`900 seconds`

This causes the workstation to lock after approximately 15 minutes of inactivity.

---

## Workstation Policy Validation

The workstation policy was tested on `CLIENT01`.

The following command was used:

`gpupdate /force`

The applied computer policies were then reviewed using:

`gpresult /r`

The output confirmed:

`GPO-Workstation-Security`

was successfully applied to:

`CLIENT01`

The workstation computer object was also confirmed as being located in:

`OU=Workstations,DC=obsidian,DC=local`

---

## Group Policy Testing Commands

Commands used during this phase included:

`gpupdate /force`

Used to immediately refresh Group Policy.

`gpresult /r`

Used to identify which Group Policy Objects were applied to the user and computer.

`net use`

Used to review mapped network drives.

These commands provided a simple method for validating and troubleshooting Group Policy deployment.

---

## Evidence

### Mapped Drive Configuration

![GPO Mapped Drives](gpo-mapped-drives.png)

This screenshot shows the Finance, HR and Sales mapped-drive configurations created using Group Policy Preferences.

---

### Finance Mapped Drive

![Finance Mapped Drive](finance-mapped-drive.png)

This screenshot shows the Finance network drive successfully mapped to `F:` on `CLIENT01`.

---

### Workstation Security GPO

![Workstation Security GPO](workstation-security-gpo.png)

This screenshot shows `GPO-Workstation-Security` successfully applying to `CLIENT01`.

---

## Phase Outcome

Group Policy was successfully implemented within the Obsidian environment.

The completed configuration provides:

- centralised network-drive mapping
- department-based drive assignment
- security group-based item-level targeting
- Finance, HR and Sales drive separation
- centralised workstation security configuration
- automatic workstation inactivity locking
- Group Policy validation using `gpupdate` and `gpresult`
- practical Group Policy troubleshooting

The environment can now centrally configure users and workstations while maintaining departmental access separation through Active Directory and Group Policy.
