# 🔐 Metasploit Bind & Reverse Shell Lab

A hands-on cybersecurity lab demonstrating **Reverse TCP, Bind TCP, Metasploit Multi/Handler, Meterpreter payload architecture, and Post-Exploitation enumeration** in an isolated virtual environment using Kali Linux and Metasploitable 2.

> ⚠️ **Lab Disclaimer:** This project was performed in an isolated, intentionally vulnerable lab environment using Kali Linux and Metasploitable 2. The techniques demonstrated here should only be used on systems where you have explicit authorization.

---
This project demonstrates practical experience with:

![Multi/Handler](https://img.shields.io/badge/Multi%2FHandler-Metasploit-red)
![Reverse TCP](https://img.shields.io/badge/Reverse-TCP-orange)
![Bind TCP](https://img.shields.io/badge/Bind-TCP-blue)
![Meterpreter](https://img.shields.io/badge/Meterpreter-Metasploit-purple)
![msfvenom](https://img.shields.io/badge/msfvenom-Payload-green)
![Post Exploitation](https://img.shields.io/badge/Post--Exploitation-Analysis-yellow)

## 📌 Project Overview

This project explores how attackers and security professionals can establish remote shell communication with a vulnerable Linux system using the Metasploit Framework.

The lab covers two major shell communication models:

- 🔄 **Reverse TCP Shell** — the target initiates a connection back to the attacker/listener.
- 🔗 **Bind TCP Shell** — the target listens for an incoming connection from the attacker.
- 🎯 **Metasploit Multi/Handler** — used to configure and manage incoming connections.
- 🖥️ **Meterpreter** — studied as a Metasploit payload/session framework.
- 🔎 **Post-Exploitation Enumeration** — gathering system and network information after authorized access.

The project also demonstrates architecture matching between the payload and target system.

---

## 🎯 Objectives

The main objectives of this lab were:

1. Understand Reverse TCP communication.
2. Understand Bind TCP communication.
3. Configure Metasploit Multi/Handler.
4. Understand Meterpreter payload architecture.
5. Identify the target system architecture.
6. Generate an architecture-compatible payload for lab analysis.
7. Perform basic post-exploitation enumeration.
8. Document the complete workflow with screenshots.
9. Understand the importance of isolated security testing environments.

---

## 🧪 Lab Environment

| Component | Details |
|---|---|
| Attacker Machine | Kali Linux |
| Target Machine | Metasploitable 2 |
| Network Type | Host-Only |
| Network | `192.168.56.0/24` |
| Kali IP | `192.168.56.102` |
| Metasploitable IP | `192.168.56.101` |
| Target Architecture | `i686 / 32-bit` |
| Framework | Metasploit |
| Payload Type Studied | Reverse TCP / Bind TCP |
| Default Handler Port | `4444` |

---

## 🗂️ Repository Structure

```text
metasploit-bind-reverse-shell-lab/
│
├── README.md
│
├── screenshots/
│   ├── 01_lab_setup.png
│   ├── 02_kali_ip.png
│   ├── 03_metasploitable_ip.png
│   ├── 04_ping_test.png
│   ├── 05_nmap_scan.png
│   ├── 06_msfconsole.png
│   ├── 07_multi_handler.png
│   ├── ...
│   │
│   └── post-exploitation/
│       ├── post_user.png
│       ├── post_hostname.png
│       ├── post_system_info.png
│       ├── post_network_info.png
│       └── post_processes.png
│
└── documentation/
    ├── reverse-tcp.md
    ├── bind-tcp.md
    ├── meterpreter.md
    └── post-exploitation.md
```
---

# 🏗️ 1. Lab Setup

The lab consists of two virtual machines connected through a Host-Only network.

### Attacker

```text
Kali Linux
IP: 192.168.56.102
```

### Target

```text
Metasploitable 2
IP: 192.168.56.101
```

The Host-Only network allows both machines to communicate without exposing the vulnerable target directly to the external network.

### Connectivity Verification

The target successfully communicated with the Kali machine.

```text
4 packets transmitted
4 packets received
0% packet loss
```

📸 **Evidence:**

`04_ping_test.png`

---

# 🔎 2. Target Enumeration

Nmap was used to identify exposed services on the Metasploitable 2 target.

Example scan:

```bash
nmap -sV 192.168.56.101
```

The scan identified multiple services including:

| Port    | Service    | Version / Information     |
| ------- | ---------- | ------------------------- |
| 21      | FTP        | vsftpd 2.3.4              |
| 22      | SSH        | OpenSSH 4.7p1             |
| 23      | Telnet     | Telnet                    |
| 25      | SMTP       | Postfix                   |
| 53      | DNS        | BIND 9.4.2                |
| 80      | HTTP       | Apache 2.2.8              |
| 139/445 | SMB        | Samba                     |
| 1524    | Bind Shell | Metasploitable root shell |
| 3306    | MySQL      | MySQL                     |
| 5432    | PostgreSQL | PostgreSQL                |
| 5900    | VNC        | VNC                       |
| 6667    | IRC        | UnrealIRCd                |
| 8180    | HTTP       | Apache Tomcat             |

📸 **Evidence:**

`05_nmap_scan.png`

The enumeration phase helped identify services available on the intentionally vulnerable target.

---

# 🔄 3. Reverse TCP

## What is a Reverse TCP Shell?

In a reverse TCP connection, the target system initiates a TCP connection back to a listening system.

Conceptually:

```text
Target
192.168.56.101
      |
      | Outbound TCP connection
      v
Kali / Listener
192.168.56.102:4444
```

This differs from a Bind TCP shell where the target listens for incoming connections.

---

## 🎯 Metasploit Multi/Handler

Metasploit's `multi/handler` can act as a listener for compatible payloads.

The lab configuration used:

```text
LHOST = 192.168.56.102
LPORT = 4444
```

The target architecture was verified before selecting the final payload architecture.

📸 **Evidence:**

`07_multi_handler.png`

---

## 🧩 Target Architecture

The target system was identified as:

```text
i686 GNU/Linux
```

This indicates a 32-bit x86 Linux environment.

Architecture compatibility is important because payloads are built for specific operating-system and CPU architectures.

📸 **Evidence:**

`metasploitable_uname.png`

---

## 📦 Payload Architecture Verification

An x86 Linux Meterpreter payload was generated for architecture analysis.

The generated file was checked using the `file` command.

Result:

```text
ELF 32-bit LSB executable
Intel i386
```

This confirmed that the generated ELF matched the target's 32-bit x86 architecture.

📸 **Evidence:**

`payload_file_verification.png`

> The generated executable payload is intentionally **not included in this repository**.

---

## 🎧 Handler Verification

The configured handler was running on:

```text
192.168.56.102:4444
```

The listening socket was verified on Kali.

The Metasploit job showed:

```text
Exploit: multi/handler
Payload: linux/x86/meterpreter/reverse_tcp
```

📸 **Evidence:**

`x86_handler_listening.png`

`x86_handler_job.png`

---

# 🔗 4. Bind TCP

## What is a Bind TCP Shell?

In a Bind TCP configuration, the target system listens on a TCP port.

The attacker connects to that listening port.

Conceptually:

```text
Kali
192.168.56.102
      |
      | TCP connection
      v
Target
192.168.56.101:1524
      |
      v
Bind Shell
```

This is different from Reverse TCP because the connection direction is initiated by the attacker.

---

## 🧪 Bind Shell Demonstration

Metasploitable 2 exposes a deliberately vulnerable bind shell service on TCP port `1524`.

The connection was tested from Kali using a TCP client.

The shell successfully returned:

```text
whoami
root
```

and:

```text
hostname
metasploitable
```

This demonstrated successful access to the intentionally vulnerable lab service.

📸 **Evidence:**

`bind_shell_connection.png`

---

# 🖥️ 5. Meterpreter

## What is Meterpreter?

Meterpreter is a Metasploit payload/session framework designed to provide an interactive post-exploitation environment.

It supports various capabilities for authorized security testing and assessment.

In this project, Meterpreter was studied in the context of:

* Reverse TCP communication
* Multi/Handler
* Payload architecture
* Session concepts
* Post-exploitation enumeration

### Important Note

A successful Meterpreter session was **not established in this documented lab workflow**.

The project therefore does not claim a Meterpreter session where one was not actually obtained.

The payload and handler configuration were documented for educational and architectural understanding.

---

# 🔍 6. Post-Exploitation Enumeration

After obtaining authorized shell access through the vulnerable lab service, basic system enumeration was performed.

The objective was to understand the compromised environment without performing destructive actions.

---

## 👤 Identify Current User

Command:

```bash
whoami
```

Result:

```text
root
```

This confirmed that the shell had root-level privileges in the intentionally vulnerable Metasploitable environment.

📸 **Evidence:**

`post-exploitation/post_user.png`

---

## 🖥️ Identify Host

Command:

```bash
hostname
```

Result:

```text
metasploitable
```

This confirmed the hostname of the target system.

📸 **Evidence:**

`post-exploitation/post_hostname.png`

---

## ⚙️ System Information

Command:

```bash
uname -a
```

The output confirmed the Linux environment and the target architecture:

```text
i686 GNU/Linux
```

This information was also useful when selecting an architecture-compatible payload.

📸 **Evidence:**

`post-exploitation/post_system_info.png`

---

## 🌐 Network Information

Command:

```bash
ifconfig
```

This was used to inspect the target's network interfaces and IP configuration.

📸 **Evidence:**

`post-exploitation/post_network_info.png`

---

## ⚙️ Running Processes

Command:

```bash
ps aux
```

This provided an overview of processes currently running on the target.

Process enumeration is commonly performed during authorized security assessments to understand the system environment.

📸 **Evidence:**

`post-exploitation/post_processes.png`

---

# 📊 7. Reverse TCP vs Bind TCP

| Feature                 | Reverse TCP                        | Bind TCP                          |
| ----------------------- | ---------------------------------- | --------------------------------- |
| Connection initiated by | Target                             | Attacker                          |
| Listener location       | Attacker                           | Target                            |
| Example lab IP          | `192.168.56.102:4444`              | `192.168.56.101:1524`             |
| Firewall considerations | Target needs outbound connectivity | Target needs inbound connectivity |
| Metasploit Handler      | Commonly used                      | Depends on payload/setup          |
| Main concept            | Target calls back                  | Attacker connects to target       |

---

# 🧠 8. Key Learnings

Through this lab, I learned:

* How Host-Only virtual networks can be used for isolated security testing.
* How to identify vulnerable services using Nmap.
* The difference between Reverse TCP and Bind TCP communication.
* How Metasploit `multi/handler` works conceptually.
* Why payload architecture must match the target architecture.
* How to verify Linux ELF architecture.
* How to perform basic post-exploitation enumeration.
* How to document security testing activities using screenshots and evidence.
* The importance of performing offensive-security testing only in authorized environments.

---

# 🛡️ 9. Security Considerations

The techniques demonstrated in this project can be dangerous when used against systems without authorization.

This project was conducted using:

```text
Kali Linux
        +
Metasploitable 2
        +
Host-Only Network
```

Metasploitable 2 is intentionally vulnerable and was used strictly as a training target.

No unauthorized systems were targeted.

The executable payload generated during the lab is intentionally excluded from this repository.

---

# 📸 10. Evidence

The repository contains screenshots documenting the major stages of the lab:

* Lab network configuration
* IP configuration
* Connectivity testing
* Nmap enumeration
* Metasploit configuration
* Handler configuration
* Listener verification
* Target architecture verification
* Payload architecture verification
* Bind shell access
* Post-exploitation enumeration

Screenshots are provided as supporting evidence for the documented workflow.

---

# 🚀 11. Future Improvements

Possible future extensions of this project include:

* Adding a dedicated network diagram.
* Adding Wireshark traffic analysis.
* Documenting detection opportunities for SOC analysts.
* Mapping observed activity to MITRE ATT&CK techniques.
* Creating IDS detection rules for reverse/bind shell traffic.
* Adding defensive recommendations.
* Building a small SOC-style monitoring workflow around the lab.

---

# 📚 12. References

* [Metasploit Framework](https://github.com/rapid7/metasploit-framework)
* [Metasploit Documentation](https://docs.metasploit.com/)
* [Metasploitable 2](https://sourceforge.net/projects/metasploitable/)
* [Nmap Documentation](https://nmap.org/docs.html)

---

## 👨‍💻 Project Type

```text
Cybersecurity Lab
Offensive Security
Network Security
Penetration Testing
Metasploit
Linux Security
Post-Exploitation
```

---

## ⚠️ Disclaimer

This repository is intended for **educational and authorized cybersecurity testing purposes only**.

Do not use these techniques against systems, networks, applications, or devices without explicit authorization.
