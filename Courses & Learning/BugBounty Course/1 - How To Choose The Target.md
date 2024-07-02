
-  Choose Those Who have large scope, having multiple subdomain gives you the leverage to test all of them and find the possible vulnerablity
- Use Amass to find the subsdomains
- Use massdns to resolve and find working subdomains

# What is Active Vs Passive scanning
In the context of the Amass tool, which is used for network mapping, asset discovery, and information gathering in cybersecurity, active and passive scanning are two different methods used to gather information about a target. Here's a detailed explanation of each:

### Active Scanning

**Active scanning** involves directly interacting with the target systems to gather information. This method is more intrusive and can be easily detected by the target's security systems. It sends various requests and probes to the target network or domain to gather data.

- **Techniques Used:**
  - **Port Scanning:** Identifying open ports on a target machine.
  - **Service Enumeration:** Determining what services are running on the open ports.
  - **Banner Grabbing:** Collecting information sent by services to understand more about the software and versions in use.
  - **Vulnerability Scanning:** Looking for known vulnerabilities in the services running on the target.

- **Advantages:**
  - Provides detailed and precise information about the target.
  - Can identify specific vulnerabilities that can be exploited.

- **Disadvantages:**
  - Easily detectable by Intrusion Detection Systems (IDS) and firewalls.
  - Can be seen as an aggressive action by the target.

### Passive Scanning

**Passive scanning** involves collecting information about the target without directly interacting with it. Instead, it relies on third-party data sources and publicly available information to gather insights. This method is stealthier and less likely to be detected by the target.

- **Techniques Used:**
  - **DNS Queries:** Using publicly available DNS records to gather information about domain names and IP addresses.
  - **WHOIS Lookups:** Retrieving registration details for domain names and IP address allocations.
  - **Public Data Sources:** Gathering information from sources like search engines, social media, and other databases.
  - **OSINT (Open Source Intelligence):** Collecting data from public sources to build a profile of the target.

- **Advantages:**
  - Less likely to be detected by the target.
  - Can be conducted without any direct interaction with the target systems.

- **Disadvantages:**
  - May not provide as detailed or accurate information as active scanning.
  - Limited by the availability and accuracy of public data sources.

### Use in Amass

Amass utilizes both active and passive scanning techniques to provide comprehensive information about the target domain. Here's how it typically works within Amass:

- **Passive Scanning in Amass:**
  - Amass starts with passive enumeration to gather initial data without alerting the target.
  - It uses sources like public DNS records, WHOIS data, and various APIs to collect information about domain names, subdomains, and related infrastructure.
  
- **Active Scanning in Amass:**
  - Once the passive data collection is complete, Amass can perform active scanning if configured to do so.
  - It can probe identified domains and IPs for open ports, running services, and other details that require direct interaction.

In summary, passive scanning is the initial, stealthier phase of information gathering in Amass, aimed at collecting as much data as possible without interacting with the target. Active scanning follows as a more aggressive phase, where direct probes and scans are conducted to gather detailed and precise information. Both methods together provide a comprehensive view of the target's network and infrastructure.


