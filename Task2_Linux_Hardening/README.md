# Task 2 — Linux Server Hardening

## Overview

This project was completed as **Task 2 of the Cyber Security Internship at Indravex Technologies**.

The objective was to take a **fresh Ubuntu Server 26.04 LTS installation** and apply essential security hardening measures before deploying it in a real environment.

The hardening focused on:

* User access control
* SSH security
* Firewall configuration
* Brute-force protection
* Automatic security updates
* Password policy

All testing was performed on an authorized **local/self-hosted Ubuntu Server virtual machine**.

---

## 🎯 Objective

To secure a fresh, default Ubuntu Server installation by implementing fundamental server-hardening practices and verifying that each security configuration was successfully applied.

The main areas covered were:

1. Creating a separate administrative user
2. Disabling root SSH login
3. Configuring the UFW firewall
4. Installing and testing Fail2Ban
5. Enabling automatic security updates
6. Reviewing password policy configuration

---

## 🖥️ Target Environment

| Component        | Details                                           |
| ---------------- | ------------------------------------------------- |
| Operating System | Ubuntu Server 26.04 LTS                           |
| Environment      | Fresh Virtual Machine                             |
| Target           | Local / Self-hosted                               |
| Testing Machine  | Kali Linux VM                                     |
| Internship       | Indravex Technologies – Cyber Security Internship |
| Task             | Task 2 – Linux Server Hardening                   |

---

## 🛠️ Tools Used

### UFW

**Uncomplicated Firewall** was used to control network access to the server.

### Fail2Ban

Used to provide protection against repeated failed authentication attempts and brute-force attacks.

### OpenSSH

Used for remote access and hardened by disabling direct root login.

### unattended-upgrades

Used to enable automatic security updates.

### libpam-pwquality

Used to provide password-quality policy support and review the existing password configuration.

---

# 🔧 Methodology

The server was hardened progressively, starting with access control and moving toward network protection and long-term maintenance.

Each configuration was followed by a verification command to confirm that the change was actually applied successfully.

### Hardening Workflow

```text
Fresh Ubuntu Server
        ↓
Create Separate User
        ↓
Disable Root SSH Login
        ↓
Configure UFW Firewall
        ↓
Install & Test Fail2Ban
        ↓
Enable Automatic Updates
        ↓
Review Password Policy
        ↓
Verify Configurations
```

---

# 1. 👤 Create a New User

A separate user account was created instead of relying exclusively on the original installation account.

### Commands

```bash
sudo adduser prajwal
```

The user was then added to the sudo group:

```bash
sudo usermod -aG sudo prajwal
```

### Verification

```bash
groups prajwal
```

This confirmed that the new user had been successfully added to the appropriate administrative group.

### Security Benefit

Using a separate administrative account improves user separation and reduces dependence on a single account with administrative privileges.

---

# 2. 🔐 Disable Root SSH Login

Direct root login through SSH was disabled.

The SSH configuration file was modified:

```text
/etc/ssh/sshd_config
```

The following configuration was applied:

```text
PermitRootLogin no
```

The SSH service was then restarted:

```bash
sudo systemctl restart ssh
```

### Verification

```bash
sudo sshd -T | grep permitrootlogin
```

This was used to confirm that root SSH login had been explicitly disabled.

### Security Benefit

Disabling direct root SSH access reduces exposure to attacks targeting the root account, which is a common target during automated brute-force attempts.

---

# 3. 🧱 Configure UFW Firewall

The UFW firewall was installed and enabled.

### Installation

```bash
sudo apt install ufw -y
```

SSH access was allowed before enabling the firewall:

```bash
sudo ufw allow ssh
```

The firewall was then enabled:

```bash
sudo ufw enable
```

### Verification

```bash
sudo ufw status verbose
```

This confirmed the firewall status and configured rules.

### Security Benefit

The default server installation had no active firewall, meaning network services could potentially be reachable without network-level filtering.

Activating UFW reduces the server's exposed attack surface.

---

# 4. 🛡️ Install and Test Fail2Ban

Fail2Ban was installed to provide protection against repeated failed authentication attempts.

### Installation

```bash
sudo apt install fail2ban -y
```

The service was enabled and started:

```bash
sudo systemctl enable --now fail2ban
```

### Testing

The protection was actually tested by deliberately generating failed SSH login attempts from the Kali Linux VM.

The Fail2Ban SSH jail status was then checked:

```bash
sudo fail2ban-client status sshd
```

The test confirmed that the originating IP address was banned.

### Security Benefit

Fail2Ban helps limit automated password-guessing and brute-force attempts by temporarily blocking sources that generate repeated failed authentication attempts.

---

