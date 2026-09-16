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

Observed Output:

Registrar: GoDaddy.com, LLC

Creation Date: 2019-11-06

Registry Expiry Date: 2027-11-06

Name Servers: NS6135.HOSTGATOR.COM, NS6136.HOSTGATOR.COM

DNSSEC: Unsigned



2. Web Technology Fingerprinting (whatweb)
Mapped out the tech stack running on the target web server.

bash
whatweb networkwalks.com

Observed Output:

CMS / Framework: WordPress 7.1

Plugins Detected: WP Download Manager 3.3.58

Web Server: Apache

JavaScript Libraries: jQuery 3.7.1, Bootstrap 7.1

Target IP: 192.232.216.135



3. DNS IP Resolution (nslookup)
Resolved the domain name directly to its IPv4 address.

Observed Output:

DNS Resolver Used: Google Public DNS (8.8.8.8#53)

Resolved Address: 192.232.216.135


4. HTTP Header Inspection (curl -I)
Fetched the HTTP response headers without pulling down the whole web page body.

Observed Output:

HTTP Status: HTTP/2 200

Server Header: Apache

Exposed Endpoint: link: <https://networkwalks.com/wp-json/>; rel="https://api.w.org/"

Caching/Proxy: x-nginx-cache: WordPress, x-endurance-cache-level: 0



5. Web Application Firewall Detection (wafw00f)
Checked for an active Web Application Firewall (WAF) filtering web traffic.

Observed Output:

WAF Detected: ModSecurity (SpiderLabs)

Requests Made: 2


6. DNS Record Enumeration (dnsrecon)
Scraped publicly exposed DNS records associated with the target domain.

Observed Output:

Nameserver IPs: 50.87.144.87 (NS6135) & 192.232.216.131 (NS6136)

Bind Software Versions: 9.16.23-RH

Mail Server (MX): mail.networkwalks.com (192.232.216.135)

TXT / SPF Record: v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all

SRV Records: Exposed _autodiscover._tcp.networkwalks.com pointing to cpanelemaildiscovery.cpanel.net



4. Risk Analysis & Impact Assessment

1. Outdated Software Versions Exposed

Evidence: whatweb identified WordPress 7.1 and WP Download Manager 3.3.58

Impact: Public CVE exploitation targeting known software vulnerabilities

Risk: 🟡 Medium

2. Direct Target IP Address Disclosed

Evidence: nslookup resolved server IP to 192.232.216.135

Impact: Target identification for active port scanning and probing

Risk: 🟢 Low

3. Unauthenticated REST API Exposed

Evidence: curl -I exposed the /wp-json/ endpoint

Impact: User and metadata enumeration without authentication

Risk: 🟢 Low

4. WAF Security Controls Revealed

Evidence: wafw00f identified ModSecurity (SpiderLabs)

Impact: Enables attackers to craft tailored WAF evasion techniques

Risk: 🟢 Low

5. DNS & Software Infrastructure Disclosure

Evidence: dnsrecon exposed BIND software version (9.16.23-RH) and cPanel MX records

Impact: Widens attack surface to include DNS and mail service exploits

Risk: 🟡 Medium

6. Live Network Hosts Visible

Evidence: Zenmap network scan mapped active hosts on local subnet

Impact: Potential entry point or lateral movement vector if unsecured

Risk: 🔴 High
   
   
    ### 6. Security Recommendations & Action Plan

Based on the findings from the passive footprinting and active scanning activities against `networkwalks.com`—including exposed software versions, direct IP resolution, unauthenticated endpoints, and network host visibility—the following security improvements are recommended to harden the target system's defensive posture:

#### 1. Software & Vulnerability Management
* **Review Publicly Exposed Technology Information:** Implement banner grabbing prevention and strip version disclosure tags (e.g., WordPress 7.1 and WP Download Manager 3.3.58 banners detected via `whatweb`) to prevent attackers from targeted fingerprinting.
* **Keep Software Updated:** Establish a regular patch management policy to update CMS platforms, plugins, BIND DNS services (`9.16.23-RH`), and web server technologies immediately upon the release of security advisories.

#### 2. Web Application & HTTP Configuration
* **Review & Harden HTTP Headers:** Audit response headers to remove verbose operational details (`Apache` server tokens, `x-endurance-cache-level`) and secure or restrict access to sensitive, unauthenticated REST API endpoints such as `/wp-json/`.
* **Properly Configure & Monitor the WAF:** Maintain and continuously tune the active Web Application Firewall (ModSecurity). Update rulesets regularly to ensure it detects advanced evasion payloads beyond default automated attacks.

#### 3. Network & Infrastructure Defense
* **Review DNS Records Regularly:** Audit public DNS configurations (MX, SPF, SRV records) to ensure only essential routing information is exposed and that DNSSEC is enabled to prevent spoofing.
* **Perform Regular Internal Network Discovery:** Execute scheduled, authorized network scans to identify active IP targets, open ports, and potential attack vectors before unauthorized actors exploit them.
* **Investigate Unknown Devices:** Establish an immediate incident response protocol for any unverified or unexpected host discovered on local subnets during network scans.
* **Maintain Up-to-Date Network Documentation:** Keep network topology maps, IP allocations, and co-hosted server inventories current to quickly pinpoint rogue devices or misconfigured assets.

#### 4. Compliance & Operational Guidelines
* **Perform Security Testing with Authorization:** Ensure all future reconnaissance, vulnerability probing, and network discovery activities strictly adhere to authorized rules of engagement and legal consent frameworks.















