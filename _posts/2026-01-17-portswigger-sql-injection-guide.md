---
title: "SQL Injection: The PortSwigger Lab Guide"
date: 2026-01-17 12:56:00 +0300
image: /assets/img/posts/portswigger-sql-injection-guide/cover.jpg
categories: [Web Security, Write-ups]
tags: [SQL Injection, PortSwigger, Web Security, Blind SQLi, OAST, Write-up, OWASP Top 10]
---

## Core Concept

`SQL Injection` (`SQLi`) is a vulnerability that allows an attacker to interfere with database queries. By manipulating input, you can `view`, `modify`, or `delete` data you aren't supposed to access.

## 1. Detection Techniques

To find a potential entry point, test every input with these payloads:

* **Syntax Break:** `'` (Check for database errors or anomalies)
* **Boolean Logic:** `OR 1=1` vs `OR 1=2` (Check for systematic differences in the application’s responses)
* **Time Delays:** `pg_sleep(10)` or `sleep(10)` (Check for differences in the time taken to respond)
* **OAST Payloads:** Designed to trigger an out-of-band network interaction (e.g., `DNS` lookup)

## 2. Retrieving Hidden Data

When an application filters results (e.g., `WHERE released = 1`), you can use SQL comments (`--`) to bypass the filter and see hidden items.

* **Original Query:** `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`
* **Payload:** `--`
* **Resulting Query:** `SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1`

### **Lab 1: Hidden Data Retrieval**
* **Goal:** Display all products in a specific category, including unreleased ones.

![Spawning an interactive shell using Python PTY module](/assets/img/posts/python-reverse-shell-upgrade/img1.png){: width="972" height="589" }

* **Solution:** Append `--` to the `category` parameter to comment out the rest of the query and ignore the `released = 1` condition.