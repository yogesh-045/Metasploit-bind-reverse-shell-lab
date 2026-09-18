# 🔄 Reverse TCP

## 📌 Overview

Reverse TCP is a communication model in which the target system initiates a TCP connection back to a system controlled by the security tester.

In this lab, Kali Linux was configured as the listener, while Metasploitable 2 was used as the intentionally vulnerable target.

---

## 🧪 Lab Configuration

| Component           | Configuration    |
| ------------------- | ---------------- |
| Attacker            | Kali Linux       |
| Attacker IP         | `192.168.56.102` |
| Target              | Metasploitable 2 |
| Target IP           | `192.168.56.101` |
| Network             | Host-Only        |
| Listener Port       | `4444`           |
| Target Architecture | `i686 / 32-bit`  |

---

## 🔄 Communication Flow

```text
┌──────────────────────┐
│   Metasploitable 2   │
│   192.168.56.101     │
│                      │
│       Target         │
└──────────┬───────────┘
           │
           │ Reverse TCP
           │ Connection
           ▼
┌──────────────────────┐
│      Kali Linux      │
│   192.168.56.102     │
│                      │
│      Listener        │
│       :4444          │
└──────────────────────┘
```

The important difference is that the connection is initiated from the target side toward the listener.

---

# ⚙️ Metasploit Multi/Handler

Metasploit's `multi/handler` can be configured to listen for a compatible payload connection.

The handler configuration used in this lab was:

```text
LHOST = 192.168.56.102
LPORT = 4444
```

The Kali Host-Only IP was used as the listener address because Kali was acting as the receiving system in this isolated lab.

---

## 🖥️ Handler Configuration

The handler was configured with an x86 Linux Meterpreter reverse TCP payload.

```text
Payload:
linux/x86/meterpreter/reverse_tcp

LHOST:
192.168.56.102

LPORT:
4444
```

📸 **Screenshot:**

`x86_handler_config.png`

---

# 🎧 Listener Verification

After starting the handler, the listening socket was verified on Kali.

The listener was bound to:

```text
192.168.56.102:4444
```

This confirmed that Kali was actively listening on the configured TCP port.

📸 **Screenshot:**

`x86_handler_listening.png`

---

# 🧩 Target Architecture

Before working with an architecture-specific payload, the target architecture was verified.

The Metasploitable system reported:

```text
i686 GNU/Linux
```

This represents a 32-bit x86 Linux environment.

Architecture verification is important because payloads are generally compiled for a specific operating system and CPU architecture.

📸 **Screenshot:**

`metasploitable_uname.png`

---

# 📦 Payload Architecture Verification

An x86 Linux Meterpreter reverse TCP payload was generated for the lab and its file type was inspected.

The resulting file was identified as:

```text
ELF 32-bit LSB executable
Intel i386
```

This confirmed that the generated payload matched the 32-bit x86 architecture of the target.

📸 **Screenshot:**

`payload_file_verification.png`

> ⚠️ The generated executable payload is intentionally not included in this GitHub repository.

---

# 📋 Metasploit Job

The configured handler appeared as a running Metasploit job.

```text
Exploit: multi/handler
Payload: linux/x86/meterpreter/reverse_tcp
```

The handler was configured to listen on:

```text
192.168.56.102:4444
```

📸 **Screenshot:**

`x86_handler_job.png`

---

# ⚠️ Session Status

A successful Meterpreter session was **not established** during the documented workflow.

Therefore, this project does not claim a successful reverse Meterpreter connection.

The lab demonstrates:

* Reverse TCP concepts
* Multi/Handler configuration
* Listener verification
* Target architecture identification
* Payload architecture verification

This distinction is important for maintaining accurate security documentation.

---

# 🧠 Key Learning

The Reverse TCP section helped demonstrate:

1. The difference between reverse and bind connections.
2. The role of the listener.
3. The purpose of `LHOST` and `LPORT`.
4. The importance of selecting the correct target architecture.
5. How to verify that a listener is active.
6. How to document unsuccessful session establishment accurately.

---

# 🔐 Security Perspective

Reverse connections are an important concept in penetration testing and malware analysis because the direction of network communication can affect firewall and network-security controls.

From a defensive perspective, security teams can monitor:

* Unexpected outbound connections
* Unusual destination IP addresses
* Suspicious high-numbered ports
* Unknown processes creating network connections
* Repeated outbound connection attempts

In a real environment, these events should be investigated according to the organization's security policies and incident-response procedures.

---

## 📸 Evidence Summary

| Evidence              | Screenshot                      |
| --------------------- | ------------------------------- |
| Handler configuration | `x86_handler_config.png`        |
| Listener verification | `x86_handler_listening.png`     |
| Metasploit job        | `x86_handler_job.png`           |
| Target architecture   | `metasploitable_uname.png`      |
| Payload architecture  | `payload_file_verification.png` |

---

## 📌 Conclusion

The Reverse TCP portion of this lab demonstrated the concepts behind reverse TCP communication and Metasploit's Multi/Handler.

The target architecture was verified as 32-bit x86, and an architecture-compatible payload was analyzed at the file level.

Although a Meterpreter session was not established, the configuration and verification steps provided practical understanding of how reverse TCP infrastructure is structured in an isolated penetration-testing lab.
