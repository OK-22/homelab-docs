# Homelab: Linux and Windows Server Lab

A small virtual lab built in VirtualBox to practice server administration, networking, and IT support workflows.

## Network
VirtualBox NAT Network "KsLabnet", 10.10.10.0/24, gateway 10.10.10.1, DHCP enabled. Servers use static addresses, and SSH is reached from the Windows host through port forwarding.

## Hosts
| Host | Role | OS | Address | SSH from host |
|---|---|---|---|---|
| ksserver | Linux server | Ubuntu Server 24.04.5 LTS | 10.10.10.11 (static) | ssh ksserver (host port 2211) |
| ksserver2 | Linux server (clone) | Ubuntu Server 24.04.5 LTS | 10.10.10.12 (static) | ssh ksserver2 (host port 2212) |

## Build log
- ksserver: installed with LVM and OpenSSH, user admink1, fully updated, baseline recorded (9.8 GB root volume, 1.9 GB RAM), snapshot "fresh-install" taken.
- ksserver2: cloned from ksserver; hostname, machine ID, and SSH host keys regenerated so the clone has its own identity.
- Static addressing: netplan on both servers, with cloud-init network config disabled so the setting survives reboots.
- Remote access: NAT Network port-forward rules for SSH (host ports 2211 and 2212), tested from the Windows host.
- SSH keys: ed25519 key pair created on the Windows host, public key installed on both servers, host aliases added in the SSH config file.
- SSH hardening: password authentication and root login disabled, key-only access, MaxAuthTries 3 (drop-in file 01-hardening.conf), verified from the Windows host with a key-less login attempt that was refused.
- Accounts (both servers): groups helpdesk and webadmins; users mlopez and jsmith created with temporary passwords and a forced change at first login.
- Least-privilege sudo (both servers): the helpdesk group may only run "systemctl is-active ssh" and "systemctl restart ssh". The rule was validated with visudo before install, and a disallowed command (apt update) was refused as expected.

## Lessons learned
- Netplan YAML needs exact spacing, including a space after each list dash. "netplan generate" catches syntax errors before they are applied, and "netplan try" rolls back automatically if the network breaks.
- Clones share a machine ID and SSH host keys, so both had to be regenerated.
- Take snapshots with the VM fully powered off. A live snapshot taken during a shutdown stalled and had to be redone.
- Keep a second SSH session open when changing SSH settings, so a mistake does not lock you out.
- Sudo matches commands exactly, including arguments, and "systemctl status" can open a pager that runs as root, so the helpdesk rule uses "is-active" instead.
- A VM that would not start, with VERR_ACCESS_DENIED in the log, had a folder its user could not write to; granting the user Modify rights with icacls from an elevated shell fixed it.
- A VM that hung at "Loading essential drivers" after an aborted session recovered with a hard reset; "VBoxManage controlvm screenshotpng" showed the console state without opening the GUI.

## Planned
## Knowledge base
- [KB-01: Reset a Linux user's password and force a change](kb-01-password-reset.md)
- [KB-02: Onboard a new employee (and offboard)](kb-02-new-employee-onboarding.md)
- [KB-03: Troubleshoot an SSH access problem](kb-03-ssh-access-troubleshooting.md)
- Active Directory (kslab.local): OU structure (KSLab with Users, Groups, Computers), security groups HelpDesk and WebAdmins, and users mlopez and jsmith created with a generated temporary password and a forced change at first login. Group membership assigned with PowerShell.
