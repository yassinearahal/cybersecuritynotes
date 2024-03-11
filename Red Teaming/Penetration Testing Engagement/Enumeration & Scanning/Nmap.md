##### <mark style="background: #BBFABBA6;">What is it</mark> ?

Nmap (Network Mapper) is a powerful open-source tool used for <mark style="background: #FF5582A6;">network exploration and security auditing</mark>. It is widely utilized in penetration testing engagements to assess the security posture of target networks. Nmap allows us to discover hosts, scan open ports, identify services running on those ports, and gather information about the target network.

##### **<mark style="background: #BBFABBA6;">Live Host Discovery using ARP</mark>:**

1. **<mark style="background: #FF5582A6;">Host Discovery Methods</mark>:**
    - Nmap employs various methods for host discovery.
    - For local network scans (Ethernet), ARP requests are used when the user is privileged (root or belongs to sudoers).
    - For scans outside the local network, Nmap uses ICMP echo requests, TCP ACK to port 80, TCP SYN to port 443, and ICMP timestamp request.
    - Unprivileged users scanning outside the local network use a TCP 3-way handshake.

2. **<mark style="background: #FF5582A6;">Default Ping Scan</mark>:**
    - Nmap, by default, uses a ping scan to identify live hosts before proceeding with port scanning.
    - Command:
```bash
nmap -sn TARGETS
```

3. **<mark style="background: #FF5582A6;">ARP Scan for Local Subnets</mark>:**
    - ARP scan is effective only on the same subnet as the target systems.
    - Uses ARP queries to obtain MAC addresses, crucial for communication on Ethernet/WiFi.
    - Command for ARP scan only:
```bash
nmap -PR -sn TARGETS
```

4. **<mark style="background: #FF5582A6;">ARP Scan Execution</mark>:**
    - During an ARP scan, Nmap sends ARP requests to all target computers on the same subnet.
    - Targets responding to ARP queries are considered live hosts.
    - Example command:
```bash
nmap -PR -sn TARGETS
```

5. **<mark style="background: #FF5582A6;">Packet Analysis</mark>:**
    - ARP queries generate network traffic.
    - Tools like tcpdump or Wireshark can be used to analyze packets.
    - Captured packets include source and destination MAC addresses, protocol, and ARP request details.

6. **<mark style="background: #FF5582A6;">ARP-Specific Scanner</mark>:**
    - Mention of a specialized ARP scanner: arp-scan.
    - Options to customize scans, such as <mark style="background: #BBFABBA6;">`arp-scan --localnet` or `arp-scan -l`</mark>.
    - Interface specification example: <mark style="background: #BBFABBA6;">`sudo arp-scan -I eth0 -l`</mark>

##### **<mark style="background: #BBFABBA6;">Live Host Discovery using ICMP Protocol</mark>:**

1. **<mark style="background: #FF5582A6;">ICMP Echo Request (Ping) Method</mark>:**
    - ICMP Type 8/Echo requests are sent to every IP address on the target network.
    - Responses in the form of ICMP Type 0/Echo replies indicate live hosts.

2. **<mark style="background: #FF5582A6;">Reliability Considerations</mark>:**
    - ICMP echo requests might be blocked by firewalls.
    - Newer MS Windows versions often have default host firewalls blocking ICMP echo requests.
    - ARP query precedes ICMP request for same-subnet targets.

3. **<mark style="background: #FF5582A6;">ICMP Echo Request Scan Option</mark>:**
    - To use ICMP echo requests for live host discovery, add the option -PE.
    - Use -sn to avoid port scanning after host discovery.

4. **<mark style="background: #FF5582A6;">Scan Execution</mark>:**
    - Results show live hosts and their MAC addresses (if on the same subnet)
    - Example command:
```bash
sudo nmap -PE -sn MACHINE_IP/24
```

3. **<mark style="background: #FF5582A6;">Alternative ICMP Requests</mark>:**
    - ICMP Timestamp Requests (ICMP Type 13) can be used for host discovery.
    - Use -PP option for timestamp requests.
    - ICMP Address Mask Queries (ICMP Type 17) with -PM option can also be employed.

