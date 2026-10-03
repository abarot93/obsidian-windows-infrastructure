# Phase 9 — Build CLIENT01

## Overview

The ninth stage of the Obsidian infrastructure project focused on deploying and configuring the first Windows client device within the environment.

`CLIENT01` was deployed as a Windows 11 Pro virtual machine, connected to the dedicated client subnet, configured to use the Obsidian DNS infrastructure, joined to the `obsidian.local` Active Directory domain and moved into the correct organisational unit.

A normal domain user was then used to successfully sign in to the workstation to confirm domain authentication was working correctly.
