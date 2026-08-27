# Task 3 — Network Reconnaissance

Scanned my own VMware lab network with Nmap — host discovery first, then a full port/service/OS scan against a specific target.

## Objective
Find out what's alive on a local network, then dig into one host to identify open ports, running services, and OS.

## Environment
- **Network:** VMware NAT subnet, 192.168.181.0/24 (local lab only)
- **Scanning machine:** Kali Linux VM
- **Target:** Ubuntu Server VM from [Task 2](../Task2)

## What was done
1. Found my own subnet with `ip a`
2. Swept the whole subnet with `nmap -sn` to find live hosts
3. Identified the target IP and ran a full scan: `nmap -sS -sV -O -p-`

## Hosts discovered

| IP | What it is |
|---|---|
| 192.168.181.1 | VMware virtual gateway |
| 192.168.181.2 | VMware NAT/DHCP service |
| 192.168.181.131 | Kali VM (scanning machine) |
| 192.168.181.132 | Ubuntu Server VM — the target |
| 192.168.181.254 | VMware internal service |

## Findings on the target (192.168.181.132)

| Port | Service | Notes |
|---|---|---|
| 22/tcp | OpenSSH 10.2p1 (Ubuntu) | Only open port — everything else filtered by UFW |

OS detection came back inconclusive — a direct side effect of the firewall filtering rather than closing unused ports, which didn't give Nmap a clean signal to fingerprint against.

## Files in this folder
- `task3_nmap_results.txt` — raw scan output


## Video
[LinkedIn demo video]()

## Note
This scan targeted my own local lab VMs only — no external or third-party network was touched.