4. **<mark style="background: #FF5582A6;">Timestamp Request Scan</mark>:**
    - Expect live hosts to reply with ICMP timestamp replies.
    - Example command:
```bash
sudo nmap -PP -sn MACHINE_IP/24
```

5. **<mark style="background: #FF5582A6;">Address Mask Query Scan</mark>:**
    - Expects live hosts to reply to ICMP address mask queries.
    - Example command:
```bash
sudo nmap -PM -sn MACHINE_IP/24
```

6. **<mark style="background: #FF5582A6;">Results and Reliability</mark>:**
    - Multiple ICMP request types ensure flexibility.
    - Firewalls or target configurations may block specific ICMP types.
    - Importance of learning and using multiple approaches for reliable results.

7. **<mark style="background: #FF5582A6;">Caution with ICMP Address Mask Queries:</mark>**
    - Despite earlier successful scans, an ICMP address mask query scan may return no results.
    - Possible reasons include target or firewall blocking this ICMP packet type.

##### **<mark style="background: #BBFABBA6;">Live Host Discovery using TCP and UDP</mark>:**

1. **<mark style="background: #FF5582A6;">TCP SYN Ping</mark>:**
    - Send a packet with SYN flag set to a TCP port (e.g., 80) and wait for a response.
    - Open port replies with SYN/ACK; closed port results in RST.
    - Use option -PS followed by port number, range, or list.
    - Privileged users can send SYN packets without completing the 3-way handshake.

2. **<mark style="background: #FF5582A6;">Execution Example</mark>:**
    - Results show live hosts and their response to TCP SYN pings.
    - Command:
```bash
sudo nmap -PS -sn MACHINE_IP/24
```

3. **<mark style="background: #FF5582A6;">Port Specification</mark>:**
    - Specify ports using -PS21, -PS21-25, or -PS80,443,8080.

4. **<mark style="background: #FF5582A6;">Network Traffic Analysis</mark>:**
    - Wireshark can capture network traffic during the scan.
    - TCP SYN scan uses common ports, like port 80, for the ping.

5. **<mark style="background: #FF5582A6;">TCP ACK Ping</mark>:**
    - Sends a packet with ACK flag set; expects an RST flag in response.
    - Requires privileged user (root or sudoers).
    - Default port is 80, and syntax is similar to TCP SYN ping (-PA).

6. **<mark style="background: #FF5582A6;">Execution Example - TCP ACK Ping</mark>:**
    - Detects live hosts based on TCP ACK responses.
    - Command:
```bash
sudo nmap -PA -sn MACHINE_IP/24
```

7. **<mark style="background: #FF5582A6;">Network Traffic Analysis - TCP ACK Ping</mark>:**
    - Captured packets show ACK packets sent to port 80; each packet sent twice.
    - Systems not responding are considered offline or inaccessible.

8. **<mark style="background: #FF5582A6;">UDP Ping</mark>:**
    - UDP packet to an open port doesn't trigger a reply.
    - UDP packet to a closed UDP port expects an ICMP port unreachable response.
    - Syntax: -PU for UDP ping.

9. **<mark style="background: #FF5582A6;">Execution Example - UDP Ping</mark>:**
    - Discovers live hosts based on UDP ping.
    - Command:
```bash
sudo nmap -PU -sn MACHINE_IP/24
```

10. **<mark style="background: #FF5582A6;">Network Traffic Analysis - UDP Ping</mark>:**
    - Captured packets show UDP packets sent; no response from open UDP ports.
    - Closed UDP ports trigger ICMP port unreachable responses.

11. **<mark style="background: #FF5582A6;">Overall Strategy</mark>:**
    - TCP SYN, TCP ACK, and UDP pings offer different approaches for host discovery.
    - Multiple methods ensure robust results in varying network conditions.

