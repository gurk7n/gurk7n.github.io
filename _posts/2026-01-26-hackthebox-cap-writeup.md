---
title: "HackTheBox: Cap Writeup"
date: 2026-01-26 22:46:00 +0300
image: /assets/img/posts/hackthebox-cap-writeup/cover.jpg
categories: [Writeups]
tags: [HackTheBox, IDOR, Linux Capabilities, PCAP Analysis, Privilege Escalation, Nmap, Wireshark, Python, GTFOBins]
---

In this write-up, I will demonstrate the step-by-step exploitation of the **Cap** machine on the **HackTheBox** platform. Cap is an easy-rated Linux box that provides an excellent environment for practicing network traffic analysis, identifying **IDOR** vulnerabilities, and exploiting misconfigured **Linux capabilities** for privilege escalation.

## Nmap Scan Results

I started by using the **Nmap** tool to perform a deep scan of the target machine's active services and open ports.

```bash
sudo nmap -T5 -sSCV 10.129.9.159
```

* `-T5`: High-speed timing template to accelerate the scan.
* `-sS`: TCP SYN scan for efficiency and speed.
* `-sC`: Runs default Nmap scripts to look for common vulnerabilities.
* `-sV`: Service version detection to identify exact software versions.

![Comprehensive Nmap scan results identifying open ports and services.](/assets/img/posts/hackthebox-cap-writeup/nmap-scan.png){: width="972" }

The scan results revealed three critical entry points, each representing a potential attack vector for our initial foothold:

* **Port 21 (FTP):** vsftpd 3.0.3 — Fast and secure FTP server software.
* **Port 22 (SSH):** OpenSSH 8.2p1 — Secure tool for encrypted remote access.
* **Port 80 (HTTP):** Gunicorn — Python WSGI HTTP server for applications.

![The security dashboard interface highlighting the Nathan user profile.](/assets/img/posts/hackthebox-cap-writeup/dashboard-ui.png){: width="972" }

After identifying the open ports, I navigated to the target IP address in my browser. The page redirects to a security dashboard belonging to a user named **Nathan**.

## Exploiting IDOR

![Manipulating the URL parameter to verify the IDOR vulnerability.](/assets/img/posts/hackthebox-cap-writeup/idor-exploit.png){: width="972" .w-50 .left}
While exploring the dashboard, I discovered a feature called "Security Snapshot" that performs a 5-second network analysis and provides a downloadable PCAP file of the results. 

I noticed that each scan was assigned a numerical ID in the URL, such as `http://10.129.9.159/data/4`. By manually changing the **ID** to `0`, I successfully bypassed access controls and accessed the system's initial network capture.

## PCAP Analysis

After downloading the **0.pcap** file, I opened it using **Wireshark** to analyze the captured network traffic.

![Extracting cleartext FTP credentials from the intercepted PCAP file.](/assets/img/posts/hackthebox-cap-writeup/wireshark-credentials.png){: width="972" }

While analyzing network traffic within a PCAP file, I found **Nathan**'s password in an FTP packet.

* **Username:** nathan
* **Password:** Buck3tH4TF0RM3!

## Initial Access via SSH

```bash
ssh nathan@10.129.9.159
```

With the credentials recovered from the PCAP analysis, I proceeded to gain remote access to the machine via **SSH**.

![Establishing an SSH session and capturing the user.txt flag.](/assets/img/posts/hackthebox-cap-writeup/ssh-flag-capture.png){: width="972" }

## Capturing User Flag

After establishing a stable session on the Ubuntu server, I listed the home directory contents and located the user.txt file. By reading its content, I successfully captured the user flag for the Cap machine.

> **User Flag**: 24edddd49b08634764d47856a6e19405
{: .prompt-tip }

## Privilege Escalation

After gaining a foothold as `nathan`, I began searching for misconfigured binaries that could allow for privilege escalation. Traditional checks like `sudo -l` didn't yield results, so I turned my focus to **Linux Capabilities**.

I ran the following command to find any binaries with interesting capabilities:
```bash
getcap -r / 2>/dev/null
```

The output revealed a high-interest vulnerability:

![Enumerating Linux capabilities to identify setuid misconfigurations.](/assets/img/posts/hackthebox-cap-writeup/capabilities-enum.png){: width="972" }

After discovering the `cap_setuid` privilege on the `/usr/bin/python3.8` binary, I consulted [**GTFOBins**](https://gtfobins.org/gtfobins/python/) to find a reliable exploitation method. This resource confirms that if **Python** has this specific capability, it can be used to escalate privileges by manipulating the process's UID.

![Using GTFOBins documentation to find the Python capability exploit.](/assets/img/posts/hackthebox-cap-writeup/gtfobins-python.png){: width="972" }

I executed the following command to elevate my privileges to root:

```bash
python3 -c 'import os; os.setuid(0); os.execl("/bin/sh", "sh")'
```

## Capturing Root Flag

Immediately after running the script, I obtained a root shell and navigated to the `/root` directory to finish the machine:

![Escalating to root and capturing the final flag.](/assets/img/posts/hackthebox-cap-writeup/root-flag-capture.png){: width="972" }

By reading the `root.txt` file, I successfully completed the **Cap** machine on **HackTheBox**.

> **Root Flag**: 9b57967bf5f69a3e55cdc94fd6efccf2
{: .prompt-tip }
