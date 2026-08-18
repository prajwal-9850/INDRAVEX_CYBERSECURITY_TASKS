# Task 2 — Linux Server Hardening 🔐

## Objective

Hardened a fresh **Ubuntu Server 26.04 LTS** installation by implementing essential security measures and verifying each configuration.

##  Tools Used

* Ubuntu Server 26.04 LTS
* UFW Firewall
* Fail2Ban
* OpenSSH
* unattended-upgrades
* libpam-pwquality

##  Tasks Performed

* Created a separate sudo user
* Disabled root SSH login
* Configured and enabled UFW firewall
* Installed and tested Fail2Ban against SSH brute-force attempts
* Enabled automatic security updates
* Reviewed password policy settings
* Verified each configuration using appropriate commands

##  Key Findings

The default server had no active firewall, no brute-force protection, and relied on default password settings. Hardening significantly reduced the server's exposed attack surface and improved its overall security posture.

##  Recommendations

* Keep root SSH login disabled
* Review firewall rules regularly
* Prefer SSH key-based authentication
* Enforce stronger password requirements
* Monitor automatic update logs

##  Documentation

Screenshots and the detailed task report are included in this repository.

##  Demo

**LinkedIn:** [https://www.linkedin.com/posts/prajwal-wanave-07a434370_indravextechnologies-cybersecurity-internship-ugcPost-7495532652972654592-Y2GE/?utm_source=share&utm_medium=member_desktop&rcm=ACoAAFvmfl4Bs7H0HSN5E99tZrZ4SntHc4JBFTo]

##  Internship

**Indravex Technologies — Cyber Security Internship**
**Task 2: Linux Server Hardening**