12. **<mark style="background: #FF5582A6;">Caution with UDP Ping</mark>:**
    - Sending UDP packets to open UDP ports does not trigger responses.
    - Closed UDP ports triggering ICMP responses indirectly confirm host availability.

##### **<mark style="background: #BBFABBA6;">Live Host Discovery using Reverse DNS Lookup</mark>:**

1. **<mark style="background: #FF5582A6;">Default Behavior</mark>:**
   - Nmap, by default, utilizes reverse DNS lookup for online hosts during the scanning process.
   - Reverse DNS can provide valuable information by revealing hostnames.

2. **<mark style="background: #FF5582A6;">Skip DNS Queries</mark>:**
   - To skip DNS queries and speed up the scanning process, use the option -n.

3. **<mark style="background: #FF5582A6;">Query DNS for Offline Hosts</mark>:**
   - Use -R option to instruct Nmap to query the DNS server even for hosts that appear offline.
   - This option can reveal additional information by attempting reverse DNS lookup on all hosts.

4. **<mark style="background: #FF5582A6;">Specify DNS Server</mark>:**
   - If we want to use a specific DNS server, include the --dns-servers DNS_SERVER option.
   - This allows us to customize the DNS server used for reverse DNS lookup.

5. **<mark style="background: #FF5582A6;">Execution Example - Default Behavior</mark>:**
   - Command: <mark style="background: #BBFABBA6;">`nmap TARGET`</mark>
   - Default behavior involves reverse DNS lookup for online hosts.

6. **<mark style="background: #FF5582A6;">Execution Example - Skip DNS Queries</mark>:**
   - Command: <mark style="background: #BBFABBA6;">`nmap -n TARGET`</mark>
   - Skips reverse DNS lookup for faster scanning.

7. **<mark style="background: #FF5582A6;">Execution Example - Query DNS for Offline Hosts</mark>:**
   - Command: `nmap -R TARGET`
   - Queries DNS server even for hosts that appear offline.

8. **<mark style="background: #FF5582A6;">Execution Example - Specify DNS Server</mark>:**
   - Uses the specified DNS server for reverse DNS lookup.
   - Command:
```bash
nmap --dns-servers DNS_SERVER TARGT
```

9. **<mark style="background: #FF5582A6;">Considerations</mark>:**
   - Reverse DNS lookup provides additional context by revealing hostnames.
   - Skipping DNS queries with -n can be beneficial for faster scans, especially on large networks.
   - Querying DNS for offline hosts with -R may uncover additional information.
   - Customizing DNS servers allows flexibility in choosing the DNS infrastructure for reverse lookup.

##### **<mark style="background: #BBFABBA6;">Port Scan (TCP and UDP)</mark>:**

1. **<mark style="background: #FF5582A6;">Port Identification</mark>:**
   - TCP and UDP ports identify network services on a host.
   - A server adheres to a specific network protocol and provides a service on a port.

2. **<mark style="background: #FF5582A6;">Default Port Bindings</mark>:**
   - Ports are linked to specific services through port numbers.
   - Example: HTTP server binds to TCP port 80, HTTPS to TCP port 443.

3. **<mark style="background: #FF5582A6;">Port Classification</mark>:**
   - <mark style="background: #D2B3FFA6;">Open Port</mark>:
      - Indicates a service is actively listening on the specified port.
   - <mark style="background: #D2B3FFA6;">Closed Port</mark>:
      - Indicates no service is listening on the specified port.
   - <mark style="background: #D2B3FFA6;">Filtered Port</mark>:
      - Nmap cannot determine if the port is open or closed.
      - Typically due to firewall restrictions preventing access.

4. **<mark style="background: #FF5582A6;">Practical Considerations</mark>:**
   - <mark style="background: #D2B3FFA6;">Firewall Impact</mark>:
      - Open port may still be blocked by a firewall.
      - Closed port is accessible but not actively serving.