# 5. 🔄 Enable Automatic Security Updates

The `unattended-upgrades` package was installed to support automatic security patching.

### Installation

```bash
sudo apt install unattended-upgrades -y
```

Automatic updates were configured using:

```bash
sudo dpkg-reconfigure --priority=low unattended-upgrades
```

### Verification

The configuration was checked using:

```bash
cat /etc/apt/apt.conf.d/20auto-upgrades
```

This was used to verify the automatic-update configuration.

### Security Benefit

Automatic security updates reduce the amount of time a system remains exposed to known vulnerabilities requiring security patches.

---

# 6. 🔑 Password Policy Review

The `libpam-pwquality` package was installed:

```bash
sudo apt install libpam-pwquality -y
```

The existing password-related configuration was reviewed using:

```bash
cat /etc/login.defs | grep PASS
```

This step was used to inspect the current password policy settings.

### Important Observation

The assessment found that the default Ubuntu configuration did not provide the desired level of password complexity enforcement.

## Therefore, a recommendation was made to explicitly enforce password-quality requirements such as minimum length and complexity.

# 🔍 Findings

The following security differences were identified between the default server and the hardened configuration:

| Area                   | Before                                      | After                              | Risk if Left Unfixed        |
| ---------------------- | ------------------------------------------- | ---------------------------------- | --------------------------- |
| User Access            | Only the installation account existed       | Separate sudo user created         | Single point of failure     |
| Root SSH               | Root SSH access was not explicitly disabled | Root SSH login disabled            | Common brute-force target   |
| Firewall               | No active firewall                          | UFW enabled                        | Wide attack surface         |
| Brute-force Protection | No protection                               | Fail2Ban installed and tested      | Unlimited password guessing |
| Patching               | Manual updates required                     | Automatic security updates enabled | Delayed security patches    |
| Password Policy        | Default settings                            | Password policy reviewed           | Weak passwords allowed      |

---

# ⚠️ Risk Analysis

The assessment showed that the main risks were caused by insecure or insufficiently hardened default configurations.

The most significant issue was the absence of an active firewall, which left network services potentially reachable without firewall filtering.

Root SSH access also represented a potential brute-force target, while the absence of brute-force protection allowed repeated authentication attempts without automated blocking.

Automatic updates and password policy improvements primarily strengthen the system's long-term security posture by reducing exposure to known vulnerabilities and weak authentication practices.

---

# 🛡️ Recommendations

The following recommendations were identified:

* Keep root SSH login disabled permanently.
* Review UFW rules whenever new services are added.
* Allow only required network services.
* Consider SSH key-based authentication instead of password authentication.
* Explicitly enforce password-quality requirements such as minimum length and complexity.
* Periodically review `unattended-upgrades` logs to ensure automatic updates are functioning correctly.

---

# 📸 Screenshots

Screenshots documenting the implementation and verification of the hardening steps are included in the report.

The screenshots demonstrate the Ubuntu Server terminal operations, including package installation, SSH configuration, firewall configuration, and automatic-update setup.

The report also documents the verification commands used throughout the process.

---

# 🎥 Demonstration Video

The demonstration video explains the complete hardening process, including:

* Creating the new user
* Disabling root SSH login
* Configuring UFW
* Installing and testing Fail2Ban
* Enabling automatic security updates
* Reviewing password policy
* Verifying the implemented configurations

**LinkedIn Post:** [Add LinkedIn post link]

---

# 📂 Repository Structure

```text
Task-2-Linux-Server-Hardening/
│
├── README.md
│
├── screenshots/
│   ├── user-creation.png
│   ├── disable-root-ssh.png
│   ├── ufw-configuration.png
│   ├── fail2ban.png
│   ├── automatic-updates.png
│   └── password-policy.png
│
└── documentation/
    └── Task-2-Report.pdf
```

---

# 📚 Learning Outcomes

This task provided practical experience in:

* Linux server administration
* Linux security hardening
* User and privilege management
* SSH security
* Firewall configuration
* Brute-force protection
* Automatic security patching
* Password policy assessment
* Security verification and documentation

A key learning from the task was the importance of **verifying every security change rather than assuming that a command succeeded simply because it produced no error**.

---

# ⚠️ Disclaimer

All security testing performed for this task was conducted on an authorized **local/self-hosted Ubuntu Server environment** for educational and internship purposes.

No unauthorized systems or third-party infrastructure were targeted.

---

## 👨‍💻 Internship Information

**Intern:** Prajwal
**Organization:** Indravex Technologies
**Program:** Cyber Security Internship
**Task:** Task 2 — Linux Server Hardening
**Target:** Ubuntu Server 26.04 LTS
**Date:** 17 August 2026
