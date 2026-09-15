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