5. **<mark style="background: #FF5582A6;">Nmap Port States</mark>:**
   - <mark style="background: #D2B3FFA6;">Open</mark>:
      - Service actively listens on the port.
   - <mark style="background: #D2B3FFA6;">Closed</mark>:
      - No service is listening, port is accessible.
   - <mark style="background: #D2B3FFA6;">Filtered</mark>:
      - Nmap cannot determine port status due to accessibility issues.
      - Often caused by firewall interference.
   - <mark style="background: #D2B3FFA6;">Unfiltered</mark>:
      - Nmap cannot determine port status, encountered in ACK scan (-sA).
   - <mark style="background: #D2B3FFA6;">Open|Filtered</mark>:
      - Nmap cannot distinguish between open or filtered.
   - <mark style="background: #D2B3FFA6;">Closed|Filtered</mark>:
      - Nmap cannot decide between closed or filtered.

6. **<mark style="background: #FF5582A6;">Firewall Considerations</mark>:**
   - <mark style="background: #D2B3FFA6;">Open Port Behind Firewall</mark>:
      - May not be reachable due to firewall blocking packets.
   - <mark style="background: #D2B3FFA6;">Closed Port Behind Firewall</mark>:
      - Accessible but not actively serving; may be blocked by a firewall.

##### **<mark style="background: #BBFABBA6;">Nmap TCP Flags</mark>:**

1. **<mark style="background: #FF5582A6;">TCP Header Overview</mark>:**
   - The TCP header consists of the first 24 bytes of a TCP segment.
   - Divided into rows, with the first row containing source and destination port numbers.

2. **<mark style="background: #FF5582A6;">TCP Header Flags</mark>:**
   - <mark style="background: #D2B3FFA6;">URG (Urgent)</mark>:
      - Indicates the urgent pointer field is significant.
      - Incoming data marked as urgent, processed immediately without waiting on previous segments.

   - <mark style="background: #D2B3FFA6;">ACK (Acknowledgement)</mark>:
      - Acknowledges the receipt of a TCP segment.
      - Significant in acknowledging data reception.

   - <mark style="background: #D2B3FFA6;">PSH (Push)</mark>:
      - Instructs TCP to promptly pass the data to the application.
      - Requests immediate data delivery.

   - <mark style="background: #D2B3FFA6;">RST (Reset</mark>):
      - Used to reset the connection.
      - Sent by devices like firewalls to terminate a TCP connection.
      - Indicates no service on the receiving end to respond.

   - <mark style="background: #D2B3FFA6;">SYN (Synchronize)</mark>:
      - Initiates a TCP 3-way handshake.
      - Synchronizes sequence numbers with the other host.
      - Sequence number set randomly during TCP connection establishment.

   - <mark style="background: #D2B3FFA6;">FIN (Finish)</mark>:
      - Indicates the sender has no more data to send.
      - Signifies the end of the data stream.

2. **<mark style="background: #FF5582A6;">Flag Setting</mark>:**
   - Setting a flag bit to 1 activates the corresponding function.
   - Flags play a crucial role in TCP communication, signaling various conditions.

#### <mark style="background: #BBFABBA6;">Port Scan Types</mark>

1. <mark style="background: #FF5582A6;">Fast Scan</mark>:
    - Performs a quick scan of the most common 1,000 ports.
    - Provides a fast overview of open ports on the target system.
    - Suitable for initial reconnaissance or when time is limited.
```bash
sudo nmap -F {target_IP}
```

1. <mark style="background: #FF5582A6;">TCP Connect Scan</mark>:
    - Initiates a full TCP connection to each targeted port.
    - Determines whether the port is open, closed, or filtered.
    - Most commonly used scan type.
```bash
sudo nmap -sT {target_IP}
```

1. <mark style="background: #FF5582A6;">UDP Scan</mark>:
    - Scans for open UDP ports on target systems.
    - UDP is connectionless, so this scan involves sending UDP packets and analyzing responses.
    - An ICMP packet of type 3, destination unreachable, and code 3, port unreachable, is expected.
    - Nmap identifies open UDP ports based on the absence of response.
    - Helps identify services and potential vulnerabilities running on UDP ports.
