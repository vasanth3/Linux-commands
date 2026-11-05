
# Cron Jobs in Linux

A cron job is a scheduled task that runs automatically at a specific time or interval in Linux.

Cron is commonly used for:
- Automated backups
- System monitoring
- Log cleanup
- Database backups
- Running shell scripts
- Checking server availability

---

## 1. Check Cron Service Status

### Ubuntu/Debian

```bash
sudo systemctl status cron
```

### AlmaLinux/Rocky Linux

```bash
sudo systemctl status crond
```

### Start Cron Service

Ubuntu/Debian:

```bash
sudo systemctl start cron
```

AlmaLinux/Rocky Linux:

```bash
sudo systemctl start crond
```

### Enable Cron at Boot

Ubuntu/Debian:

```bash
sudo systemctl enable cron
```

AlmaLinux/Rocky Linux:

```bash
sudo systemctl enable crond
```

---

## 2. Crontab Commands

### Edit Current User's Crontab

```bash
crontab -e
```

This opens the crontab file in an editor.

### List Current Cron Jobs

```bash
crontab -l
```

### Remove All Cron Jobs for Current User

```bash
crontab -r
```

**Warning:** This removes all cron jobs belonging to the current user. Review your crontab before deleting it.

### Edit Root's Crontab

```bash
sudo crontab -e
```

### List Root's Cron Jobs

```bash
sudo crontab -l
```

---

## 3. Cron Job Syntax

A cron job has five time fields followed by the command.

```text
* * * * * command
| | | | |
| | | | +---- Day of the week (0-7, Sunday is 0 or 7)
| | | +------ Month (1-12)
| | +-------- Day of the month (1-31)
| +---------- Hour (0-23)
+------------ Minute (0-59)
```

Example:

```bash
*/5 * * * * /usr/bin/date
```

This runs the `date` command every 5 minutes.

### Cron Symbols

| Symbol | Meaning | Example |
|--------|---------|---------|
| `*` | Every value | `* * * * *` |
| `,` | Multiple values | `0,30 * * * *` |
| `-` | Range | `0 9-17 * * *` |
| `/` | Step or interval | `*/5 * * * *` |

---

## 4. Common Cron Job Examples

### Every Minute

```bash
* * * * * command
```

### Every 5 Minutes

```bash
*/5 * * * * command
```

### Every 10 Minutes

```bash
*/10 * * * * command
```

### Every 30 Minutes

```bash
*/30 * * * * command
```

### Every Hour

```bash
0 * * * * command
```

### Every Day at Midnight

```bash
0 0 * * * command
```

### Every Day at 2:30 AM

```bash
30 2 * * * command
```

### Every Sunday at 3:00 AM

```bash
0 3 * * 0 command
```

### Every Monday at 9:00 AM

```bash
0 9 * * 1 command
```

### Every Month on the 1st at Midnight

```bash
0 0 1 * * command
```

---

## 5. Practical System Administrator Examples

### Example 1: Run a Script Every 5 Minutes

Create a script:

```bash
nano /home/user/monitor.sh
```

Add:

```bash
#!/bin/bash

echo "System monitoring executed at $(date)"
uptime
free -h
df -h /
```

Make it executable:

```bash
chmod +x /home/user/monitor.sh
```

Edit crontab:

```bash
crontab -e
```

Add:

```bash
*/5 * * * * /home/user/monitor.sh
```

The script will run every 5 minutes.

### Example 2: Save Output to a Log File

```bash
*/5 * * * * /home/user/monitor.sh >> /var/log/monitor.log 2>&1
```

Explanation:

- `>>` appends output to the log file.
- `2>&1` sends errors to the same log file.
- The log file records the script's output.

Make sure the cron user has permission to write to the log file.

### Example 3: Run a Daily Backup

```bash
0 2 * * * /usr/local/bin/backup.sh
```

This runs the backup script every day at 2:00 AM.

### Example 4: Clean Temporary Files

```bash
0 3 * * 0 /usr/bin/find /tmp -type f -mtime +7 -delete
```

This deletes files in `/tmp` that are older than 7 days, every Sunday at 3:00 AM.

**Warning:** Carefully verify the directory and command before using automated deletion on a production server.

---

## 6. Cron Job Logging and Troubleshooting

### Check Cron Logs on Ubuntu/Debian

```bash
sudo grep CRON /var/log/syslog
```

### Check Cron Logs on AlmaLinux/Rocky Linux

```bash
sudo grep CROND /var/log/cron
```

### Check Cron Logs Using journalctl

Ubuntu/Debian:

```bash
sudo journalctl -u cron
```

AlmaLinux/Rocky Linux:

```bash
sudo journalctl -u crond
```

### Check Recent Cron Logs

```bash
sudo journalctl -u cron -n 50
```

For AlmaLinux/Rocky Linux:

```bash
sudo journalctl -u crond -n 50
```

### Troubleshooting Checklist

- Check whether the cron service is running.
- Verify the cron schedule syntax.
- Use absolute paths for scripts and commands.
- Make sure the script has execute permission.
- Check file and directory permissions.
- Redirect output and errors to a log file.
- Check the system timezone.

---

## 7. Cron Environment Variables

Cron runs with a limited environment. It may not have the same `PATH` or environment variables as an interactive terminal.

You can define variables in your crontab:

```bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
SHELL=/bin/bash
```

Example:

```bash
*/5 * * * * /usr/bin/date >> /home/user/cron.log 2>&1
```

Use absolute paths to avoid command-not-found errors.

---

## 8. System-Wide Cron Jobs

System-wide cron jobs can be configured in:

```text
/etc/crontab
/etc/cron.d/
/etc/cron.hourly/
/etc/cron.daily/
/etc/cron.weekly/
/etc/cron.monthly/
```

### View System Crontab

```bash
cat /etc/crontab
```

System-wide cron entries include a username field.

Example in `/etc/crontab`:

```bash
*/5 * * * * root /usr/bin/date >> /var/log/date.log 2>&1
```

This runs the command as `root` every 5 minutes.

---

## 9. Cron Job Practice

### Practice 1

Run a command every minute that writes the current date to a file.

```bash
* * * * * /usr/bin/date >> /home/user/date.log 2>&1
```

### Practice 2

Run a system monitoring script every 5 minutes.

```bash
*/5 * * * * /home/user/monitor.sh >> /home/user/monitor.log 2>&1
```

### Practice 3

Run a backup script every day at 1:00 AM.

```bash
0 1 * * * /home/user/backup.sh
```

### Practice 4

Run a script every Monday at 8:00 AM.

```bash
0 8 * * 1 /home/user/weekly-report.sh
```

---

## Summary

| Task | Command |
|------|---------|
| Edit cron jobs | `crontab -e` |
| List cron jobs | `crontab -l` |
| Remove all user cron jobs | `crontab -r` |
| Every minute | `* * * * *` |
| Every 5 minutes | `*/5 * * * *` |
| Every hour | `0 * * * *` |
| Every day at midnight | `0 0 * * *` |
| Every Sunday at 3 AM | `0 3 * * 0` |
| Check cron service | `systemctl status cron` |
| Check cron logs | `journalctl -u cron` |

## Important Notes

- Cron jobs run automatically in the background.
- Use absolute paths for commands and scripts.
- Test scripts manually before scheduling them.
- Redirect output to log files for troubleshooting.
- Avoid scheduling resource-intensive jobs too frequently.
- Do not use `crontab -r` unless you intend to remove every cron job for that user.
