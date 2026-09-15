Pentester Name

	Hira Ali
	
Program/Batch	B083-Networkwalks

Date	15 September 2026

Modules completed	W2-PM1 (Multiple Kali Tools)

W2-PM5 (Zenmap Scanning)

Client/Target	1. Networkwalks (secured written permission already)

2. My own local LAN Network
Permission secured from client?	Yes

Phases covered	Phase 1: Reconnaissance & Footprinting

Phase 2: Scanning & Network Discovery

This week, I focused on moving from passive information gathering to active network scanning. I broke the work into two main parts: first, footprinting the networkwalks.com domain using various tools in Kali Linux (W2-PM1), and second, running a scan on my own local network using Zenmap on Windows (W2-PM5). Combining these two modules really helped me see the bigger picture of how an attacker transitions from pulling public DNS/domain info to actively mapping out live hosts and open ports on a private network.

I ran all the footprinting commands inside Kali Linux and handled the network scans through the Zenmap GUI on Windows. In the sections below, I’ve documented the exact commands I ran, the actual output I saw, screenshots for proof, and quick takeaways on why each piece of information is valuable from a security and attacker perspective.


Tools I Used 

Here is a quick breakdown of the OS environments and tools I used for these exercises, along with what each one does:

Tool 
What It Does / Why I Used It ,
Kali Linux & Windows :   My primary OS setup—Kali for command-line recon and Windows for host-level tools.

WHOIS     :              Pulls domain registration info (who owns it, creation dates, and nameservers).

whatweb   :              Fingerprints the site to uncover the web server type, CMS, plugins, and IP address.

nslookup   :             Queries DNS to resolve the target domain name to its corresponding IP address.

curl       :              -I	Fetches the HTTP headers from the site to inspect server response details.

wafw00f     :        	Checks if the website is sitting behind a Web Application Firewall (WAF).


dnsrecon     :       	Performs deep DNS enumeration to scrape all available records (NS, MX, SPF, TXT, SRV).

Zenmap (Nmap GUI)	:    Scans my local subnet to discover live hosts, IP addresses, and MAC addresses visually.

Windows CMD    :     	Runs local utility commands (ipconfig, arp) to verify my own machine's IP and network context.


Footprinting & Reconnaissance
For the initial stage of this lab, I ran passive recon against networkwalks.com using six different tools in Kali Linux: WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, and DNSRecon.

My goal here was to act like a real-world attacker gathering public intelligence without directly interacting with or alerting the internal target network.

1. Domain WHOIS Query (whois)
I started by querying the global WHOIS database to gather public domain registration details, registrar contact info, and core hosting infrastructure.

Observed Output:

Registrar: GoDaddy.com, LLC

Creation Date: 2019-11-06

Registry Expiry Date: 2027-11-06

Name Servers: NS6135.HOSTGATOR.COM, NS6136.HOSTGATOR.COM

DNSSEC: Unsigned

2. Web Technology Fingerprinting (whatweb)
Next, I used whatweb to map out the tech stack running on the target web server.

Observed Output:

CMS / Framework: WordPress 7.1

Plugins Detected: WP Download Manager 3.3.58

Web Server: Apache

JavaScript Libraries: jQuery 3.7.1, Bootstrap 7.1

Target IP: 192.232.216.135

3. DNS IP Resolution (nslookup)
I ran nslookup to resolve the human-readable domain name directly to its destination IPv4 address.

Observed Output:

DNS Resolver Used: Google Public DNS (8.8.8.8#53)

Resolved Address: 192.232.216.135

4. HTTP Header Inspection (curl -I)
I used curl with the -I flag to fetch only the HTTP response headers without pulling down the whole web page body.

Observed Output:

HTTP Status: HTTP/2 200

Server Header: Apache

Exposed Endpoint: link: [https://networkwalks.com/wp-json/](https://networkwalks.com/wp-json/); rel="[https://api.w.org/](https://api.w.org/)"

Caching/Proxy: x-nginx-cache: WordPress, x-endurance-cache-level: 0

5. Web Application Firewall Detection (wafw00f)
Before probing deeper, I used wafw00f to check if the site was guarded by a Web Application Firewall (WAF) that might block automated attacks.

Observed Output:

WAF Detected: ModSecurity (SpiderLabs)

Requests Made: 2

6. DNS Record Enumeration (dnsrecon)
Finally, I ran dnsrecon to pull all publicly exposed DNS records associated with the target domain.

Observed Output:

Nameserver IPs: 50.87.144.87 (NS6135) & 192.232.216.131 (NS6136)

Bind Software Versions: 9.16.23-RH

Mail Server (MX): mail.networkwalks.com (192.232.216.135)

TXT / SPF Record: v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all

SRV Records: Exposed _autodiscover._tcp.networkwalks.com pointing to cpanelemaildiscovery.cpanel.net