```bash
sudo nmap -sU {target_IP}
```

1. <mark style="background: #FF5582A6;">SYN Scan (Half-open Scan)</mark>:
    - Sends SYN packets to target ports without completing the TCP handshake.
    - Determines if the port is open or closed based on the response received.
    - Faster and more stealthy than TCP Connect Scan.
```bash
sudo nmap -sS {target_IP}
```

1. <mark style="background: #FF5582A6;">Null Scan</mark>:
    - Sends packets with no TCP flags set.
    - Exploits a quirk in the TCP/IP protocol stack to determine if the port is open, closed, or filtered.
    - Useful for bypassing certain firewall rules.
```bash
sudo nmap -sN {target_IP}
```

1. <mark style="background: #FF5582A6;">FIN Scan</mark>:
    - Sends packets with only the FIN flag set.
    - Exploits the behavior of closed ports to determine if they are open, closed, or filtered.
    - Useful for stealthy scanning.
```bash
sudo nmap -sF {target_IP}
```

1. <mark style="background: #FF5582A6;">Xmas Scan</mark>:
    - Sends packets with the FIN, PSH, and URG flags set.
    - Exploits the behavior of closed ports to determine if they are open, closed, or filtered.
    - Can be used for evading firewalls or intrusion detection systems (IDS).
```bash
sudo nmap -sX {target_IP}
```

1. <mark style="background: #FF5582A6;">ACK Scan</mark>:
    - Sends TCP packets with the ACK flag set, a flag normally used to acknowledge received data.
    - The target's response helps identify ports not blocked by a firewall, revealing potential vulnerabilities.
    - Specifically beneficial for probing firewall rule sets and configurations.
```bash
sudo nmap -sA {target_IP}
```

1. <mark style="background: #FF5582A6;">Maimon Scan</mark>:
    - Attempts to identify open ports using set FIN and ACK bits.
    - Target should ideally respond with an RST packet.
    - Certain BSD-derived systems drop the packet on open ports, exposing them.
```bash
sudo nmap -sM {target_IP}
```

1. <mark style="background: #FF5582A6;">Window Scan</mark>:
    - Similar to ACK scan but examines TCP Window field in RST packets.
    - Reveals open ports on specific systems based on responses to ACK packets.
    - Provides nuanced insights into firewall configurations and open ports.
```bash
sudo nmap -sW {target_IP}
```

1.  <mark style="background: #FF5582A6;">Custom Scan</mark>:
    - Use <mark style="background: #BBFABBA6;">`--scanflags`</mark> for custom TCP flag combinations.
    - Efficiently map firewall rules with ACK and Window scans.
    - Gain insights into firewall behavior using varied TCP flags.
    - The absence of firewall blocks doesn't guarantee active services.
    - ACK and Window scans expose firewall rules, not the availability of active services.
```bash
sudo nmap --scanflags RSTSYNFIN {target_IP}
```

1. <mark style="background: #FF5582A6;">Service Version Detection</mark>:
    - Identifies the specific versions of services running on open ports.
    - Provides information about potential vulnerabilities associated with particular software versions.
    - Forces connection establishment; stealth SYN scan (-sS) not possible with -sV.
    - Intensity level (0-9), where 0 is the lightest and 9 is the most complete.
    - Helps in targeting specific exploits.
```bash
sudo nmap -sV {target_IP}
```

1. <mark style="background: #FF5582A6;">Idle Scan (Zombie Scan</mark>):
- <mark style="background: #D2B3FFA6;">Purpose</mark>: Stealthy scanning by masking probes through an idle (zombie) host.
- <mark style="background: #D2B3FFA6;">Requirements</mark>: Idle system on the network for communication.
- <mark style="background: #D2B3FFA6;">Operation</mark>:
  1. Trigger idle host response, record its IP ID.
  2. Send spoofed SYN packet to target port from idle host's IP.
  3. Trigger idle host response again, record new incremented IP ID.
