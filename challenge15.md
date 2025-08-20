# Day 14: Linux Network Administration Challenge - Complete Guide

## Introduction to Linux Network Administration

Linux network administration forms the backbone of modern infrastructure management. In today's interconnected world, the ability to configure, secure, and troubleshoot network connectivity is an essential skill for any system administrator, DevOps engineer, or SRE. This comprehensive guide covers the fundamental networking concepts and commands that every Linux professional must master.

Effective network administration ensures that services remain available, secure, and performant. Whether you're managing a single server or a large cluster, understanding how to properly configure network interfaces, set up DNS resolution, implement firewall rules, and diagnose connectivity issues is crucial for maintaining reliable systems.

## IP Address Configuration

**Definition:** IP address configuration involves assigning and managing Internet Protocol addresses to network interfaces, enabling communication between devices on local networks and the internet.

**Why we use IP configuration:**
- Enables network communication and data transmission between devices
- Provides unique identification for each device on the network
- Supports both static (manual) and dynamic (DHCP) addressing methods

Read More: https://www.geeksforgeeks.org/computer-networks/differences-between-ipv4-and-ipv6/

**Essential Commands:**
```bash
# View all network interfaces and IP addresses
ip addr show

# Alternative command (older but still useful)
ifconfig

# Show only IPv4 addresses
ip -4 addr show

# Show only IPv6 addresses
ip -6 addr show

# Display routing table
ip route show

# Add a temporary IP address to an interface
sudo ip addr add 192.168.1.100/24 dev eth0

# Remove an IP address from an interface
sudo ip addr del 192.168.1.100/24 dev eth0
```

## Hostname Management

**Definition:** Hostname management involves setting and maintaining the system's network identity, which is used for identification and communication within networks.

**Why we manage hostnames:**
- Provides human-readable identification for systems
- Enables easier network navigation and service discovery
- Supports proper functioning of various network services and applications

**Essential Commands:**
```bash
# Check current hostname
hostnamectl status

# View detailed hostname information
hostnamectl

# Set a new hostname temporarily
sudo hostname new-hostname

# Set a new hostname permanently
sudo hostnamectl set-hostname new-hostname

# View the transient hostname
hostnamectl --transient

# View the static hostname
hostnamectl --static

# View the pretty hostname
hostnamectl --pretty
```

## DNS Configuration

**Definition:** DNS (Domain Name System) configuration involves setting up and managing how the system resolves domain names to IP addresses.

**Why we configure DNS:**
- Translates human-readable domain names to machine-readable IP addresses
- Enables access to internet resources using familiar names
- Supports local network name resolution and service discovery

**Essential Commands:**
```bash
# Check current DNS configuration
cat /etc/resolv.conf

# Test DNS resolution with nslookup
nslookup google.com

# Test DNS resolution with dig
dig google.com

# Test reverse DNS lookup
dig -x 8.8.8.8

# Check DNS response time
dig google.com | grep "Query time"

# View DNS cache statistics (if using systemd-resolved)
systemd-resolve --statistics

# Flush DNS cache
sudo systemd-resolve --flush-caches

# Add entry to local hosts file
echo "192.168.1.100   myserver.local" | sudo tee -a /etc/hosts

# Edit hosts file manually
sudo nano /etc/hosts
```

## Firewall Management with firewalld

**Definition:** firewalld is a dynamic firewall manager that provides a customizable host-based firewall with support for network zones and services.

**Why we use firewalld:**
- Provides flexible and dynamic firewall management
- Supports zone-based configuration for different network environments
- Offers service-based rules for easier management of common applications

**Essential Commands:**
```bash
# Check firewall status
sudo firewall-cmd --state

# List all active zones
sudo firewall-cmd --get-active-zones

# View complete firewall configuration
sudo firewall-cmd --list-all

# View configuration for specific zone
sudo firewall-cmd --zone=public --list-all

# Add HTTP service to firewall
sudo firewall-cmd --add-service=http --permanent

# Add HTTPS service to firewall
sudo firewall-cmd --add-service=https --permanent

# Open specific port
sudo firewall-cmd --add-port=8080/tcp --permanent

# Remove service from firewall
sudo firewall-cmd --remove-service=ssh --permanent

# Reload firewall configuration
sudo firewall-cmd --reload

# Create new zone
sudo firewall-cmd --new-zone=customzone --permanent

# Set default zone
sudo firewall-cmd --set-default-zone=public

# Check if service is allowed
sudo firewall-cmd --query-service=http

# Get list of all available services
sudo firewall-cmd --get-services
```

