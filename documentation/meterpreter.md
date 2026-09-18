# 🖥️ Meterpreter

## 📌 Overview

Meterpreter is an advanced payload and session framework within the Metasploit ecosystem.

It provides an interactive environment that can be used during authorized penetration testing and security assessments.

In this project, Meterpreter was studied as part of the Reverse TCP workflow.

---

## 🎯 Role in This Project

The Meterpreter section of this lab focuses on:

* Understanding Meterpreter payloads.
* Understanding Reverse TCP communication.
* Configuring Metasploit Multi/Handler.
* Matching payload architecture with the target.
* Verifying the generated payload architecture.
* Understanding the relationship between a payload and a handler.

---

# 🧪 Lab Environment

| Component            | Details          |
| -------------------- | ---------------- |
| Attacker             | Kali Linux       |
| Attacker IP          | `192.168.56.102` |
| Target               | Metasploitable 2 |
| Target IP            | `192.168.56.101` |
| Network              | Host-Only        |
| Target Architecture  | `i686 / 32-bit`  |
| Handler Port         | `4444`           |
| Payload Architecture | x86              |

---

# 🧩 Target Architecture

Before selecting an architecture-specific payload, the target system architecture was verified.

The target reported:

```text
i686 GNU/Linux
```

This indicates a 32-bit x86 Linux environment.

Architecture verification is important because a payload compiled for a different architecture may not be compatible with the target environment.

📸 **Evidence:**

`metasploitable_uname.png`

---

# 📦 Meterpreter Reverse TCP Payload

For the lab configuration, an x86 Linux Meterpreter Reverse TCP payload was selected.

The payload type was:

```text
linux/x86/meterpreter/reverse_tcp
```

The associated listener configuration used:

```text
LHOST = 192.168.56.102
LPORT = 4444
```

Kali Linux was used as the listener because it was the receiving system in the Host-Only lab network.

---

# 🎧 Multi/Handler

Metasploit's Multi/Handler was configured to handle the selected payload type.

The configured payload was:

```text
linux/x86/meterpreter/reverse_tcp
```

The handler was configured to listen on:

```text
192.168.56.102:4444
```

📸 **Evidence:**

`x86_handler_config.png`

---

# 🔍 Listener Verification

The listener was verified on Kali Linux.

The TCP socket was listening on:

```text
192.168.56.102:4444
```

This confirmed that the handler was actively waiting for a compatible connection.

📸 **Evidence:**

`x86_handler_listening.png`

---

# ⚙️ Metasploit Job Verification

The configured handler was visible as a running Metasploit job.

The job showed:

```text
Exploit: multi/handler
Payload: linux/x86/meterpreter/reverse_tcp
```

📸 **Evidence:**

`x86_handler_job.png`

---

# 📄 Payload File Verification

An x86 Linux Meterpreter payload was generated for architecture verification.

The resulting file was inspected to confirm its executable architecture.

The file was identified as:

```text
ELF 32-bit LSB executable
Intel i386
```

This matched the architecture reported by the Metasploitable 2 target.

📸 **Evidence:**

`payload_file_verification.png`

---

# 🔄 Meterpreter Communication Model

The intended Reverse TCP communication model can be represented as:

```text
┌──────────────────────┐
│   Metasploitable 2   │
│   192.168.56.101     │
│                      │
│       Target         │
└──────────┬───────────┘
           │
           │ Reverse TCP
           │
           ▼
┌──────────────────────┐
│      Kali Linux      │
│   192.168.56.102     │
│                      │
│   Multi/Handler      │
│       :4444          │
└──────────────────────┘
```

The key concept is that the target initiates the connection toward the configured listener.

---

# ⚠️ Session Status

A successful Meterpreter session was **not established** during this documented workflow.

Therefore, this project does not claim successful Meterpreter commands or post-exploitation actions through a Meterpreter session.

The following parts were successfully demonstrated:

* Target architecture identification.
* x86 payload selection.
* Multi/Handler configuration.
* Listener verification.
* Metasploit job verification.
* Payload file architecture verification.

---

# 🛡️ Security Perspective

Meterpreter and similar remote-access payloads can provide extensive capabilities during authorized security assessments.

From a defensive perspective, security teams can monitor:

* Unexpected outbound connections.
* Suspicious processes making network connections.
* Unknown executable files.
* Unusual connections to uncommon ports.
* Persistence mechanisms.
* Abnormal parent-child process relationships.
* Endpoint and network telemetry associated with remote-access activity.

Security controls such as EDR, network monitoring, application control, and host firewalls can help detect or restrict suspicious activity.

---

# 🧠 Key Learning

The Meterpreter section demonstrated:

1. The purpose of Meterpreter within the Metasploit ecosystem.
2. The relationship between a payload and a handler.
3. The importance of target architecture compatibility.
4. How to verify an ELF executable's architecture.
5. How a Multi/Handler listener is configured.
6. How to verify a running Metasploit handler.
7. The importance of accurately documenting whether a session was actually established.

---

# 📸 Evidence Summary

| Evidence              | Screenshot                      |
| --------------------- | ------------------------------- |
| Target architecture   | `metasploitable_uname.png`      |
| Handler configuration | `x86_handler_config.png`        |
| Listener verification | `x86_handler_listening.png`     |
| Metasploit job        | `x86_handler_job.png`           |
| Payload architecture  | `payload_file_verification.png` |

---

# 📌 Conclusion

The Meterpreter portion of this project focused on understanding the architecture and workflow of a Meterpreter Reverse TCP setup.

The target was confirmed as a 32-bit x86 Linux system, an architecture-compatible payload was analyzed, and the Multi/Handler listener was successfully configured and verified.

Although a Meterpreter session was not established, the lab provided practical understanding of payload architecture, handler configuration, and Reverse TCP communication.
