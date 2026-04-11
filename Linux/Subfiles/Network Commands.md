---
created: 2026-04-10 21:50
tags:
  - Devops
  - linux
source:
---
> [!quote] The best thing in every noble dream is the dreamer...
> — Moncure Conway

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


Basic network commands for checking connectivity and network configuration.

---

## 📋 `ip addr` — Network Interfaces

Shows all network interfaces and their IP addresses.

### Basic Usage

```Bash
ip addr
ip addr show
```

### Output Example

```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP
    inet 192.168.1.100/24 brd 192.168.1.255 scope global dynamic eth0
       valid_lft 86400sec preferred_lft 86400sec
```

### What it shows:

|Field|Meaning|
|---|---|
|`inet`|IPv4 address|
|`inet6`|IPv6 address|
|`scope`|Where the address is valid (`host`, `global`)|
|`state`|Interface status (`UP`, `DOWN`)|

### Useful Options

```Bash
ip addr show eth0          # Show specific interface
ip -4 addr              # Only IPv4
ip -6 addr              # Only IPv6
ip -br addr             # Brief output
```

---

## 📋 `ping` — Test Connectivity

Tests if your computer can reach another device on the network or internet.

### Basic Usage

```Bash
ping 8.8.8.8            # Test connection to IP
ping google.com           # Test DNS + connection
ping -c 4 google.com    # Send 4 packets only
```

### What `ping` tells you:

- If a device or website is **reachable**
- **Latency** (how long packets take to travel round-trip)
- If you have **DNS issues** (IP works but hostname doesn't)

### Important Flags

|Flag|Purpose|Example|
|---|---|---|
|`-c`|Count (stop after N packets)|`ping -c 4 8.8.8.8`|
|`-i`|Interval between packets|`ping -i 2 google.com`|
|`-t`|TTL value|`ping -t 64 8.8.8.8`|

### Interpret Results

```
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=14 ms
      │         │              │           │
      │         │              │           └── Round-trip time
      │         │              └── TTL (time to live)
      │         └── IP address reached
      └── Packet size
```

---

## 📋 `netstat` — Network Connections

Shows all network connections on your system, plus routing information and interface statistics.

### Basic Usage

```Bash
netstat              # Show all connections
netstat -a           # Show all (listening + established)
netstat -at          # TCP only
netstat -au          # UDP only
```

### Common Flags

|Flag|Purpose|
|---|---|
|`-a`|Show all connections + listening ports|
|`-n`|Show numeric IPs/ports (don't resolve)|
|`-t`|TCP only|
|`-u`|UDP only|
|`-p`|Show program using connection|
|`-r`|Show routing table|

### Examples

```Bash
# Show all listening ports
netstat -tulpn

# Show established connections
netstat -tn

# Show routing table
netstat -rn

# Quick view of listening services
netstat -tuln
```

---

## 📋 `ss` — Modern netstat

A newer, faster replacement for `netstat`.

```Bash
ss -tuln              # Listening ports (like netstat -tuln)
ss -tp                # Show process
ss -tn                # TCP connections
```

---

## 📋 Other Useful Network Commands

### Check DNS

```Bash
nslookup google.com      # DNS lookup
dig google.com          # Detailed DNS info
host google.com         # Simple DNS lookup
```

### Check Port Connectivity

```Bash
# Connect to port (like telnet)
nc -zv 192.168.1.1 80

# Check if port is open
telnet 192.168.1.1 80
```

### Trace Route

```Bash
traceroute google.com    # Trace path to destination
tracepath google.com  # Modern traceroute
```

---

## 📊 Quick Reference Table

|Command|Purpose|
|---|---|
|`ip addr`|Show network interfaces and IPs|
|`ping 8.8.8.8`|Test internet connectivity|
|`netstat -tuln`|Show listening ports|
|`ss -tuln`|Modern netstat alternative|
|`nslookup`|DNS lookup|
|`traceroute`|Trace network path|

---

## 💡 Common Troubleshooting

```Bash
# Check IP address
ip addr show eth0

# Test internet connection
ping -c 3 8.8.8.8

# Check DNS
ping google.com

# See listening ports
netstat -tuln

# Check gateway
ip route show
```

---

## 🔗 Related Notes

- [[grep]]
- [[sed]]
- [[awk]] (for parsing network logs)