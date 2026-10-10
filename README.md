# Homelab: Linux and Windows Server Lab

A small virtual lab built in VirtualBox to practice server administration, networking, and IT support workflows.

## Network
VirtualBox NAT Network "KsLabnet", 10.10.10.0/24, gateway 10.10.10.1, DHCP enabled.

## Hosts
| Host | Role | OS | Address |
|---|---|---|---|
| ksserver | Linux server | Ubuntu Server 24.04.5 LTS | 10.10.10.3 (DHCP, static planned) |

## Build log
- ksserver: installed with LVM and OpenSSH, user admink1, fully updated, baseline recorded (9.8 GB root volume, 1.9 GB RAM), snapshot "fresh-install" taken.
- ksserver2: cloned from ksserver; hostname, machine ID, and SSH host keys regenerated so the clone has its own identity.
