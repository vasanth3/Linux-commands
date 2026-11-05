
# Linux Networking Commands

Linux networking commands are used to configure network interfaces, check IP addresses, troubleshoot connectivity, monitor network connections, and diagnose DNS issues.

These commands are essential for Linux system administrators and DevOps engineers.

## 1. Network Interface Commands

| Command | Description |
|---------|-------------|
| `ip addr` | Display IP addresses |
| `ip a` | Short form of `ip addr` |
| `ip link` | Display network interfaces |
| `ip link show` | Show interface status |
| `ip link set` | Enable or disable an interface |
| `ifconfig` | Display network configuration (legacy) |

### Examples

```bash
# Display IP addresses
ip addr

# Display network interfaces
ip link show

# Display a specific interface
ip addr show ens18

# Enable a network interface
sudo ip link set ens18 up

# Disable a network interface
sudo ip link set ens18 down

# Display interface configuration (legacy)
ifconfig
```

Note: `ens18` is an example interface name. Your interface may be named `eth0`, `ens33`, or something else.

## 2. IP Address and Routing Commands

| Command | Description |
|---------|-------------|
| `ip route` | Display routing table |
| `ip route show` | Show available routes |
| `ip route get` | Show route to a destination |
| `hostname -I` | Display system IP addresses |
| `ip neigh` | Display neighbor/ARP table |

### Examples

```bash
# Display routing table
ip route

# Display default gateway
ip route | grep default

# Check route to Google DNS
ip route get 8.8.8.8

# Display IP addresses
hostname -I

# Display ARP/neighbor table
ip neigh
```

Example routing output:

```text
default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel
```

Here, `192.168.1.1` is the default gateway.

## 3. Connectivity Testing Commands

| Command | Description |
|---------|-------------|
| `ping` | Test network connectivity |
| `traceroute` | Trace network path |
| `tracepath` | Trace network path |
| `mtr` | Combine ping and traceroute |
| `curl` | Test HTTP/HTTPS connectivity |
| `wget` | Download files from the network |

### Examples

```bash
# Test connectivity to Google
ping google.com

# Send only 4 ping packets
ping -c 4 8.8.8.8

# Trace network path
traceroute google.com

# Test website connectivity
curl -I https://google.com

# Download a file
wget https://example.com/file.txt
```

Press `Ctrl + C` to stop a continuous ping.

## 4. DNS Troubleshooting Commands

DNS converts domain names into IP addresses.

| Command | Description |
|---------|-------------|
| `nslookup` | Query DNS records |
| `dig` | Detailed DNS lookup |
| `host` | Perform DNS lookup |
| `resolvectl` | Check DNS resolver information |
| `cat /etc/resolv.conf` | View DNS resolver configuration |

### Examples

```bash
# Resolve a domain name
nslookup google.com

# Query DNS records
dig google.com

# Display only the IP address
dig +short google.com

# Query MX records
dig MX google.com

# Query nameserver records
dig NS google.com

# Check DNS configuration
cat /etc/resolv.conf

# Display DNS status (systemd-resolved)
resolvectl status
```

## 5. Network Connection and Port Commands

| Command | Description |
|---------|-------------|
| `ss` | Display socket information |
| `ss -tuln` | Display listening TCP/UDP ports |
| `ss -tulpn` | Display listening ports and processes |
| `netstat` | Display network connections (legacy) |
| `nc` | Test TCP/UDP connections |
| `lsof -i` | Display network-related open files |

### Examples

```bash
# Display listening ports
sudo ss -tulpn

# Display TCP connections
ss -tan

# Display UDP sockets
ss -uan

# Check whether port 22 is listening
sudo ss -tulpn | grep ':22'

# Test connectivity to port 443
nc -vz google.com 443

# Display network-related processes
sudo lsof -i
```

### Common Network Ports

