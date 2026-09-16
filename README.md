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

Assessment Type: Passive Reconnaissance & Technical Footprinting.

Primary Target: networkwalks.com (Secondary scope observed: microsoft.com).

Objective: Gather public DNS, hosting, server headers, technology stack details, and OSINT data without executing intrusive exploits.

Overall Assessment: The target infrastructure relies on standard web hosting with active WAF protection (ModSecurity). Multiple API errors during OSINT gathering indicate unconfigured or invalid external integrations in the testing environment.

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
   
   
Conclusion & Final Takeaways

Hands-on Reconnaissance & Scanning: Completed Week 2 internship tasks transitioning from passive information gathering on networkwalks.com (using Kali Linux) to active local subnet host discovery (using Zenmap on Windows).

Footprinting Results (networkwalks.com):

WHOIS: Identified domain ownership through GoDaddy and HostGator nameservers (NS6135, NS6136).

WhatWeb: Fingerprinted the tech stack, uncovering WordPress 7.1, WP Download Manager 3.3.58, Apache, and IP 192.232.216.135.

Nslookup: Confirmed public IPv4 address resolution via Google DNS (8.8.8.8).

Curl (-I): Exposed Apache server headers and the unauthenticated REST API endpoint (/wp-json/).

Wafw00f: Detected an active ModSecurity (SpiderLabs) Web Application Firewall.

DNSRecon: Enumerated mail servers (MX), cPanel autodiscover records, and disclosed BIND DNS software version 9.16.23-RH.

Network Discovery Results (Zenmap): Mapped the local network layout to discover live hosts, record IP and MAC address pairings, and plot network topology.

Core Cybersecurity Insight: Security posture assessment starts long before exploitation; passive DNS records, HTTP response headers, and open network ports disclose vast intelligence about a target environment.

Professional Reporting Value: Clear documentation connects raw CLI tool outputs directly to risk context, impact levels, and actionable remediations.

Rules of Engagement: All reconnaissance and scanning activities were executed strictly within authorized educational lab scope.












