<img width="1536" height="1024" alt="e17bcf3a-c18e-4fe8-8b10-5729ce2403ff" src="https://github.com/user-attachments/assets/7464adb4-320b-4d29-a0cd-6a452c9e434a" />


# 💾 Disk and Storage Management in Linux ๋࣭ ⭑✮💻₊ ⊹

## 1. 📌 Introduction to Disk and Storage Management

Managing disks and storage efficiently is crucial for **system performance, reliability, scalability, and stability**.

Linux provides various commands and utilities to:

* Monitor disk usage
* Identify disks and partitions
* Create and manage partitions
* Format storage devices
* Mount and unmount filesystems
* Manage Logical Volumes using LVM
* Configure swap space
* Troubleshoot storage-related issues

Disk and storage management is an essential skill for **Linux Administrators, DevOps Engineers, Cloud Engineers, and SREs**.

---

# 2. 📚 Index of Commands Covered (﹙˓ 📟 ˒﹚)

### 2.1 Viewing Disk Information

| Command        | Purpose                  |
| -------------- | ------------------------ |
| `lsblk`        | Display block devices    |
| `fdisk -l`     | List disk partitions     |
| `blkid`        | Show UUIDs of devices    |
| `df -h`        | Check disk space usage   |
| `du -sh /path` | Show size of a directory |

### 2.2 Partition Management

| Command               | Purpose                            |
| --------------------- | ---------------------------------- |
| `fdisk /dev/sdX`      | Create and manage partitions       |
| `parted /dev/sdX`     | Alternative to fdisk for GPT disks |
| `mkfs.ext4 /dev/sdX1` | Format a partition as ext4         |
| `mkfs.xfs /dev/sdX1`  | Format a partition as XFS          |

### 2.3 Mounting and Unmounting

| Command                    | Purpose                           |
| -------------------------- | --------------------------------- |
| `mount /dev/sdX1 /mnt`     | Mount a partition                 |
| `umount /mnt`              | Unmount a partition               |
| `mount -o remount,rw /mnt` | Remount a partition as read-write |

### 2.4 Logical Volume Management (LVM)

| Command                              | Purpose                  |
| ------------------------------------ | ------------------------ |
| `pvcreate /dev/sdX`                  | Create a physical volume |
| `vgcreate vg_name /dev/sdX`          | Create a volume group    |
| `lvcreate -L 10G -n lv_name vg_name` | Create a logical volume  |
| `mkfs.ext4 /dev/vg_name/lv_name`     | Format an LVM partition  |
| `mount /dev/vg_name/lv_name /mnt`    | Mount an LVM partition   |

### 2.5 Swap Management

| Command            | Purpose                 |
| ------------------ | ----------------------- |
| `mkswap /dev/sdX`  | Create a swap partition |
| `swapon /dev/sdX`  | Enable swap space       |
| `swapoff /dev/sdX` | Disable swap space      |

---

# 3. 🔍 Viewing Disk Information

## 3.1 Using `lsblk`

List all block devices:

```bash
lsblk
```

`lsblk` is one of the most commonly used commands for quickly understanding the disk and partition structure of a Linux system.

---

## 3.2 Using `fdisk`

View partition details:

```bash
fdisk -l
```

This displays information such as:

* Disk size
* Partition table
* Partition sizes
* Partition types
* Sector information

---

## 3.3 Using `blkid`

Display filesystem UUIDs and types:

```bash
blkid
```

Example:

```text
/dev/sda1: UUID="abc123..." TYPE="ext4"
/dev/sdb1: UUID="xyz789..." TYPE="xfs"
```

UUIDs are commonly used when configuring persistent mounts in `/etc/fstab`.

---

## 3.4 Using `df`

Check available disk space:

```bash
df -h
```

Example:

```text
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        96G   42G   50G  46% /
```

The `-h` option displays sizes in a human-readable format.

---

## 3.5 Using `du`

Find the size of a directory:

```bash
du -sh /var/log
```

This is especially useful for troubleshooting situations where disk usage is unexpectedly high.

---

# 4. 🧩 Partition Management

## 4.1 Creating a Partition with `fdisk`

```bash
fdisk /dev/sdX
```

Follow the interactive prompts to create a partition.

Inside `fdisk`:

```text
n → Create a new partition
p → Display partition table
d → Delete a partition
w → Write changes
q → Quit without saving
```

> ⚠️ **Important:** Always verify the target disk before modifying partitions. Selecting the wrong disk can result in data loss.

---

## 4.2 Using `parted`

