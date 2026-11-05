
# Package Management

Package management is used to install, update, remove, and manage software packages in Linux.

Linux distributions use different package managers:

| Linux Distribution | Package Manager | Package Format |
|--------------------|-----------------|----------------|
| Ubuntu             | APT             | .deb           |
| Debian             | APT             | .deb           |
| AlmaLinux          | DNF             | .rpm           |
| Rocky Linux        | DNF             | .rpm           |
| RHEL               | DNF             | .rpm           |

---

## 1. APT Package Management (Ubuntu/Debian)

### Update Package Lists

Downloads the latest package information from configured repositories.

```bash
sudo apt update
```

### Upgrade Installed Packages

Installs available updates for installed packages.

```bash
sudo apt upgrade
```

### Install a Package

Example: Install Apache.

```bash
sudo apt install apache2
```

### Remove a Package

Removes a package but may leave its configuration files.

```bash
sudo apt remove apache2
```

### Completely Remove a Package

Removes the package and its system configuration files.

```bash
sudo apt purge apache2
```

### Search for a Package

```bash
apt search nginx
```

### Show Package Information

```bash
apt show nginx
```

### List Installed Packages

```bash
apt list --installed
```

### Remove Unused Packages

```bash
sudo apt autoremove
```

### Clean Package Cache

```bash
sudo apt clean
```

---

## 2. DNF Package Management (RHEL/AlmaLinux/Rocky Linux)

### Check for Updates

```bash
sudo dnf check-update
```

### Update Installed Packages

```bash
sudo dnf upgrade
```

### Install a Package

Example: Install Apache.

```bash
sudo dnf install httpd
```

### Remove a Package

```bash
sudo dnf remove httpd
```

### Search for a Package

```bash
dnf search nginx
```

### Show Package Information

```bash
dnf info nginx
```

### List Installed Packages

```bash
dnf list installed
```

### List Available Updates

```bash
dnf list updates
```

### Clean Package Cache

```bash
sudo dnf clean all
```

---

## 3. RPM Package Management

RPM is used to manage individual `.rpm` package files.

### Install an RPM Package

```bash
sudo rpm -ivh package.rpm
```

### Upgrade an RPM Package

```bash
sudo rpm -Uvh package.rpm
```

### Remove an RPM Package

```bash
sudo rpm -e package_name
```

### Check Whether a Package Is Installed

```bash
rpm -q package_name
```

### List Installed RPM Packages

```bash
rpm -qa
```

### Find Which Package Owns a File

```bash
rpm -qf /usr/bin/ls
```

---

## 4. Debian Package Management

### Install a Local DEB Package

```bash
sudo dpkg -i package.deb
```

### Fix Missing Dependencies

```bash
sudo apt --fix-broken install
```

### Remove a DEB Package

```bash
sudo dpkg -r package_name
```

### Check Whether a Package Is Installed

```bash
dpkg -l package_name
```

### Find Which Package Owns a File

```bash
dpkg -S /usr/bin/ls
```

---

## 5. Useful Package Management Commands

### Check Package Manager Version

```bash
apt --version
dnf --version
rpm --version
dpkg --version
```

### Check Package Installation Status

Ubuntu/Debian:

```bash
dpkg -s nginx
```

RHEL-based systems:

```bash
rpm -q nginx
```

### Reinstall a Package

Ubuntu/Debian:

```bash
sudo apt install --reinstall nginx
```

RHEL-based systems:

```bash
sudo dnf reinstall nginx
```

---

## 6. Practical Examples for System Administrators

### Install Apache Web Server

Ubuntu:

```bash
sudo apt update
sudo apt install apache2
```

AlmaLinux:

```bash
sudo dnf install httpd
```

### Install Network Tools

Ubuntu:

```bash
sudo apt install net-tools
```

AlmaLinux:

```bash
sudo dnf install net-tools
```

### Install Monitoring Tools

Ubuntu:

```bash
sudo apt install htop
```

AlmaLinux:

```bash
sudo dnf install htop
```

### Verify Installed Software

```bash
which htop
htop --version
```

---

## Important Notes

- Use `apt` on Ubuntu and Debian.
- Use `dnf` on AlmaLinux, Rocky Linux, and modern RHEL systems.
- Use `rpm` and `dpkg` to query or manage individual package files.
- Always review package changes before confirming installation or removal.
- Be careful when removing packages from production servers.
- Avoid using `rpm` or `dpkg` to install packages when dependency resolution is needed. Prefer `dnf` or `apt`.

## Summary

| Task | Ubuntu/Debian | RHEL-Based |
|------|---------------|------------|
| Update package lists | `apt update` | `dnf check-update` |
| Upgrade packages | `apt upgrade` | `dnf upgrade` |
| Install package | `apt install` | `dnf install` |
| Remove package | `apt remove` | `dnf remove` |
| Search package | `apt search` | `dnf search` |
| Package information | `apt show` | `dnf info` |
| List installed packages | `apt list --installed` | `dnf list installed` |
