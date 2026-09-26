# day7-networking-mastery-challenge

1. What is DNS?
DNS stands for Domain Name System. It acts as the Internet's phonebook by translating human-readable domain names (like google.com) into machine-readable IP addresses (like 142.250.67.46) so that browsers can load web resources.
2. Explain the difference between HTTP and HTTPS.
 * HTTP (Hypertext Transfer Protocol): Transmits data across the network in plain text without encryption. It operates over default port 80 and is vulnerable to packet sniffing.
 * HTTPS (Hypertext Transfer Protocol Secure): Encrypts all transmitted data using TLS/SSL protocols. It operates over default port 443, ensuring data integrity, confidentiality, and authentication.
3. What is NAT?
NAT stands for Network Address Translation. It is a networking method that translates multiple private IP addresses inside a local network into a single public IP address before routing traffic over the Internet. It conserves IPv4 address space and hides internal network topology from external threats.
4. What is DHCP?
DHCP stands for Dynamic Host Configuration Protocol. It is a network management protocol used to automatically assign IP addresses, subnet masks, default gateways, and DNS server configurations to client devices joining a local network.
5. What is VPN?
VPN stands for Virtual Private Network. It establishes a secure, encrypted communication tunnel over an unsecure public network (such as the Internet). It enables remote users to securely access private company subnets while preventing eavesdropping and data interception.
PART B – COMMANDS (30 Marks)
1. Write the command to check network connectivity.
ping -c 4 google.com

2. Write the command to trace the network path to a destination.
traceroute google.com

3. Write the command to check HTTP response headers.
curl -I https://google.com

4. Write the command to display listening ports.
ss -tuln

5. Write the command to perform a DNS lookup.
nslookup google.com

(Alternative: dig google.com)
PART C – TROUBLESHOOTING SCENARIOS (50 Marks)
Problem 1: A website is not opening. List the troubleshooting steps in the correct order.
 * DNS Resolution Check: Verify domain name to IP translation using nslookup example.com.
 * Network Reachability Check: Send ICMP echo packets to test network connectivity using ping example.com.
 * Path Diagnostics: If ping drops, trace intermediate routers using traceroute example.com to isolate the failing network hop.
 * HTTP/Service Response Check: Check whether the web server returns valid HTTP status codes using curl -I [https://example.com](https://example.com).
 * Internal Server/Port Inspection: SSH into the target host to check active listening ports using ss -tuln and review firewall rules using sudo ufw status.
Problem 2: DNS resolution is failing. Which commands would you use to diagnose the issue?
 * nslookup google.com : Quickly verifies whether your configured local DNS resolver answers queries.
 * dig google.com : Performs a verbose query to inspect DNS authority, TTL values, and return flags.
 * dig @8.8.8.8 google.com : Queries a reliable external DNS server (Google Public DNS) to verify whether the failure is local to your ISP's resolver.
Problem 3: A server is reachable, but the website returns an error. Which command helps verify the HTTP response?
curl -I https://example.com

 * Explanation: This command fetches only the HTTP response headers, showing the exact HTTP status code (such as 500 Internal Server Error, 502 Bad Gateway, or 403 Forbidden) without downloading page content.
Problem 4: An application is running but cannot be accessed externally. Which command would you use to check listening ports?
ss -tuln

 * Explanation: This command lists all active TCP and UDP listening sockets. You can filter for the application's port (e.g., ss -tuln | grep :8080) to verify whether it is bound to 0.0.0.0 (all interfaces) or restricted locally to 127.0.0.1 (localhost only).
Problem 5: Ping fails after the ISP router. What is the most likely cause and how would you investigate further?
 * Most Likely Cause: Upstream network outage at the ISP core, routing misconfigurations, or upstream firewalls intentionally blocking ICMP echo traffic.
 * How to Investigate Further:
   * Run traceroute -I google.com (using ICMP) or traceroute -T -p 443 google.com (using TCP SYN on web ports) to see if regular web packets pass through where ICMP is dropped.
   * Ping an external public DNS IP directly (ping 8.8.8.8) to confirm whether the issue is isolated to a specific target route or general Internet connectivity.
   * Contact the ISP support team with the traceroute hop logs to report the link failure.