`parted` can be used as an alternative partitioning tool and is particularly useful when working with **GPT partition tables and large disks**.

```bash
parted /dev/sdX
```

---

# 5. 🗂️ Formatting a Partition

After creating a partition, it generally needs to be formatted with a filesystem before it can be used.

## 5.1 Format as EXT4

```bash
mkfs.ext4 /dev/sdX1
```

## 5.2 Format as XFS

```bash
mkfs.xfs /dev/sdX1
```

### Common Filesystems

| Filesystem | Common Usage                             |
| ---------- | ---------------------------------------- |
| EXT4       | General-purpose Linux systems            |
| XFS        | Enterprise servers and large filesystems |
| Btrfs      | Advanced Linux storage features          |

---

# 6. 📁 Mounting and Unmounting

## 6.1 Mount a Partition

```bash
mount /dev/sdX1 /mnt
```

The partition becomes accessible through `/mnt`.

---

## 6.2 Unmount a Partition

```bash
umount /mnt
```

Before unmounting, make sure applications are not actively using the filesystem.

---

## 6.3 Remount a Partition

```bash
mount -o remount,rw /mnt
```

This remounts the filesystem with read-write access.

---

# 7. 🧱 Logical Volume Management — LVM

LVM provides a flexible way to manage storage by introducing an abstraction layer between physical disks and filesystems.

### LVM Architecture

```text
Physical Disk
     ↓
Physical Volume (PV)
     ↓
Volume Group (VG)
     ↓
Logical Volume (LV)
     ↓
Filesystem
     ↓
Mount Point
```

---

## 7.1 Create a Physical Volume

```bash
pvcreate /dev/sdX
```

---

## 7.2 Create a Volume Group

```bash
vgcreate vg_name /dev/sdX
```

---

## 7.3 Create a Logical Volume

```bash
lvcreate -L 10G -n lv_name vg_name
```

This creates a **10 GB logical volume**.

---

## 7.4 Format the Logical Volume

```bash
mkfs.ext4 /dev/vg_name/lv_name
```

---

## 7.5 Mount the Logical Volume

```bash
mount /dev/vg_name/lv_name /mnt
```

---

# 8. 🔄 Swap Management

Swap provides disk space that Linux can use when RAM becomes heavily utilized.

## 8.1 Create a Swap Partition

```bash
mkswap /dev/sdX
```

---

## 8.2 Enable Swap

```bash
swapon /dev/sdX
```

Verify:

```bash
swapon --show
```

---

## 8.3 Disable Swap

```bash
swapoff /dev/sdX
```

---

# 9. 🛠️ Real-World Company Use Cases

## 9.1 E-Commerce Application — Expanding Application Storage

Imagine an e-commerce company running its application on a Linux server.

The application stores:

* Product images
* Application logs
* Reports
* Temporary files

The existing disk starts running out of space.

A DevOps Engineer can:

```bash
lsblk
```

Identify the newly attached disk:

```text
sda   100G
sdb    50G
```

Then:

```bash
sudo fdisk /dev/sdb
sudo mkfs.ext4 /dev/sdb1
sudo mkdir /data
sudo mount /dev/sdb1 /data
```

The application can now use `/data` for additional storage.

### Practical Benefit

This approach allows teams to **expand storage without replacing the existing server**.

---

## 9.2 Enterprise Application — LVM for Flexible Storage

A large enterprise application may use LVM to manage application data.

For example:

```text
/dev/sdb
   ↓
Physical Volume
   ↓
vg_app
   ↓
lv_database
   ↓
XFS/EXT4
   ↓
/data
```

Commands:

```bash
sudo pvcreate /dev/sdb
sudo vgcreate vg_app /dev/sdb
sudo lvcreate -L 50G -n lv_database vg_app
sudo mkfs.xfs /dev/vg_app/lv_database
sudo mkdir /data
sudo mount /dev/vg_app/lv_database /data
```

### Practical Benefit

LVM provides flexibility for **extending logical volumes and managing storage without directly managing individual partitions**.

---

## 9.3 Production Server — Troubleshooting Disk Full Alerts

A monitoring system may generate an alert:

```text
Disk Usage: 92%
```

The engineer can investigate using:

```bash
df -h
```

Then identify large directories:

```bash
du -sh /var/log
du -sh /var/*
```

For example:

```text
/var/log      18G
/var/lib      25G
/var/cache     8G
```

The engineer can then investigate the responsible application, logs, or data before taking corrective action.

### Practical Benefit

Regular disk monitoring helps prevent:

* Application failures
* Database issues
* Log-writing failures
* Service interruptions
* Unexpected production incidents

---

# 10. 🔎 Additional Notes — When to Use `fdisk`, `mount`, or Both

## 10.1 Check Available Disks

Before creating or mounting anything, always check what block devices exist:

```bash
lsblk
```

Example output:

```text
NAME   MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda      8:0    0  100G  0 disk
├─sda1   8:1    0   96G  0 part /
└─sda2   8:2    0    4G  0 part [SWAP]
sdb      8:16   0   20G  0 disk
```

Here:

```text
sda → Existing disk (already partitioned)

sdb → New disk, no partitions yet
```

---

# 11. 🧰 When to Use `fdisk`

Use `fdisk` when:

* The disk is brand new and has no partitions
* You want to create `/dev/sdb1`, `/dev/sdb2`, etc.
* You need to modify a partition table

Inside `fdisk`:

```text
n → Create a new partition
w → Write changes
```

Then confirm:

```bash
lsblk
```

---

# 12. 📂 When to Use `mount`

Use `mount` when:

* The partition already exists
* The partition is already formatted
* You just want to make it accessible

Example:

```bash
sudo mkdir /mnt/mydisk
sudo mount /dev/sdb1 /mnt/mydisk
```

Now your disk is available at:

```text
/mnt/mydisk
```

---

# 13. 🔧 When to Use `fdisk + mkfs + mount`

Use `fdisk + mkfs + mount` when:

* The disk is completely new
* You need to partition it
* You need to format the partition
* You need to mount it

### Full Setup

```bash
# 1. Check available disks
lsblk

# 2. Create partition
sudo fdisk /dev/sdb

# 3. Format the partition
sudo mkfs.ext4 /dev/sdb1

# 4. Create mount point
sudo mkdir /data

# 5. Mount the partition
sudo mount /dev/sdb1 /data
```

Verify:

```bash
df -h
```

---

# 14. 📊 Quick Reference

| Use Case                    | Command(s)             |
| --------------------------- | ---------------------- |
| View disks and partitions   | `lsblk`                |
| View partition details      | `fdisk -l`             |
| View device UUIDs           | `blkid`                |
| Check disk space            | `df -h`                |
| Check directory size        | `du -sh /path`         |
| Partition a new disk        | `fdisk`                |
| GPT partition management    | `parted`               |
| Format as EXT4              | `mkfs.ext4`            |
| Format as XFS               | `mkfs.xfs`             |
| Mount an existing partition | `mount`                |
| Unmount a partition         | `umount`               |
| Create LVM physical volume  | `pvcreate`             |
| Create LVM volume group     | `vgcreate`             |
| Create logical volume       | `lvcreate`             |
| Create swap                 | `mkswap`               |
| Enable swap                 | `swapon`               |
| Disable swap                | `swapoff`              |
| Full setup for new disk     | `fdisk + mkfs + mount` |

---

# 15. ♾️👨🏼‍💻DevOps & Cloud Importance☁️🌐

Disk and storage management is frequently required in real-world **DevOps and Cloud environments**.

A DevOps Engineer should understand how to:

* Identify attached disks
* Monitor filesystem utilization
* Create and manage partitions
* Format filesystems
* Mount storage
* Work with LVM
* Manage swap
* Investigate disk-full incidents
* Understand persistent storage
* Troubleshoot storage-related production issues

### ☁️ Cloud Connection

The same concepts apply to cloud platforms such as:

* AWS EC2 + EBS
* Azure Virtual Machines + Managed Disks
* Google Cloud Compute Engine + Persistent Disk

For example, when a new cloud disk is attached to a Linux VM, the typical workflow is:

```text
Cloud Disk
    ↓
Linux Block Device
    ↓
Partition (Optional)
    ↓
Filesystem
    ↓
Mount Point
    ↓
Application
```

---

# 16. 🎯 Key Takeaways

```text
lsblk
  ↓
Identify the disk
  ↓
fdisk / parted
  ↓
Create partition
  ↓
 mkfs
  ↓
Create filesystem
  ↓
mount
  ↓
Make storage accessible
```

### 👉Remember🤔

> **`fdisk` → Partition**

> **`mkfs` → Format**

> **`mount` → Access**

> **`df` → Filesystem usage**

> **`du` → Directory usage**

> **`LVM` → Flexible storage management**

😊Understanding these fundamentals provides a strong foundation for **Linux Administration, DevOps, Cloud Engineering, SRE, and Production Troubleshooting**.😊