- <mark style="background: #D2B3FFA6;">Analysis</mark>:
  - If port closed, target responds with RST, idle host's IP ID remains unchanged.
  - If port open, target responds with SYN/ACK, idle host's IP ID increments by 1.
  - If no response (firewall), idle host's IP ID remains unchanged.
- <mark style="background: #D2B3FFA6;">Result Interpretation</mark>:
  - If IP ID difference is 1, port on target closed or filtered.
  - If IP ID difference is 2, port on target open.
```bash
nmap -sI ZOMBIE_IP TARGET_IP
```

1. <mark style="background: #FF5582A6;">OS Detection</mark>:
    - Attempts to identify the operating system running on the target system.
    - Provides information about the target's network environment.
    - Accuracy relies on finding at least one open and one closed port.
```bash
sudo nmap -O {target_IP}
```

1. <mark style="background: #FF5582A6;">Script Scanning</mark>:
    - Utilizes Nmap Scripting Engine (NSE) to run predefined or custom scripts against target hosts.
    - Helps identify common vulnerabilities and misconfigurations.
    - Allows for more advanced scanning and information gathering.
```bash
sudo nmap -sC {target_IP}
```

 1. <mark style="background: #FF5582A6;">Spoofed IP and MAC Scans</mark>:
    - Feasible in specific network setups.
    - Reliable responses require the ability to capture the replies on the network.

        - <mark style="background: #D2B3FFA6;">Steps for Spoofed IP Scan</mark>:
        1. Attacker sends a packet with a spoofed source IP to the target.
        2. Target replies to the spoofed IP as the destination.
        3. Attacker captures replies to identify open ports.

        - <mark style="background: #D2B3FFA6;">Configuration for Spoofed IP Scan</mark>:
        1. Use `-e NET_INTERFACE` to specify the network interface.
        2. Disable ping scan with `-Pn`.

        - <mark style="background: #D2B3FFA6;">MAC Address Spoofing</mark>:
        1. Works when attacker and target are on the same subnet.
        2. Specify source MAC with `--spoof-mac SPOOFED_MAC`.
 
1. <mark style="background: #FF5582A6;">Decoy Scans</mark>:
    - Conceal attacker's IP by blending with decoy addresses.
    - Use `-D` to specify decoy IP addresses.

    - Decoy Scan Examples:
```bash
nmap -D 10.10.0.1,10.10.0.2,ME 10.10.145.170
nmap -D 10.10.0.1,10.10.0.2,RND,RND,ME 10.10.145.170
```

##### **<mark style="background: #BBFABBA6;">Port Specification and Scan Timing</mark>:**

1. **<mark style="background: #FF5582A6;">Port Specification</mark>:**
   - Specify ports using intuitive options:
      - `-p22,80,443` scans specific ports 22, 80, and 443.
      - `-p1-1023` scans ports between 1 and 1023 inclusive.
      - `-p20-25` scans ports between 20 and 25 inclusive.
      - `-p-` scans all 65535 ports.
      - `-F` scans the most common 100 ports.
      - `--top-ports 10` checks the ten most common ports.

2. **<mark style="background: #FF5582A6;">Scan Timing Options</mark>:**
   - Control scan timing using `-T<0-5>` templates.
      - `-T0` is the slowest (paranoid).
      - `-T5` is the fastest (insane).
    - Templates include paranoid, sneaky, polite, normal, aggressive, and insane.

3. **<mark style="background: #FF5582A6;">Timing Considerations</mark>:**
    - `-T0` or `-T1` for stealth and avoiding IDS alerts.
    - `-T3` (normal) is the default timing.
    - `-T4` for CTFs and practice targets.
    - `-T1` for real engagements prioritizing stealth.

4. **<mark style="background: #FF5582A6;">Packet Rate Control</mark>:**
    - Adjust packet rate with `--min-rate <number>` and `--max-rate <number>`.
      - `--max-rate 10` ensures a maximum of ten packets per second.