| Port | Service |
|------|---------|
| 20, 21 | FTP |
| 22 | SSH |
| 25 | SMTP |
| 53 | DNS |
| 80 | HTTP |
| 110 | POP3 |
| 143 | IMAP |
| 443 | HTTPS |
| 3306 | MySQL |
| 5432 | PostgreSQL |
| 8080 | Alternative HTTP |

## 6. Network Configuration Commands

### nmcli

The `nmcli` command manages network connections using NetworkManager.

```bash
# Display network devices
nmcli device status

# Display network connections
nmcli connection show

# Display active connections
nmcli connection show --active

# Display device information
nmcli device show

# Restart a connection
sudo nmcli connection down "Wired connection 1"
sudo nmcli connection up "Wired connection 1"
```

### systemctl

Network services can be managed using `systemctl`.

```bash
# Check NetworkManager status
sudo systemctl status NetworkManager

# Restart NetworkManager
sudo systemctl restart NetworkManager

# Check systemd-resolved status
sudo systemctl status systemd-resolved
```

Note: Restarting networking services on a remote production server may disconnect your SSH session.

## 7. MAC Address and ARP Commands

| Command | Description |
|---------|-------------|
| `ip link` | Display MAC addresses |
| `ip neigh` | Display neighbor table |
| `arp -a` | Display ARP cache (legacy) |
| `ethtool` | Display network interface information |

### Examples

```bash
# Display MAC addresses
ip link show

# Display neighbor table
ip neigh

# Display ARP cache
arp -a

# Display interface information
sudo ethtool ens18
```

## 8. Network Statistics

| Command | Description |
|---------|-------------|
| `ip -s link` | Display interface statistics |
| `sar -n DEV` | Display network activity |
| `iftop` | Monitor network bandwidth |
| `nload` | Monitor incoming and outgoing traffic |
| `vnstat` | Display network traffic statistics |

### Examples

```bash
# Display interface statistics
ip -s link

# Monitor network bandwidth
sudo iftop

# Monitor traffic
nload

# Display traffic statistics
vnstat
```

Install tools on Ubuntu:

```bash
sudo apt update
sudo apt install iftop nload vnstat
```

## 9. Practical Network Troubleshooting

### Step 1: Check IP Address

```bash
ip addr
```

### Step 2: Check Default Gateway

```bash
ip route
```

### Step 3: Test Gateway Connectivity

Replace the gateway address with your actual gateway.

```bash
ping -c 4 192.168.1.1
```

### Step 4: Test Internet Connectivity

```bash
ping -c 4 8.8.8.8
```

### Step 5: Test DNS Resolution

```bash
nslookup google.com
```

### Step 6: Test Website Connectivity

```bash
curl -I https://google.com
```

### Troubleshooting Guide

| Problem | Command |
|---------|---------|
| No IP address | `ip addr` |
| No default gateway | `ip route` |
| Internet connectivity issue | `ping 8.8.8.8` |
| DNS resolution issue | `nslookup google.com` |
| Port not listening | `ss -tulpn` |
| Website not responding | `curl -I https://example.com` |
| Network interface errors | `ip -s link` |

## 10. Production System Administrator Tips

- Check the IP address and interface status before troubleshooting.
- Verify the default gateway using `ip route`.
- Test IP connectivity before checking DNS.
- Use `ss` instead of the older `netstat` command where possible.
- Check listening ports when troubleshooting SSH, Apache, or other services.
- Avoid restarting networking services on remote servers without console access.
- Use `tcpdump` for packet-level troubleshooting when needed.
- Verify firewall rules when a service is unreachable.

## Summary

| Command | Purpose |
|---------|---------|
| `ip addr` | Display IP addresses |
| `ip route` | Display routing table |
| `ping` | Test connectivity |
| `traceroute` | Trace network path |
| `nslookup` | DNS lookup |
| `dig` | DNS troubleshooting |
| `ss` | Check network sockets |
| `curl` | Test HTTP/HTTPS |
| `nc` | Test ports |
| `nmcli` | Manage network connections |
| `ip neigh` | Display neighbor table |
| `tcpdump` | Capture network packets |
