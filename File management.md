
# Linux File Management Commands

Linux file management commands are used to create, view, copy, move, rename, and delete files and directories.

## 1. Create Files and Directories

| Command | Use | Example |
|---------|-----|---------|
| `touch` | Create an empty file | `touch file.txt` |
| `mkdir` | Create a directory | `mkdir documents` |
| `mkdir -p` | Create nested directories | `mkdir -p dir1/dir2/dir3` |

### Examples

```bash
touch file.txt
mkdir documents
mkdir -p backup/2026/linux
```

## 2. List Files and Directories

| Command | Use | Example |
|---------|-----|---------|
| `ls` | List files and directories | `ls` |
| `ls -l` | Display detailed file information | `ls -l` |
| `ls -a` | Show hidden files | `ls -a` |
| `ls -lh` | Show file sizes in readable format | `ls -lh` |

### Examples

```bash
ls
ls -la
ls -lh /var/log
```

## 3. Copy Files and Directories

| Command | Use | Example |
|---------|-----|---------|
| `cp` | Copy a file | `cp file.txt backup.txt` |
| `cp -r` | Copy a directory recursively | `cp -r documents backup` |
| `cp -i` | Ask before overwriting | `cp -i file.txt backup.txt` |

### Examples

```bash
cp file.txt file-backup.txt
cp -r documents documents-backup
```

## 4. Move and Rename Files

| Command | Use | Example |
|---------|-----|---------|
| `mv` | Move a file | `mv file.txt /tmp/` |
| `mv` | Rename a file | `mv old.txt new.txt` |
| `mv -i` | Ask before overwriting | `mv -i file.txt backup.txt` |

### Examples

```bash
mv file.txt /tmp/
mv oldname.txt newname.txt
mv documents /home/user/
```

## 5. Delete Files and Directories

| Command | Use | Example |
|---------|-----|---------|
| `rm` | Remove a file | `rm file.txt` |
| `rm -i` | Ask before deleting | `rm -i file.txt` |
| `rmdir` | Remove an empty directory | `rmdir documents` |
| `rm -r` | Remove a directory and its contents | `rm -r documents` |

### Examples

```bash
rm file.txt
rmdir empty-folder
rm -r backup
```

**Warning:** Be careful when using `rm -r` and especially `rm -rf`. Deleted files may not be recoverable.

## 6. View File Contents

| Command | Use | Example |
|---------|-----|---------|
| `cat` | Display file contents | `cat file.txt` |
| `less` | View a file page by page | `less file.txt` |
| `head` | Display first 10 lines | `head file.txt` |
| `tail` | Display last 10 lines | `tail file.txt` |
| `tail -f` | Monitor a file continuously | `tail -f /var/log/syslog` |

### Examples

```bash
cat /etc/hostname
less /var/log/syslog
head -n 5 file.txt
tail -n 20 file.txt
tail -f /var/log/syslog
```

Press `q` to exit `less` or stop following a file with `tail -f`.

## 7. Search for Files

| Command | Use | Example |
|---------|-----|---------|
| `find` | Search for files and directories | `find /home -name file.txt` |
| `locate` | Find files using a database | `locate file.txt` |
| `which` | Find a command's executable path | `which ls` |

### Examples

```bash
find /var/log -name "*.log"
find /home -type f
find /tmp -type d
```

## 8. File Permissions

| Command | Use | Example |
|---------|-----|---------|
| `chmod` | Change file permissions | `chmod 644 file.txt` |
| `chown` | Change file owner | `chown user:user file.txt` |
| `stat` | Display file details | `stat file.txt` |

### Examples

```bash
chmod 755 script.sh
chmod 644 file.txt
chown www-data:www-data file.txt
stat file.txt
```

## 9. Disk Usage and File Size

| Command | Use | Example |
|---------|-----|---------|
| `du -sh` | Display directory size | `du -sh /var/log` |
| `df -h` | Display filesystem disk usage | `df -h` |
| `ls -lh` | Display file sizes | `ls -lh` |

### Examples

```bash
du -sh /var/log
du -sh /home/*
df -h
```

## 10. Practical File Management Practice

```bash
# Create a practice directory
mkdir linux-practice

# Enter the directory
cd linux-practice

# Create files
touch file1.txt file2.txt

# Create a subdirectory
mkdir backup

# Copy a file
cp file1.txt backup/

# Rename a file
mv file2.txt file3.txt

# List files
ls -la

# Display file contents
cat file1.txt

# Remove a file
rm file3.txt

# Remove the copied file
rm backup/file1.txt

# Remove the empty directory
rmdir backup

# Return to the parent directory
cd ..

# Remove the practice directory
rmdir linux-practice
```

