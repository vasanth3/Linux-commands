
# Linux Process Management

Process management in Linux is used to monitor, control, start, and stop running processes.

A process is a running instance of a program. Every process has a unique Process ID (PID).

## 1. Basic Process Commands

| Command | Description |
|---------|-------------|
| `ps` | Display running processes |
| `ps aux` | Display all running processes |
| `ps -ef` | Display processes in full format |
| `top` | Monitor processes in real time |
| `htop` | Interactive process monitoring |
| `pidof` | Find PID of a program |
| `pgrep` | Search processes by name |
| `pstree` | Display processes in tree format |

### Examples

```bash
# Display processes in the current terminal
ps

# Display all processes
ps aux

# Display processes in full format
ps -ef

# Find PID of Apache
pidof apache2

# Search for SSH processes
pgrep sshd

# Display process tree
pstree
```

## 2. Monitor Processes

### top

The `top` command displays real-time system and process information.

```bash
top
```

Important fields:

| Field | Description |
|-------|-------------|
| PID | Process ID |
| USER | Process owner |
| %CPU | CPU usage |
| %MEM | Memory usage |
| TIME+ | Total CPU time |
| COMMAND | Process name |

Useful keys inside `top`:

- `P` - Sort by CPU usage
- `M` - Sort by memory usage
- `k` - Kill a process
- `q` - Quit top

### htop

The `htop` command provides an interactive process viewer.

```bash
htop
```

Install on Ubuntu:

```bash
sudo apt update
sudo apt install htop
```

## 3. Start and Run Processes

### Foreground Process

A foreground process runs directly in the terminal.

```bash
ping google.com
```

Press `Ctrl + C` to stop it.

### Background Process

A background process runs without blocking the terminal.

```bash
ping google.com &
```

The `&` symbol starts the process in the background.

### Run a Process with nohup

The `nohup` command allows a process to continue running after logout.

```bash
nohup ping google.com > ping.log 2>&1 &
```

Explanation:

- `nohup` - Ignore hangup signals
- `>` - Redirect output to a file
- `2>&1` - Redirect errors to the same file
- `&` - Run in the background

## 4. Jobs Management

| Command | Description |
|---------|-------------|
| `jobs` | Display background jobs |
| `bg` | Resume a job in the background |
| `fg` | Bring a job to the foreground |
| `Ctrl + Z` | Suspend the foreground process |
| `Ctrl + C` | Terminate the foreground process |

### Examples

```bash
# Start a process
sleep 300

# Suspend the process
# Press Ctrl + Z

# Display jobs
jobs

# Resume in background
bg

# Bring back to foreground
fg
```

## 5. Kill and Terminate Processes

The `kill` command sends signals to processes.

| Command | Description |
|---------|-------------|
| `kill PID` | Send SIGTERM to a process |
| `kill -9 PID` | Send SIGKILL to a process |
| `kill -15 PID` | Send SIGTERM explicitly |
| `killall process_name` | Send a signal to processes by name |
| `pkill process_name` | Kill processes by name |

### Examples

```bash
# Find process ID
pgrep sleep

# Gracefully terminate a process
kill 1234

# Forcefully terminate a process
kill -9 1234

# Terminate a process by name
pkill sleep

# Terminate all processes named sleep
killall sleep
```

**Important:** Use `kill -9` only when a process does not respond to normal termination. SIGKILL does not allow the process to clean up.

## 6. Process Priority

Linux uses process priority to determine CPU scheduling.

The nice value ranges from `-20` to `19`.

- `-20` - Highest priority
- `0` - Default priority
- `19` - Lowest priority

| Command | Description |
|---------|-------------|
| `nice` | Start a process with a priority |
| `renice` | Change priority of a running process |

### Examples

```bash
# Start a process with low priority
nice -n 10 sleep 300 &

# Start a process with high priority
sudo nice -n -10 sleep 300 &

# Change priority of a running process
sudo renice 10 -p 1234

# Check process priority
ps -o pid,ni,comm -p 1234
```

## 7. Find High CPU and Memory Processes

### Top 5 CPU-Consuming Processes

```bash
ps aux --sort=-%cpu | head -n 6
```

### Top 5 Memory-Consuming Processes

```bash
ps aux --sort=-%mem | head -n 6
```

### Display Process Details

```bash
ps -p 1234 -o pid,ppid,user,%cpu,%mem,cmd
```

## 8. Process Information

| Command | Description |
|---------|-------------|
| `ps -p PID` | Display a specific process |
| `pstree -p` | Display process tree with PIDs |
| `lsof -p PID` | Display files opened by a process |
| `strace -p PID` | Trace system calls of a process |
| `kill -0 PID` | Check whether a process exists and is accessible |

### Examples

```bash
# Display process details
ps -p 1234 -f

# Display files opened by a process
sudo lsof -p 1234

# Check whether a process exists
kill -0 1234
```

## 9. Practical Process Management Exercise

### Step 1: Start a Process

```bash
sleep 500 &
```

### Step 2: Find the Process

```bash
pgrep -a sleep
```

### Step 3: Check Process Details

```bash
ps -ef | grep sleep
```

### Step 4: Monitor the Process

```bash
top
```

### Step 5: Stop the Process

```bash
pkill sleep
```

### Step 6: Verify

```bash
pgrep -a sleep
```

If no matching process is displayed, the process has stopped.

## 10. Production System Administrator Tips

- Check CPU and memory usage using `top` or `htop`.
- Identify high CPU processes using `ps aux --sort=-%cpu`.
- Check memory-consuming processes using `ps aux --sort=-%mem`.
- Use `kill` before `kill -9`.
- Investigate a process before terminating it.
- Use `systemctl` to manage services rather than killing service processes manually.
- Use `nohup` or `tmux` for long-running terminal tasks.
- Check parent processes and dependencies before stopping a process.

## Summary

| Command | Purpose |
|---------|---------|
| `ps` | View processes |
| `top` | Monitor processes |
| `htop` | Interactive monitoring |
| `pgrep` | Search processes |
| `kill` | Terminate processes |
| `pkill` | Terminate processes by name |
| `jobs` | View shell jobs |
| `bg` | Resume jobs in background |
| `fg` | Resume jobs in foreground |
| `nice` | Set process priority |
| `renice` | Change process priority |
| `pstree` | Display process hierarchy |
| `nohup` | Keep process running after logout |
