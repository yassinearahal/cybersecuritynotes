#### What is it?
Passive reconnaissance, also known as passive information gathering, is a phase in penetration testing where the tester <mark style="background: #BBFABBA6;">collects information about the target system or organization without directly interacting with it</mark>. It involves gathering publicly available information and data from open sources.

During passive reconnaissance, the tester utilizes various techniques to gather information discreetly, <mark style="background: #BBFABBA6;">without triggering any alerts or leaving a trace</mark>. Some common methods used in passive reconnaissance include:

1. <mark style="background: #FF5582A6;">**Open Source Intelligence (OSINT):**</mark> Gathering information from publicly available sources like search engines, social media, company websites, public databases, online forums, etc.

2. <mark style="background: #FF5582A6;">**DNS Enumeration:**</mark> Gathering information about the target's domain names and associated IP addresses. This can be done using tools like "dig," "nslookup," or "whois" and can reveal various DNS records, such as A, CNAME, MX, and TXT records.

   **Example:** Gather information about internet-connected devices and systems using "Shodan.io"

   [https://www.shodan.io/](https://www.shodan.io/)

     **Example:** gathering information about a domain's DNS configuration using "DNSDumpster":

   [DNSdumpster.com - dns recon and research, find and lookup dns records](https://dnsdumpster.com/)
   
   **Example:** Performing a DNS lookup to find the IP address of a domain using "dig":

   ```bash
   dig @SERVER DOMAIN_NAME TYPE
   ```

   **Example:** Performing a DNS lookup using "nslookup":

   ```bash
   nslookup OPTIONS DOMAIN_NAME SERVER
   ```

   - SERVER is the DNS server that we want to query.
   - DOMAIN_NAME is the domain name we are looking up.
   - TYPE contains the DNS record type.

   **Example:** Checking the WHOIS information of a domain using "whois":

   ```bash
   whois example.com
   ```

1. <mark style="background: #FF5582A6;">**Email Harvesting:**</mark> Searching for email addresses related to the target organization using tools like Email Extractor or Email Spider, which can be useful for social engineering or targeted attacks.

   **Example:** Extracting email addresses from a webpage using the Email Extractor tool:

   ```bash
   theharvester -d example.com -b google
   ```

   This command will use the theHarvester tool to search for email addresses associated with the domain "example.com" using the Google search engine.

4. <mark style="background: #FF5582A6;">**Network Mapping:**</mark> Analyzing publicly available network diagrams, infrastructure details, or IP address ranges to gain insights into the target's network structure.

5. <mark style="background: #FF5582A6;">**Social Engineering:**</mark> Gathering information about individuals within the organization through publicly available sources or by exploiting human vulnerabilities, such as phishing attacks or impersonation.
#### Common DNS Record Types
Here's a small table of common DNS record types and their purposes:

| Record Type | Purpose |
|-------------|---------|
| A           | Maps a domain name to an IPv4 address. |
| AAAA        | Maps a domain name to an IPv6 address. |
| CNAME       | Specifies an alias for another domain name. |
| MX          | Lists mail servers responsible for receiving email. |
| TXT         | Contains text information, often used for SPF and DKIM records. |
| NS          | Specifies the authoritative name servers for a domain. |
| SOA         | Contains information about the domain's primary DNS server. |
