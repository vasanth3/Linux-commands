
# Linux User Management

Linux user management is the process of creating, modifying, deleting, and managing user accounts and groups.

These commands are commonly used by Linux system administrators to manage server access,
Always login as root user to execute commands.

## 1. User Management Commands

| Command | Description |
|---------|-------------|
| `whoami` | Display the current username |
| `who` | Show logged-in users |
| `w` | Display logged-in users and their activities |
| `id` | Display user ID and group ID |
| `users` | Display currently logged-in usernames |
| `useradd` | Create a new user |
| `adduser` | Create a user interactively (Ubuntu/Debian) |
| `usermod` | Modify an existing user |
| `userdel` | Delete a user |
| `passwd` | Set or change a user's password |
| `su` | Switch to another user |
| `sudo` | Execute commands with elevated privileges |

## 2. Create a User

### useradd

The `useradd` command creates a new user account.

Syntax:

```bash
useradd username
```

Examples:

```bash
# Create a user
useradd john

# Create a user with a home directory
useradd -m john

# Create a user with a specific shell
useradd -m -s /bin/bash john

# Set the user's password
passwd john
```

### adduser

On Ubuntu and Debian, `adduser` provides an interactive way to create users.

```bash
 adduser john
```

This command creates the user, prompts for a password, and normally creates a home directory.

## 3. Modify a User

The `usermod` command modifies an existing user account.

| Command | Description |
|---------|-------------|
| `usermod -l` | Change username |
| `usermod -d` | Change home directory |
| `usermod -s` | Change login shell |
| `usermod -aG` | Add user to secondary group |
| `usermod -L` | Lock user account |
| `usermod -U` | Unlock user account |

### Examples

```bash
# Change username
 usermod -l newname oldname

# Add user to sudo group on Ubuntu
 usermod -aG sudo john

# Add user to a group
usermod -aG developers john

# Change login shell
usermod -s /bin/bash john

# Lock user account
 usermod -L john

# Unlock user account
usermod -U john
```

**Important:** Always use `-aG` when adding a user to a supplementary group. Using `-G` without `-a` can replace the user's existing supplementary group memberships.

## 4. Delete a User

The `userdel` command deletes a user account.

```bash
# Delete user account
 userdel john

# Delete user and home directory
 userdel -r john
```

The `-r` option removes the user's home directory and mail spool.

**Warning:** Check for important files before deleting a user.

## 5. Password Management

The `passwd` command manages user passwords.

```bash
# Change your own password
passwd

# Change another user's password
passwd john

# Lock a user password
 passwd -l john

# Unlock a user password
passwd -u john

# Check password expiration information
 chage -l john
```

### chage - Password Expiration

The `chage` command manages password aging and expiration.

```bash
# View password aging information
chage -l john

# Set password to expire after 90 days
chage -M 90 john

# Require password change at next login
chage -d 0 john
```

## 6. Group Management

Linux groups allow administrators to manage access for multiple users.

| Command | Description |
|---------|-------------|
| `groupadd` | Create a group |
| `groupmod` | Modify a group |
| `groupdel` | Delete a group |
| `groups` | Display user's groups |
| `getent group` | Display group information |

### Examples

```bash
# Create a group
 groupadd developers

# Create another group
groupadd admins

# Add user to a group
usermod -aG developers john

# Display user's groups
groups john

# Display group information
getent group developers

# Rename a group
groupmod -n devteam developers

# Delete a group
groupdel devteam
``

## 7. Switch Between Users

### su - Switch User

```bash
# Switch to another user
su john

# Switch to another user with login environment
su - john

# Switch to root
su -
```

### sudo - Execute Commands as Root

```bash
# Run a command with elevated privileges
ls /root

# Open a root shell
sudo -i
```

Use `sudo` instead of logging in directly as root whenever possible.

## 8. Important User Files

| File | Description |
|------|-------------|
| `/etc/passwd` | User account information |
| `/etc/shadow` | Encrypted password hashes and password aging |
| `/etc/group` | Group information |
| `/etc/gshadow` | Secure group information |
| `/etc/skel` | Default files copied into new home directories |

### View User Information

```bash
# Display user account details
cat /etc/passwd

# Display current user's information
id

# Display a specific user
id john

# View group information
cat /etc/group
```

**Security Note:** Avoid sharing the contents of `/etc/shadow`. It contains sensitive password hash information.

## 9. Practical User Management Example

Create a user and assign the user to a group.

```bash
# Create a group
 groupadd developers

# Create a user with a home directory
useradd -m -s /bin/bash john

# Set password
 passwd john

# Add user to developers group
 usermod -aG developers john

# Verify user details
id john

# Verify group membership
groups john

# Switch to the user
su - john

# Return to previous user
exit
```

## 10. Production System Administrator Tips

- Use strong passwords and SSH keys.
- Grant sudo access only to trusted users.
- Follow the principle of least privilege.
- Review user accounts regularly.
- Remove or disable unused accounts.
- Verify group memberships before granting access.
- Avoid sharing root credentials.
- Check `/etc/passwd` and `/etc/group` when troubleshooting account issues.

## Summary

| Command | Purpose |
|---------|---------|
| `useradd` | Create user |
| `usermod` | Modify user |
| `userdel` | Delete user |
| `passwd` | Manage passwords |
| `groupadd` | Create group |
| `groupmod` | Modify group |
| `groupdel` | Delete group |
| `groups` | Display group membership |
| `id` | Display UID and GID |
| `chage` | Manage password expiration |
| `sudo` | Execute commands with elevated privileges |
