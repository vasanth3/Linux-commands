
# Linux Disk Management

Linux disk management is used to manage storage devices, partitions, filesystems, and mount points.

System administrators use disk management commands to check disk space, create partitions, format disks, mount storage, and expand filesystems.

## 1. Disk Information Commands

| Command | Description |
|---------|-------------|
| `lsblk` | Display block devices |
| `lsblk -f` | Display filesystems and UUIDs |
| `fdisk -l` | List disk partitions |
| `blkid` | Display filesystem UUIDs and types |
| `parted -l` | Display partition information |
| `df -h` | Display filesystem disk usage |
| `du -sh` | Display directory size |

### Examples

```bash
# Display all disks and partitions
lsblk

# Display filesystem information
lsblk -f

# List disk partitions
sudo fdisk -l

# Display UUIDs
sudo blkid

# Check filesystem disk usage
df -h

# Check directory size
du -sh /var/log
```

## 2. Check Disk Usage

The `df` command displays available and used disk space.

| Command | Description |
|---------|-------------|
| `df -h` | Display disk usage in human-readable format |
| `df -Th` | Display filesystem type and disk usage |
| `df -i` | Display inode usage |
| `du -sh /path` | Display directory size |
| `du -ah` | Display sizes of files and directories |

### Examples

```bash
# Check disk usage
df -h

# Check filesystem types
df -Th

# Check inode usage
df -i

# Check /var directory size
du -sh /var

# Check sizes of directories
du -sh /var/*
```

## 3. Partition Management

Linux uses partitions to divide a disk into separate storage areas.

Common partitioning tools:

| Command | Description |
|---------|-------------|
| `fdisk` | Manage disk partitions |
| `parted` | Manage partition tables |
| `gdisk` | Manage GPT partitions |

### Display Partitions

```bash
sudo fdisk -l
```

### Open fdisk

```bash
sudo fdisk /dev/sdb
```

Common commands inside fdisk:

| Command | Description |
|---------|-------------|
| `m` | Display help |
| `p` | Print partition table |
| `n` | Create a new partition |
| `d` | Delete a partition |
| `w` | Write changes and exit |
| `q` | Quit without saving |

**Warning:** Partition changes can destroy data. Always verify the target disk before modifying partitions.

## 4. Create a Filesystem

The `mkfs` command creates a filesystem on a partition.

| Command | Description |
|---------|-------------|
| `mkfs.ext4` | Create an ext4 filesystem |
| `mkfs.xfs` | Create an XFS filesystem |
| `mkfs.vfat` | Create a FAT filesystem |

### Examples

```bash
# Create an ext4 filesystem
sudo mkfs.ext4 /dev/sdb1

# Create an XFS filesystem
sudo mkfs.xfs /dev/sdb1

# Display filesystem information
lsblk -f
```

**Warning:** Formatting a partition destroys its existing filesystem data. Do not run these commands on a disk containing important data.

## 5. Mount and Unmount Disks

The `mount` command attaches a filesystem to a directory.

The `umount` command detaches a mounted filesystem.

### Examples

```bash
# Create a mount point
sudo mkdir -p /mnt/data

# Mount a partition
sudo mount /dev/sdb1 /mnt/data

# Verify the mount
df -h

# Display mounted filesystems
findmnt

# Unmount the filesystem
sudo umount /mnt/data
```

### Mount by UUID

Using UUIDs is generally more reliable than using device names.

```bash
# Display UUID
sudo blkid /dev/sdb1

# Mount using UUID
sudo mount UUID=<filesystem-uuid> /mnt/data
```

Replace `<filesystem-uuid>` with the actual UUID.

## 6. Permanent Mounting Using /etc/fstab

The `/etc/fstab` file defines filesystems that should be mounted automatically.

### Steps

```bash
# Display filesystem UUID
sudo blkid /dev/sdb1

# Create mount point
sudo mkdir -p /mnt/data

# Edit fstab
sudo nano /etc/fstab
```

Example entry for an ext4 filesystem:

```text
UUID=<filesystem-uuid> /mnt/data ext4 defaults 0 2
```

Verify the configuration:

```bash
# Reload systemd configuration
sudo systemctl daemon-reload

# Test fstab entries
sudo mount -a

# Verify mount
df -h
```

**Important:** Incorrect `/etc/fstab` entries can cause boot problems. Back up the file before editing.

## 7. Swap Management

Swap provides additional virtual memory when physical RAM is under pressure.

