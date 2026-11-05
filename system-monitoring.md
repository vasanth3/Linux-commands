
# System Monitoring in Linux

System monitoring is the process of checking the health, performance, and resource usage of a Linux system.

System administrators monitor:
- CPU usage
- Memory usage
- Disk space
- System load
- Running processes
- Network activity
- System uptime
- System logs

---

## 1. System Uptime

### Check System Uptime

Shows how long the system has been running.

```bash
uptime
```

Example output:

```text
11:30:00 up 5 days, 2:15, 2 users, load average: 0.20, 0.15, 0.10
```

### Show Uptime in a Readable Format

```bash
uptime -p
```

Example:

```text
up 5 days, 2 hours, 15 minutes
```

---

## 2. CPU Monitoring

### Check CPU Load

```bash
uptime
```

The load averages represent the system load over:
- 1 minute
- 5 minutes
- 15 minutes

### Display CPU Information

```bash
lscpu
```

### Monitor CPU Usage in Real Time

```bash
top
```

### Install and Use htop

Ubuntu/Debian:

```bash
sudo apt install htop
```

AlmaLinux/Rocky Linux:

```bash
sudo dnf install htop
```

Run:

```bash
htop
```

`htop` provides an interactive view of CPU, memory, and running processes.

---

## 3. Memory Monitoring

### Check Memory Usage

```bash
free -h
```

Example output:

```text
               total        used        free
Mem:            7.7Gi       2.1Gi       3.5Gi
Swap:           2.0Gi       0.0Gi       2.0Gi
```

### Display Memory Information

```bash
cat /proc/meminfo
```

### Check Memory Usage Continuously

```bash
watch -n 2 free -h
```

This refreshes memory information every 2 seconds.

---

## 4. Disk Monitoring

### Check Filesystem Disk Usage

```bash
df -h
```

Shows disk usage for mounted filesystems.

### Check a Specific Filesystem

```bash
df -h /
```

### Check Directory Size

```bash
du -sh /var
```

### Find the Largest Directories

```bash
sudo du -ah /var | sort -rh | head -10
```

### Display Block Devices

```bash
lsblk
```

---

## 5. Process Monitoring

### Display Running Processes

```bash
ps aux
```

### Display Processes in a Tree

```bash
ps -ef --forest
```

### Find a Specific Process

```bash
ps aux | grep apache2
```

On AlmaLinux or Rocky Linux, Apache is usually named `httpd`:

```bash
ps aux | grep httpd
```

### Monitor Processes in Real Time

```bash
top
```

### Display the Top 10 CPU-Consuming Processes

```bash
ps aux --sort=-%cpu | head -10
```

### Display the Top 10 Memory-Consuming Processes

```bash
ps aux --sort=-%mem | head -10
```

---

## 6. Network Monitoring

### Display Network Interfaces

```bash
ip addr
```

### Display Routing Table

```bash
ip route
```

### Check Network Connections

```bash
ss -tulnp
```

Shows listening TCP and UDP ports.

### Check Network Statistics

```bash
ip -s link
```

### Test Network Connectivity

```bash
ping -c 4 8.8.8.8
```

### Check DNS Resolution

```bash
nslookup google.com
```

---

## 7. System Logs Monitoring

### View System Logs

```bash
sudo journalctl
```

### View Recent System Logs

```bash
sudo journalctl -n 50
```

### Follow Logs in Real Time

```bash
sudo journalctl -f
```

### Check Logs for the Current Boot

```bash
sudo journalctl -b
```

### Check Service Logs

Example: Apache service logs.

Ubuntu/Debian:

```bash
sudo journalctl -u apache2
```

AlmaLinux/Rocky Linux:

```bash
sudo journalctl -u httpd
```

### View Kernel Messages

```bash
sudo dmesg
```

### Follow Kernel Messages

```bash
sudo dmesg -w
```

---

## 8. Service Monitoring

### Check Apache Service Status

Ubuntu/Debian:

```bash
systemctl status apache2
```

AlmaLinux/Rocky Linux:

```bash
systemctl status httpd
```

### Check SSH Service Status

Ubuntu/Debian:

```bash
systemctl status ssh
```

AlmaLinux/Rocky Linux:

```bash
systemctl status sshd
```

### List Failed Services

```bash
systemctl --failed
```

### List Running Services

```bash
systemctl list-units --type=service --state=running
```

---

## 9. Continuous Monitoring

### Monitor Memory Every 5 Seconds

```bash
watch -n 5 free -h
```

### Monitor Disk Usage Every 5 Seconds

```bash
watch -n 5 df -h
```

### Monitor CPU and Processes

```bash
top
```

### Monitor Network Connections

```bash
watch -n 2 ss -tulnp
```

---

## 10. Practical System Administrator Commands

### Check Overall System Health

```bash
uptime
free -h
df -h
lsblk
```

### Check CPU and Memory Usage

```bash
top
```

### Check Disk Space and Large Directories

```bash
df -h
sudo du -sh /var/log
```

### Check Failed Services

```bash
systemctl --failed
```

### Check Recent System Errors

```bash
sudo journalctl -p err -n 50
```

### Check System Information

```bash
hostnamectl
```

---

## Summary

| Monitoring Task | Command |
|-----------------|---------|
| System uptime | `uptime` |
| CPU information | `lscpu` |
| CPU monitoring | `top` |
| Memory usage | `free -h` |
| Disk usage | `df -h` |
| Directory size | `du -sh` |
| Running processes | `ps aux` |
| Network interfaces | `ip addr` |
| Listening ports | `ss -tulnp` |
| System logs | `journalctl` |
| Failed services | `systemctl --failed` |
| System information | `hostnamectl` |

## Important Notes

- Use `top` or `htop` to investigate high CPU usage.
- Use `free -h` to check memory availability.
- Use `df -h` to investigate disk space issues.
- Use `journalctl` to troubleshoot system and service errors.
- Use `systemctl --failed` to identify failed services.
- Monitoring commands such as `top`, `free`, and `df` are generally safe to run on production servers.
