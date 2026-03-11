# HTB Write-Up: Fawn — The Danger of Anonymous FTP

## Introduction

In cybersecurity, many breaches do not begin with complex zero-day exploits or sophisticated malware. Often, they start with something far simpler: **misconfigurations**. One of the most classic examples is **anonymous FTP access**.

In this write-up, I will document my walkthrough of **Fawn**, one of the beginner machines from **Hack The Box (HTB)**. Despite its simplicity, Fawn teaches a crucial lesson: **exposed services with weak authentication can become an immediate entry point for attackers**.

This exercise focuses on reconnaissance, service enumeration, and exploiting a common misconfiguration in FTP.

---

# 1. Connecting to the Hack The Box Network

Before interacting with any HTB machine, the first step is establishing a secure connection to the HTB lab environment.

This is done using **OpenVPN**.

```bash
sudo openvpn <your_vpn_file>.ovpn
```

Once connected successfully, your machine becomes part of the HTB network, allowing communication with target machines.

A good habit after connecting is verifying that the connection works correctly.

---

# 2. Verifying Connectivity with Ping

Before performing deeper scans, I verified connectivity with the target machine using **ping**.

```bash
ping <target-ip>
```

If the target responds, it confirms two important things:

* The VPN connection is functioning correctly
* The target machine is reachable from our environment

Skipping this step often leads beginners to waste time troubleshooting tools instead of identifying network connectivity issues.

---

# 3. Reconnaissance: Scanning Open Ports with Nmap

Once connectivity is confirmed, the next phase is **enumeration**.

Enumeration is where attackers gather as much information as possible about exposed services.

I used **Nmap** with service version detection.

```bash
nmap -sV <target-ip>
```

Explanation:

* **-sV** : Detects service versions running on open ports.

The scan revealed an interesting result:

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd
```

The target machine exposes **FTP on port 21**.

At this point, the attack surface becomes clear: **FTP service enumeration**.

---

# 4. Attempting FTP Access

FTP servers sometimes allow **anonymous authentication**, which means anyone can log in without a password.

I attempted to connect using the FTP client.

```bash
ftp <target-ip>
```

Login credentials used:

```
Username: anonymous
Password: (leave blank)
```

The login was successful.

This immediately reveals a **critical security misconfiguration**.

Anonymous FTP access allows anyone on the network to explore files stored on the server.

---

# 5. Enumerating Files on the FTP Server

Once inside the FTP server, I listed the available files.

```bash
ls
```

This revealed files stored on the server.

At this point, enumeration becomes critical. Attackers typically look for:

* Backup files
* Configuration files
* Credentials
* Flags (in CTF environments)

---

# 6. Downloading Files from the Target

To retrieve the file from the server, I used the **get** command.

```bash
get <filename>
```

This downloads the file from the FTP server to the local machine.

Once downloaded, the file can be inspected locally.

---

# 7. Retrieving the Flag

After opening the downloaded file, the **flag** was revealed.

This confirms successful exploitation of the misconfigured FTP service.

---

# Key Cybersecurity Lessons from Fawn

Even though this machine is simple, it highlights several real-world lessons.

### 1. Misconfiguration is one of the biggest security risks

Many organizations focus heavily on advanced defenses, but basic service misconfigurations remain common.

Allowing **anonymous FTP access** exposes internal files to anyone who can reach the service.

---

### 2. Enumeration is the foundation of penetration testing

The attack path was discovered purely through systematic enumeration:

1. Confirm connectivity
2. Scan open ports
3. Identify services
4. Test authentication methods

No exploit was needed — only observation and logical thinking.

---

### 3. Exposed services expand the attack surface

Every open port increases potential risk.

If a service is not required, it should be:

* Disabled
* Firewalled
* Properly authenticated

---

### 4. Simple services can lead to major breaches

FTP is one of the **oldest protocols on the internet**, yet it still appears frequently in security incidents.

In real-world environments, attackers often discover:

* Credential backups
* Database dumps
* Internal documents
* Private SSH keys

inside poorly configured FTP servers.

---

# Defensive Recommendations for Cybersecurity Teams

Organizations should consider the following defensive practices:

**1. Disable Anonymous FTP**

If FTP is required, enforce authenticated access.

---

**2. Replace FTP with Secure Alternatives**

Prefer secure protocols such as:

* SFTP
* FTPS

These provide encryption and stronger authentication.

---

**3. Continuous Attack Surface Monitoring**

Regularly scan internal and external infrastructure for exposed services using tools such as:

* Nmap
* Nessus
* OpenVAS

---

**4. Principle of Least Privilege**

Users and services should only have the minimum level of access required.

---

# Final Thoughts

Fawn demonstrates a fundamental truth in cybersecurity:

**Attackers don't always break systems — sometimes they simply log in.**

A single misconfigured service can expose sensitive data to the world.

For cybersecurity professionals, mastering **reconnaissance and enumeration** is essential. These skills form the foundation of every penetration test and real-world intrusion.

Sometimes the most powerful attack is simply asking the system a question — and carefully observing the answer.

---

*Written by Cheka*
Cybersecurity Enthusiast | Security Researcher
