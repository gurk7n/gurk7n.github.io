---
title: "HackTheBox: Cap Writeup"
date: 2026-01-26 22:46:00 +0300
image: /assets/img/posts/hackthebox-cap-writeup/cover.jpg
categories: [Writeups]
tags: [HackTheBox, IDOR, Linux Capabilities, PCAP Analysis, Privilege Escalation, Nmap, Wireshark, Python, GTFOBins]
---

In this article, I will explain how I solved the Cap machine problem on the HackTheBox platform.

## Enumeration

First, I used the nmap tool to scan for open ports on the target machine.

```bash
sudo nmap -T5 -sSCV 10.129.9.159
```

After scanning, I found that ports 21, 22, and 80 were open. FTP, SSH, and HTTP services are running on these ports.

![Nmap scan results of the target machine](/assets/img/posts/hackthebox-cap-writeup/img2.png){: width="972" }
