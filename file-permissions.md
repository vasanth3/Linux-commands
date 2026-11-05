
# Linux File Permissions

Linux file permissions control who can read, write, and execute files and directories.

Linux has three permission categories:

| Category | Symbol | Description |
|----------|--------|-------------|
| User | `u` | File owner |
| Group | `g` | Group owner |
| Others | `o` | All other users |
| All | `a` | User, group, and others |

## 1. Understanding Linux Permissions

Linux uses three basic permissions:

| Permission | Symbol | Numeric Value | Description |
|------------|--------|---------------|-------------|
| Read | `r` | 4 | Read file contents |
| Write | `w` | 2 | Modify file contents |
| Execute | `x` | 1 | Execute a file or access a directory |

### Example

```bash
ls -l file.txt
```

Output:

```text
-rwxr-xr-- 1 root root 100 Nov 5 10:00 file.txt
```

Explanation:

- `-` : Regular file
- `rwx` : Owner has read, write, execute
- `r-x` : Group has read and execute
- `r--` : Others have read-only access
- `root` : File owner
- `root` : Group owner

Permission value:

```text
Owner  = rwx = 7
Group  = r-x = 5
Others = r-- = 4

Permission = 754
```

## 2. chmod - Change File Permissions

The `chmod` command changes file and directory permissions.

### Syntax

```bash
chmod [permissions] filename
```

### Numeric Method

| Command | Description |
|---------|-------------|
| `chmod 755 file.txt` | Owner has full permissions; group and others can read and execute |
| `chmod 644 file.txt` | Owner can read/write; group and others can read |
| `chmod 600 file.txt` | Only owner can read and write |
| `chmod 700 script.sh` | Only owner has full permissions |
| `chmod 777 file.txt` | Everyone has full permissions |

### Examples

```bash
# Give owner full permissions
chmod 700 script.sh

# Give read and write to owner
chmod 600 file.txt

# Give read permission to everyone
chmod 644 file.txt

# Make a script executable
chmod +x script.sh

# Remove write permission from others
chmod o-w file.txt
```

### Symbolic Method

| Command | Description |
|---------|-------------|
| `chmod u+x file.txt` | Add execute permission to owner |
| `chmod g+w file.txt` | Add write permission to group |
| `chmod o-r file.txt` | Remove read permission from others |
| `chmod a+x file.txt` | Add execute permission to everyone |
| `chmod u=rwx,g=rx,o=r file.txt` | Set specific permissions |

## 3. chown - Change File Owner

The `chown` command changes the owner of a file or directory.

### Syntax

```bash
chown [owner] filename
```

### Examples

```bash
# Change owner to user1
sudo chown user1 file.txt

# Change owner and group
sudo chown user1:developers file.txt

# Change ownership recursively
sudo chown -R user1:developers /var/www/html

# Verify ownership
ls -l file.txt
```

### Practical Example

For a website hosted using Apache:

```bash
sudo chown -R www-data:www-data /var/www/html
```

This changes the owner and group of the directory to `www-data`.

**Note:** Use recursive ownership changes carefully, especially on production servers.

## 4. chgrp - Change Group Ownership

The `chgrp` command changes the group ownership of a file or directory.

### Syntax

```bash
chgrp [group] filename
```

### Examples

```bash
# Change group ownership
sudo chgrp developers file.txt

# Change group ownership recursively
sudo chgrp -R developers /var/www/html

# Verify group ownership
ls -l file.txt
```

### Practical Example

```bash
sudo chgrp www-data /var/www/html/index.html
```

This changes the group ownership of `index.html` to `www-data`.

## 5. umask - Default File Permissions

The `umask` command controls the default permissions assigned to newly created files and directories.

### Check Current umask

```bash
umask
```

Example output:

```text
0022
```

### Understanding umask

Linux starts with these base permissions:

| Type | Base Permission |
|------|-----------------|
| Files | 666 |
| Directories | 777 |

The umask removes permissions from these base values.

For example:

```text
Base file permission       = 666
Umask                     = 022
Default file permission   = 644

Base directory permission = 777
Umask                     = 022
Default directory permission = 755
```

### Common umask Values

| umask | File Permission | Directory Permission |
|-------|-----------------|----------------------|
| `000` | 666 | 777 |
| `002` | 664 | 775 |
| `022` | 644 | 755 |
| `027` | 640 | 750 |
| `077` | 600 | 700 |

### Examples

```bash
# Display current umask
umask

# Set umask to 022
umask 022

# Set umask to 027
umask 027

# Set restrictive umask
umask 077
```

### Test umask

```bash
# Set umask
umask 022

# Create a file
touch test.txt

# Create a directory
mkdir testdir

# Check permissions
ls -l test.txt
ls -ld testdir
```

Expected permissions:

```text
-rw-r--r-- test.txt
drwxr-xr-x testdir
```

**Note:** The umask setting normally applies to the current shell and its child processes. It does not automatically change permissions on existing files.

## 6. Practical File Permission Exercises

### Exercise 1: Create a File

```bash
touch practice.txt
ls -l practice.txt
```

### Exercise 2: Change Permissions

```bash
chmod 644 practice.txt
ls -l practice.txt
```

### Exercise 3: Make a Script Executable

```bash
touch script.sh
chmod 755 script.sh
ls -l script.sh
```

### Exercise 4: Change Ownership

```bash
sudo chown root:root practice.txt
ls -l practice.txt
```

### Exercise 5: Change Group

```bash
sudo chgrp adm practice.txt
ls -l practice.txt
```

### Exercise 6: Test umask

```bash
umask 027
touch secure.txt
mkdir secure-dir

ls -l secure.txt
ls -ld secure-dir
```

Expected permissions:

```text
-rw-r----- secure.txt
drwxr-x--- secure-dir
```

## 7. Important Production Tips

- Avoid using `chmod 777` on production files.
- Use `chmod 600` for sensitive files such as private keys.
- Use `chmod 700` for scripts that should only be accessible by the owner.
- Use `chown` carefully on web server directories.
- Use `umask 027` or `umask 077` when more restrictive default permissions are needed.
- Verify permissions using `ls -l` and `stat`.

## Summary

| Command | Purpose |
|---------|---------|
| `chmod` | Change file permissions |
| `chown` | Change file owner |
| `chgrp` | Change group ownership |
| `umask` | Control default permissions |
| `ls -l` | View permissions and ownership |
| `stat` | Display detailed file information |
