# Network Walkthrough & Reconnaissance Report

**Metadata**
* **Pentester Name:** Hira Ali
* **Program / Batch:** B083-Networkwalks
* **Date:** 15 September 2026
* **Modules Completed:** W2-PM1 (Multiple Kali Tools), W2-PM5 (Zenmap Scanning)
* **Client / Target:** Networkwalks & Local Subnet LAN
* **Permission Secured:** Yes
* **Phases Covered:** Phase 1 (Reconnaissance & Footprinting), Phase 2 (Scanning & Network Discovery)

---

## 1. Executive Summary

This week, I focused on moving from passive information gathering to active network scanning. I broke the work into two main parts: first, footprinting the `networkwalks.com` domain using various tools in Kali Linux (W2-PM1), and second, running a scan on my own local network using Zenmap on Windows (W2-PM5). Combining these two modules really helped me see the bigger picture of how an attacker transitions from pulling public DNS/domain info to actively mapping out live hosts and open ports on a private network.

I ran all the footprinting commands inside Kali Linux and handled the network scans through the Zenmap GUI on Windows. In the sections below, I’ve documented the exact commands I ran, the actual output I saw, screenshots for proof, and quick takeaways on why each piece of information is valuable from a security and attacker perspective.

---

## 2. Tools Used

| Tool | Purpose / Description |
| :--- | :--- |
| **Kali Linux & Windows** | Primary operating systems—Kali for Linux CLI recon tools and Windows for host-level tools. |
| **WHOIS** | Queries domain registration information (ownership, creation dates, nameservers). |
| **whatweb** | Fingerprints web server technologies, CMS framework, active plugins, and IP address. |
| **nslookup** | Queries DNS servers to resolve target domain names to IP addresses. |
| **curl -I** | Inspects HTTP response headers to view server details without fetching page content. |
| **wafw00f** | Detects whether a Web Application Firewall (WAF) is active on the target site. |
| **dnsrecon** | Performs deep DNS enumeration to collect NS, MX, SPF, TXT, and SRV records. |
| **Zenmap (Nmap GUI)** | Scans local subnets to discover live hosts, IP addresses, and MAC addresses visually. |
| **Windows CMD** | Executes local utility commands (`ipconfig`, `arp`) to verify host network configurations. |

---

## 3. Footprinting & Reconnaissance

For the initial stage of this lab, I ran passive recon against `networkwalks.com` using six different tools in Kali Linux: **WHOIS**, **WhatWeb**, **Nslookup**, **Curl**, **Wafw00f**, and **DNSRecon**.

My goal here was to act like a real-world attacker gathering public intelligence without directly interacting with or alerting the internal target network.

### 1. Domain WHOIS Query (`whois`)
Queried the global WHOIS database to gather public domain registration details, registrar contact info, and core hosting infrastructure.

```bash
whois networkwalks.com