## Network Troubleshooting Tools

**Definition:** Network troubleshooting involves using various tools to diagnose and resolve connectivity issues, performance problems, and configuration errors.

**Why we troubleshoot networks:**
- Identifies and resolves connectivity issues quickly
- Helps optimize network performance and reliability
- Provides insights for capacity planning and security monitoring

**Essential Commands:**
```bash
# Basic connectivity test
ping -c 4 google.com

# Continuous ping (until stopped)
ping google.com

# Trace network path to destination
traceroute google.com

# Modern path tracing
tracepath google.com

# Show all listening ports
ss -tuln

# Show listening ports with process names
sudo ss -tulnp

# Show established connections
ss -t

# Show UDP connections
ss -u

# Show socket statistics
ss -s

# Monitor network interfaces
ip -s link show

# Check network statistics
netstat -s

# Test specific port connectivity
nc -zv google.com 443

# Test bandwidth with iperf (if installed)
iperf3 -c server-address

# Check MTU settings
ip link show | grep mtu

# Monitor network traffic in real-time
sudo tcpdump -i eth0 -n
```

## System Log Analysis

**Definition:** Log analysis involves examining system logs to identify events, errors, and patterns related to network activity and connectivity.

**Why we analyze logs:**
- Provides historical context for network issues and events
- Helps identify security incidents and unauthorized access attempts
- Supports troubleshooting and forensic analysis of network problems

**Essential Commands:**
```bash
# View NetworkManager logs
journalctl -u NetworkManager --since "today"

# View SSH connection logs
journalctl -u sshd --since "today"

# View system logs with priority 3 (errors) and higher
journalctl -p 3 -b

# Follow logs in real-time
journalctl -f

# View logs for specific time period
journalctl --since "2023-01-01 00:00:00" --until "2023-01-01 23:59:59"

# Filter logs for DHCP events
journalctl -u NetworkManager | grep -i dhcp

# Filter logs for connection issues
journalctl -u NetworkManager | grep -i "failed\|error"

# View kernel messages related to networking
dmesg | grep -i network

# View authentication logs
sudo tail -f /var/log/auth.log

# Check system messages
sudo tail -f /var/log/messages

# Search for specific error patterns
journalctl --since "1 hour ago" | grep -E "(error|fail|denied)"
```

## Comprehensive Network Audit

**Definition:** A network audit involves systematically reviewing and documenting all network configurations, connections, and security settings.

**Why we perform network audits:**
- Ensures compliance with security policies and standards
- Identifies misconfigurations and potential vulnerabilities
- Provides documentation for troubleshooting and planning

**Essential Commands:**
```bash
# Complete network interface information
ip addr show

# Detailed interface statistics
ip -s link show

# Routing table information
ip route show

# ARP table查看
ip neigh show

# All listening ports with processes
sudo lsof -i -P -n

# All established connections
ss -tun

# Firewall configuration overview
sudo iptables -L -n -v

# Network interface configuration files
ls -la /etc/sysconfig/network-scripts/

# Check DNS configuration files
cat /etc/nsswitch.conf

# View network manager connections
nmcli connection show

# Check network service status
systemctl status NetworkManager

# Test all configured DNS servers
for server in $(grep nameserver /etc/resolv.conf | awk '{print $2}'); do
    echo "Testing DNS server: $server"
    dig @$server google.com +short
done

# Check network time synchronization
timedatectl status

# Verify network connectivity to critical services
ping -c 2 gateway-ip
ping -c 2 dns-server
ping -c 2 important-service
```
Deliverables
- A markdown file solution.md with:
  - Commands and outputs for Tasks
  - 3–5 bullet notes on what you learned

---

## Submission Guidelines
- Store your findings and execution steps in solution.md
- Submit it in your GitHub repository and share the link
- Post your experience on social media with #getfitwithsagar #SRELife #DevOpsForAll
- Tag us in your posts for visibility and networking

---

##Join Our Community
- Discord – Ask questions and collaborate: https://discord.gg/mNDm39qB8t
- Google Group – Get updates and discussions: https://groups.google.com/forum/#!forum/daily-devops-sre-challenge-series/join
- YouTube – Watch solution videos and tutorials: https://www.youtube.com/@Sagar.Utekar

---

## Stay Motivated!
Keep it simple. Master the basics. You’ll use these commands every week in real DevOps/SRE work.

Happy Learning!

Best regards,  
Sagar Utekar