5. **<mark style="background: #FF5582A6;">Probing Parallelization</mark>:**
    - Control parallelization with `--min-parallelism <numprobes>` and `--max-parallelism <numprobes>`.
      - `--min-parallelism=512` maintains at least 512 probes in parallel.
    - Specifies the number of probes for live host and open port discovery.

##### <mark style="background: #BBFABBA6;">Differences between Null, FIN, and Xmas scans</mark>

- <mark style="background: #FF5582A6;">Null Scan</mark>:
    - Sends packets with no TCP flags set.
    - If a port is open, it will typically ignore the null packets.
    - If a port is closed, it may respond with a TCP RST packet.
    - If a port is filtered, it may not respond at all.
    - Can bypass certain firewall rules.
- <mark style="background: #FF5582A6;">FIN Scan</mark>:
    - Sends packets with only the FIN flag set.
    - If a port is closed, it will typically respond with a TCP RST packet.
    - If a port is open or filtered, it may ignore the FIN packets.
    - Can be used for stealthy scanning.
- <mark style="background: #FF5582A6;">Xmas Scan</mark>:
    - Sends packets with the FIN, PSH, and URG flags set.
    - If a port is closed, it will typically respond with a TCP RST packet.
    - If a port is open or filtered, it may ignore the Xmas packets.
    - Can be used for evading firewalls or intrusion detection systems (IDS).

#### <mark style="background: #BBFABBA6;">NSE Scripts</mark>:

1. <mark style="background: #FF5582A6;">Introduction to NSE Scripts</mark>:
    
    - NSE scripts extend the functionality of Nmap for network scanning.
    - They automate tasks and gather information during penetration tests.
2. <mark style="background: #FF5582A6;">Searching for NSE Scripts</mark>:
    
    - Use the `--script` or `-p` option to search for specific scripts or categories.
    - Display script information with `--script-help`.
    - Localy @ /usr/share/nmap/scripts
1. <mark style="background: #FF5582A6;">Working with NSE Scripts</mark>:
    
    - NSE scripts perform various tasks, including:
    
    a. <mark style="background: #BBFABBA6;">Discovery</mark>:
    
    - `discovery`: Discover hosts and services on a network.
    - `broadcast`: Send and receive custom ARP and UDP packets.
    
    b. <mark style="background: #BBFABBA6;">Vulnerability Scanning</mark>:
    
    - `vuln`: Identify vulnerabilities in target systems.
    - `exploit`: Exploit known vulnerabilities.
    
    c. <mark style="background: #BBFABBA6;">Brute Forcing</mark>:
    
    - `brute`: Perform brute force attacks on various services.
    
    d. <mark style="background: #BBFABBA6;">Enumeration</mark>:
    
    - `dns`: Enumerate DNS information and perform zone transfers.
    - `snmp`: Gather information from SNMP-enabled devices.
    
    e. <mark style="background: #BBFABBA6;">Exploitation and Post-Exploitation</mark>:
    
    - `exploit`: Exploit known vulnerabilities.
    - `post`: Gather information or perform actions after exploitation.
    
    f. <mark style="background: #BBFABBA6;">Web Application Testing</mark>:
    
    - `http`: Perform various tests on HTTP services.
    - `ssl`: Test SSL/TLS configurations and vulnerabilities.
    
    g. <mark style="background: #BBFABBA6;">Miscellaneous</mark>:
    
    - `default`: Perform default scripts.
    - `safe`: Perform safe scanning without causing disruptions.
    
1. <mark style="background: #FF5582A6;">Example Usage</mark>:
    - Description: Perform vulnerability scanning using the `vuln` category.
```bash
sudo nmap -sV --script=vuln {target_IP}
```

####  <mark style="background: #BBFABBA6;">Firewall & IDS Evasion</mark>:
1. <mark style="background: #FF5582A6;">ICMP Block</mark>:
    - Windows firewall blocks ICMP packets, including ping.
    - Nmap treats hosts with ICMP block as dead and skips scanning.
    - Solution: Use Nmap's `-Pn` option to bypass ICMP check.
2. <mark style="background: #FF5582A6;">ARP Requests</mark>:
    - When on the local network, Nmap can use ARP requests to detect host activity.
