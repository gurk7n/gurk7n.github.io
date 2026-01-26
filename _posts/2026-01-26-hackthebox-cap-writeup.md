---
title: "HackTheBox: Cap Writeup"
date: 2026-01-26 22:46:00 +0300
image: /assets/img/posts/hackthebox-cap-writeup/cover.jpg
categories: [Writeups]
tags: [HackTheBox, IDOR, Linux Capabilities, PCAP Analysis, Privilege Escalation, Nmap, Wireshark, Python, GTFOBins]
published: false
---

In this write-up, I will demonstrate the step-by-step exploitation of the Cap machine on the HackTheBox platform. Cap is an easy-rated Linux box that provides an excellent environment for practicing network traffic analysis, identifying Insecure Direct Object Reference (IDOR) vulnerabilities, and exploiting misconfigured Linux capabilities for privilege escalation.

## Enumeration

I started by using the `nmap` tool to perform a deep scan of the target machine's active services and open ports.

```bash
sudo nmap -T5 -sSCV 10.129.9.159
```

![HackTheBox Cap Nmap Scan](/assets/img/posts/hackthebox-cap-writeup/img1.png){: width="972" }

The scan results revealed three critical entry points:

* **Port 21 (FTP):** vsftpd 3.0.3
* **Port 22 (SSH):** OpenSSH 8.2p1
* **Port 80 (HTTP):** Gunicorn