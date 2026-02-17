# Mastering VM Identity & Cloning  
Building Clean, Repeatable Security Labs with Windows & Linux

---

## Overview

Reinstalling operating systems every time you build a lab is inefficient.

This repository documents a structured approach to building scalable, identity-safe virtual lab environments using:

- Windows Sysprep (SID reset)
- Linux machine-id regeneration
- Controlled cloning
- Snapshot strategy
- Template-based deployment

---

## Why Identity Matters

Cloning virtual machines without resetting identity artifacts can cause:

- Duplicate Windows SIDs
- Active Directory conflicts
- Duplicate Linux machine-id
- SSH fingerprint collisions
- Log correlation inconsistencies
- Infrastructure confusion in SOC environments

Clean identity = clean lab architecture.

---

## Windows Workflow

1. Build a fully configured base Windows machine.
2. Run Sysprep with:
   - OOBE
   - Generalize
   - Shutdown
3. Clone using virt-manager.
4. First boot generates a new SID.
5. Rename and join domain safely.

---

## Linux Workflow

After cloning:

```bash
sudo rm -f /etc/machine-id
sudo systemd-machine-id-setup


If SSH is installed:

sudo rm -f /etc/ssh/ssh_host_*
sudo dpkg-reconfigure openssh-server

Then update hostname:

sudo hostnamectl set-hostname client01