| Command | Description |
|---------|-------------|
| `free -h` | Display memory and swap usage |
| `swapon --show` | Display active swap |
| `swapoff` | Disable swap |
| `mkswap` | Create swap area |

### Examples

```bash
# Check memory and swap
free -h

# Display active swap
swapon --show

# Create a 2 GB swap file
sudo fallocate -l 2G /swapfile

# Set permissions
sudo chmod 600 /swapfile

# Format as swap
sudo mkswap /swapfile

# Enable swap
sudo swapon /swapfile

# Verify
swapon --show
```

To disable the swap file:

```bash
sudo swapoff /swapfile
```

## 8. Logical Volume Management (LVM)

LVM allows administrators to manage storage flexibly by combining physical storage into logical volumes.

LVM components:

- PV: Physical Volume
- VG: Volume Group
- LV: Logical Volume

### Display LVM Information

```bash
# Display physical volumes
sudo pvs

# Display volume groups
sudo vgs

# Display logical volumes
sudo lvs

# Detailed logical volume information
sudo lvdisplay
```

### Basic LVM Example

The following commands are examples for a new, unused disk.

```bash
# Create a physical volume
sudo pvcreate /dev/sdb1

# Create a volume group
sudo vgcreate data-vg /dev/sdb1

# Create a 10 GB logical volume
sudo lvcreate -L 10G -n data-lv data-vg

# Create an ext4 filesystem
sudo mkfs.ext4 /dev/data-vg/data-lv

# Create mount point
sudo mkdir -p /mnt/data

# Mount logical volume
sudo mount /dev/data-vg/data-lv /mnt/data

# Verify
df -h
```

**Warning:** LVM initialization and filesystem creation can overwrite existing data. Verify the device and its contents before proceeding.

## 9. Extend a Logical Volume

LVM can extend a logical volume when additional free space is available in its volume group.

### Example

```bash
# Check free space in volume group
sudo vgs

# Extend logical volume by 5 GB
sudo lvextend -L +5G /dev/data-vg/data-lv

# Extend ext4 filesystem
sudo resize2fs /dev/data-vg/data-lv

# Verify
df -h
```

For XFS:

```bash
sudo xfs_growfs /mnt/data
```

XFS grows while mounted. The mount point must be the actual mounted XFS filesystem.

## 10. Disk Troubleshooting Commands

| Problem | Command |
|---------|---------|
| Check disk space | `df -h` |
| Check inode usage | `df -i` |
| Identify disks | `lsblk` |
| Check filesystem type | `lsblk -f` |
| Find large directories | `du -sh /var/*` |
| Check mount points | `findmnt` |
| Check disk errors in logs | `dmesg -T \| grep -i error` |
| Check disk I/O | `iostat -xz 1` |

Install `iostat` on Ubuntu:

```bash
sudo apt update
sudo apt install sysstat
```

Run:

```bash
iostat -xz 1
```

## 11. Practical Disk Management Practice

Use a test disk or a virtual disk in a lab environment.

```bash
# Identify disks
lsblk

# Check disk usage
df -h

# Display filesystem UUIDs
lsblk -f

# Create a mount point
sudo mkdir -p /mnt/practice

# Mount an existing test partition
sudo mount /dev/sdb1 /mnt/practice

# Verify
df -h /mnt/practice

# Unmount
sudo umount /mnt/practice
```

## 12. Production System Administrator Tips

- Always verify disk names using `lsblk` before modifying disks.
- Check disk usage using `df -h`.
- Check inode usage using `df -i`.
- Back up important data before partitioning or formatting.
- Use UUIDs in `/etc/fstab` for persistent mounts.
- Check filesystem type before resizing.
- Never format a mounted filesystem.
- Verify mount points after storage changes.
- Use LVM when flexible storage expansion is required.
- Monitor disk space regularly on production servers.

## Summary

| Command | Purpose |
|---------|---------|
| `lsblk` | Display block devices |
| `fdisk` | Manage partitions |
| `mkfs` | Create filesystems |
| `mount` | Mount filesystems |
| `umount` | Unmount filesystems |
| `df` | Check disk usage |
| `du` | Check directory size |
| `blkid` | Display filesystem UUIDs |
| `findmnt` | Display mount points |
| `pvs` | Display physical volumes |
| `vgs` | Display volume groups |
| `lvs` | Display logical volumes |
| `resize2fs` | Resize ext2/ext3/ext4 filesystems |
| `xfs_growfs` | Grow XFS filesystems |
| `swapon` | Enable swap |
| `swapoff` | Disable swap |