3. <mark style="background: #FF5582A6;">Fragmentation (-f)</mark>:
    - Fragments packets to avoid detection by firewalls or IDS.
    - Fragmentation involves breaking IP data into smaller units.
    - The process involves dividing IP data, utilizing the identification (ID) and fragment offset.
    - Alternative: <mark style="background: #BBFABBA6;">`--mtu <number>`</mark> for controlling packet size.
    - Employ the <mark style="background: #BBFABBA6;">`-f` </mark>option to fragment IP packets during scans.
    - Use `-ff` or customize with <mark style="background: #BBFABBA6;">`--mtu`</mark> for different fragment sizes.
    - Nmap provides flexibility with options like <mark style="background: #BBFABBA6;">`--data-length NUM`</mark> for adjusting packet sizes.
1. <mark style="background: #FF5582A6;">Scan Delay (--scan-delay <time>ms)</mark>:
    - Adds a delay between packets to evade time-based firewall/IDS triggers.
    - Useful for unstable networks.
5. <mark style="background: #FF5582A6;">Invalid Checksum (--badsum)</mark>:
    - Generates packets with an invalid checksum.
    - Firewalls may respond automatically without checking the checksum.
    - Can be used to detect the presence of a firewall/IDS.
6. <mark style="background: #FF5582A6;">Source Port Spoofing (--source-port <port>)</mark>:
    - Spoofs the source port number to appear as a legitimate service.
    - Helps bypass firewall rules that only allow specific ports.
7. <mark style="background: #FF5582A6;">Data Payload Manipulation (-data <hex_string>)</mark>:
    - Modifies the payload data to evade signature-based detection.
    - Useful for bypassing deep packet inspection mechanisms.
8. <mark style="background: #FF5582A6;">Traffic Obfuscation (--data-length <size>)</mark>:
    - Modifies packet length to disguise network traffic.
    - Helps evade traffic analysis and firewall rules.
9. <mark style="background: #FF5582A6;">IP Fragmentation (--fuzzy)</mark>:
    - Randomizes packet fragmentation to avoid pattern detection.
    - Makes it harder for firewalls to reconstruct and analyze packets.
10. <mark style="background: #FF5582A6;"> Randomized Timing (--randomize-hosts)</mark>:
    - Randomizes timing between hosts to avoid pattern recognition.
    - Helps evade firewalls with behavior-based detection.

```bash
sudo nmap -Pn -f --scan-delay 500ms --badsum --source-port 80 --data "Hello" --data-length 100 --fuzzy --randomize-hosts {target_IP}
```

##### <mark style="background: #BBFABBA6;">Advanced Options</mark>:
```bash
1. `--reason`: Display the reason a port is in a particular state.
2. `--traceroute`: Perform a hop-by-hop traceroute to the target.
3. `--open`: Show only open ports.
4. `--packet-trace`: Display the route packets take during the scan.
5. `--osscan-limit`, `--osscan-guess`: Limit OS detection to promising targets and make a best guess when OS detection is inconclusive.
6. `--script`: Run specific Nmap scripts.
7. `--version-intensity`: Control the intensity of version detection.
8. `--top-ports`: Scan the top N most common ports.
9. `--max-retries`: Set the maximum number of port scan probe retransmissions.
10. `--max-scan-delay`: Adjust the maximum delay between probes.
11. `--min-rate`, `--max-rate`: Control the packet rate.
12. `--allports`: Scan all 65535 ports.
13. `--aggressive`: Enable aggressive scan options.
14. `--service-version`: Enable service version detection.
15. `--host-timeout`: Set the maximum time allowed for host discovery.
16. `--max-hostgroup`: Set the maximum number of hosts in a single hostgroup.
17. `--scan-delay`: Adjust the delay between Nmap probes.
18. `-oN`, `-oX`, `-oG`: Specify output formats (normal, XML, grepable).
19. `--open`: Show only open ports.
20. `-oA`: Output in all formats.
```