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

## Lessons learned
- Netplan YAML needs exact spacing, including a space after each list dash. "netplan generate" catches syntax errors before they are applied, and "netplan try" rolls back automatically if the network breaks.
- Clones share a machine ID and SSH host keys, so both had to be regenerated.
- Take snapshots with the VM fully powered off. A live snapshot taken during a shutdown stalled and had to be redone.
- Keep a second SSH session open when changing SSH settings, so a mistake does not lock you out.

## Planned
- Users, groups, and least-privilege sudo rules
- Knowledge-base articles (password reset, new employee onboarding, SSH access troubleshooting)
- Windows Server with Active Directory
