<img width="1536" height="1024" alt="936b93e3-e598-4f0d-a2cf-86fbecc529ee" src="https://github.com/user-attachments/assets/e05e0268-881b-45e8-90ba-bcfdb86449a1" />


# 📁Linux Folder Structure 💻

Understanding the Linux filesystem is essential for **DevOps, Cloud, SRE, System Administration, Docker, Kubernetes, and Production Operations**.

Linux organizes files and directories under a single root directory:

```text
/
├── boot
├── dev
├── etc
├── home
├── lib -> usr/lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin -> usr/sbin
├── srv
├── sys
├── tmp
├── usr
├── var
└── data
```

> **Note:** The exact directory structure can vary between Linux distributions and environments.

---

# 1. Symbolic Links

Modern Linux distributions commonly use symbolic links for compatibility and filesystem organization.

| Directory            | Description                                                  |
| -------------------- | ------------------------------------------------------------ |
| `/bin -> /usr/bin`   | Essential user commands such as `ls`, `cp`, `mv`, and `cat`. |
| `/sbin -> /usr/sbin` | Administrative and system-management commands.               |
| `/lib -> /usr/lib`   | Shared libraries and other system libraries.                 |

### Check symbolic links

```bash
ls -ld /bin /sbin /lib
```

Example:

```text
/bin -> usr/bin
/sbin -> usr/sbin
/lib -> usr/lib
```

### Production relevance

On modern Ubuntu, RHEL-compatible systems, and many cloud images, these links help maintain compatibility while keeping system binaries organized under `/usr`.

---

# 2. `/boot` — Boot Files

The `/boot` directory contains files required to start the Linux operating system.

Typical contents include:

```text
/boot
├── vmlinuz
├── initrd.img
├── grub/
└── config-*
```

### Common use

```bash
ls -lh /boot
```

### Production example

In a production AWS EC2 or Azure VM environment, `/boot` contains the Linux kernel and bootloader-related files required when the server starts.

> **Container note:** Containers normally do not manage the host's boot process, so `/boot` is generally not important inside application containers.

---

# 3. `/usr` — User Applications and Libraries

`/usr` contains a large portion of the operating system's applications, binaries, libraries, and shared resources.

Important subdirectories:

```text
/usr
├── bin
├── sbin
├── lib
├── local
└── share
```

Examples:

```bash
/usr/bin/python3
/usr/bin/git
/usr/bin/docker
/usr/sbin/nginx
```

### Production example

A DevOps engineer may install and manage tools such as:

```text
Git
Python
Docker
Ansible
Nginx
Kubernetes tools
Cloud CLI tools
```

Many of their executables and libraries are stored under `/usr`.

---

# 4. `/etc` — System Configuration

`/etc` contains system-wide configuration files.

This is one of the **most important directories for DevOps and system administrators**.

Examples:

```text
/etc/hosts
/etc/hostname
/etc/fstab
/etc/passwd
/etc/group
/etc/ssh/
/etc/nginx/
/etc/systemd/
```

### Useful commands

```bash
cat /etc/hostname
cat /etc/hosts
cat /etc/fstab
```

### Production example — Nginx

A production web server may have:

```text
/etc/nginx/
├── nginx.conf
├── sites-available/
└── sites-enabled/
```

An administrator might modify:

```bash
sudo vi /etc/nginx/nginx.conf
```

to configure reverse proxying, load balancing, SSL/TLS, or web-server behavior.

### Production example — SSH

SSH configuration is commonly located at:

```text
/etc/ssh/sshd_config
```

For example:

```bash
sudo vi /etc/ssh/sshd_config
```

---

# 5. `/var` — Frequently Changing Data

`/var` contains data that changes during normal system operation.

Important directories include:

```text
/var
├── log
├── cache
├── lib
└── tmp
```

### Logs

```text
/var/log/
```

Examples:

```bash
ls -lh /var/log/
```

Common logs include:

```text
/var/log/syslog
/var/log/auth.log
/var/log/nginx/
```

### Production example — Application Monitoring

A production Nginx server may generate:

```text
/var/log/nginx/access.log
/var/log/nginx/error.log
```

DevOps/SRE teams may collect these logs using centralized logging platforms such as:

```text
ELK / Elastic Stack
Fluent Bit
Loki
CloudWatch
Azure Monitor
```

---

# 6. `/home` — User Home Directories

`/home` contains home directories for normal users.

Example:

```text
/home
├── alice
├── bob
└── devops
```

Check:

```bash
ls -la /home
```

A user's files may be stored under:

```text
/home/devops/
```

### Production example

On a shared Linux server, different engineers may have separate accounts:

```text
/home/devops
/home/developer
/home/jenkins
```

This provides separation between users and their files.

---

# 7. `/root` — Root User's Home

`/root` is the home directory of the Linux `root` user.

Example:

```text
/root
├── .bashrc
├── .ssh/
└── scripts/
```

Access normally requires root privileges.

```bash
sudo ls -la /root
```

### Production example

System administrators may keep administrative scripts or root-specific configuration under:

```text
/root/scripts/
```

However, production teams should avoid unnecessarily storing application data under `/root`.

---

# 8. `/opt` — Optional / Third-Party Software

`/opt` is commonly used for optional or third-party software.

Example:

```text
/opt
├── application/
├── monitoring/
└── custom-tool/
```

### Production example

An organization may deploy an internally developed application as:

```text
/opt/company-app/
```

with:

```text
/opt/company-app/bin
/opt/company-app/config
/opt/company-app/lib
```

This keeps the application separate from standard operating-system files.

---

# 9. `/srv` — Service Data

`/srv` is intended for data served by system services.

Example:

```text
/srv/www/
/srv/ftp/
/srv/app/
```

### Production example

A web application could theoretically store service data under:

```text
/srv/www/
```

However, many modern production environments use application-specific directories, container volumes, or cloud storage instead.

---

# 10. `/tmp` — Temporary Files

`/tmp` is used for temporary files.

Example:

```bash
echo "temporary data" > /tmp/example.txt
```

Check:

```bash
ls -la /tmp
```

### Production example

Applications may temporarily store:

```text
temporary files
downloaded packages
processing files
runtime artifacts
```

Do **not** assume `/tmp` is persistent. Files may be removed automatically according to the operating system's cleanup policy.

---

# 11. `/run` — Runtime Data

`/run` contains temporary runtime information created after the system boots.

Examples include:

```text
/run
├── systemd/
├── lock/
└── user/
```

It can contain:

```text
PID files
Unix sockets
runtime state
service information
```

### Production example

A service such as Nginx may use runtime files under `/run`, for example:

```text
/run/nginx.pid
```

These files are generally not intended to survive a reboot.

---

# 12. `/proc` — Process and Kernel Information

`/proc` is a **virtual filesystem** that exposes information about running processes and the Linux kernel.

Example:

```bash
ls /proc
```

Check CPU information:

```bash
cat /proc/cpuinfo
```

Check memory:

```bash
cat /proc/meminfo
```

Check a specific process:

```bash
ls /proc/<PID>
```

### Production example

Monitoring agents and troubleshooting tools use information exposed by `/proc` to understand:

```text
CPU usage
Memory usage
Running processes
Kernel information
Process statistics
```

This is especially relevant to:

```text
Prometheus
Node Exporter
top
htop
ps
```

---

# 13. `/sys` — Kernel and Hardware Information

`/sys` is another virtual filesystem that exposes information about devices, hardware, drivers, and the Linux kernel.

Example:

```bash
ls /sys
```

Explore devices:

```bash
ls /sys/class/
```

### Production example

System monitoring and infrastructure tools can use `/sys` to obtain information about:

```text
CPU
Memory
Network interfaces
Block devices
Hardware
Kernel subsystems
```

---

# 14. `/dev` — Device Files

`/dev` contains device files used by Linux to interact with hardware and virtual devices.

Common examples:

```text
/dev/null
/dev/zero
/dev/random
/dev/sda
/dev/nvme0n1
```

### Important examples

#### `/dev/null`

Discards output:

```bash
command > /dev/null
```

#### `/dev/zero`

Provides a stream of zero bytes.

#### Disk devices

A cloud VM might expose a disk such as:

```text
/dev/nvme0n1
```

or:

```text
/dev/sda
```

### Production example

When attaching an additional disk to a cloud VM, engineers may see a new device under:

```text
/dev/
```

They can then partition, format, and mount it.

---

# 15. `/mnt` — Temporary Mount Point

`/mnt` is traditionally used as a temporary mount point for filesystems.

Example:

```bash
sudo mount /dev/sdb1 /mnt
```

Check mounted filesystems:

```bash
df -h
```

### Production example

An administrator may temporarily mount an additional disk under:

```text
/mnt/backup
```

for backup or migration operations.

---

# 16. `/media` — Removable Media

`/media` is commonly used for automatically mounted removable storage.

Examples:

```text
/media/user/USB
/media/user/CD
```

This is more common on desktop Linux systems than on cloud servers.

---

# 17. `/data` — Application / Mounted Data

`/data` is **not a mandatory Linux standard directory**.

It is commonly created by administrators or applications for storing persistent application data.

Example:

```text
/data
├── database
├── application
├── backups
└── logs
```

### Cloud production example

Suppose a Linux VM has an additional storage volume:

```text
/dev/sdb
```

An administrator may mount it as:

```text
/data
```

Then applications can store persistent data there:

```text
/data/application/
/data/database/
/data/backups/
```

### Windows / WSL example

If `/data` maps to a Windows directory such as:

```text
C:\ubuntu-data
```

the exact implementation depends on the environment and mount configuration.

---

# 18. Quick Reference Table

| Directory | Main Purpose                    | DevOps Importance |
| --------- | ------------------------------- | ----------------- |
| `/`       | Root of filesystem              | ⭐⭐⭐⭐⭐      |
| `/boot`   | Boot files and kernel           | ⭐⭐⭐           |
| `/etc`    | Configuration                   | ⭐⭐⭐⭐⭐      |
| `/usr`    | Applications and libraries      | ⭐⭐⭐⭐⭐      |
| `/var`    | Logs and changing data          | ⭐⭐⭐⭐⭐      |
| `/home`   | User files                      | ⭐⭐⭐           |
| `/root`   | Root user's home                | ⭐⭐⭐           |
| `/opt`    | Optional software               | ⭐⭐⭐           |
| `/srv`    | Service data                    | ⭐⭐              |
| `/tmp`    | Temporary files                 | ⭐⭐⭐           |
| `/run`    | Runtime information             | ⭐⭐⭐           |
| `/proc`   | Process/kernel information      | ⭐⭐⭐⭐⭐      |
| `/sys`    | Hardware/kernel information     | ⭐⭐⭐⭐         |
| `/dev`    | Device files                    | ⭐⭐⭐⭐         |
| `/mnt`    | Temporary mount point           | ⭐⭐⭐            |
| `/media`  | Removable media                 | ⭐                 |
| `/data`   | Custom application/mounted data | ⭐⭐⭐⭐          |

---

# 19. Real-World Production Scenario

Consider a production web application running on a Linux cloud server.

```text
                    Linux Production Server
                             |
        ┌────────────────────┼────────────────────┐
        │                    │                    │
       /etc                 /var                 /opt
        │                    │                    │
   Configuration          Logs              Application
        │                    │                    │
   nginx.conf          nginx/access.log      company-app
   ssh config          nginx/error.log       ├── bin
                                             ├── config
                                             └── lib
        │
        └─────────────────────────────────────────
                             |
                           /data
                             |
                       Persistent Data
                       ├── uploads
                       ├── backups
                       └── application-data
```

### Typical operational workflow

A DevOps engineer might:

**1. Configure Nginx**

```bash
sudo vi /etc/nginx/nginx.conf
```

**2. Check application logs**

```bash
sudo tail -f /var/log/nginx/error.log
```

**3. Check disk usage**

```bash
df -h
```

**4. Check running processes**

```bash
ps aux
```

**5. Inspect memory**

```bash
cat /proc/meminfo
```

**6. Check network interfaces**

```bash
ip addr
```

**7. Check mounted storage**

```bash
lsblk
```

**8. Check application files**

```bash
ls -lah /opt/company-app/
```

---

# 20. Production Companies & How These Concepts Apply

Large technology companies and enterprises such as **Amazon, Microsoft, Google, Netflix, Uber, and Airbnb** operate large Linux-based infrastructure environments.

The exact internal directory layouts and operational practices are company-specific, but the underlying Linux filesystem concepts remain fundamental.

### Example: E-commerce Platform

A production e-commerce service might use:

```text
/etc/nginx/             → Nginx configuration
/opt/ecommerce/         → Application
/var/log/               → System/application logs
/data/                  → Persistent application data
/run/                   → Runtime information
/proc/                  → Process/system metrics
/sys/                   → Hardware/kernel information
```

### Example: Monitoring Infrastructure

A monitoring server running Prometheus and Node Exporter may interact with:

```text
/proc/
/sys/
/var/
/etc/
```

to obtain system and application information.

For example:

```bash
node_exporter
```

collects many Linux system metrics exposed through kernel interfaces.

---

# 21. Useful Linux Commands for Filesystem Troubleshooting

### Show filesystem structure

```bash
ls /
```

### Show disk usage

```bash
df -h
```

### Find directory sizes

```bash
du -sh /var/*
```

### Show mounted disks

```bash
lsblk
```

### Show mount information

```bash
mount
```

### Find filesystem type

```bash
df -Th
```

### Check inode usage

```bash
df -i
```

### Find large files

```bash
sudo du -ah /var | sort -rh | head
```

### Check a directory

```bash
ls -lah /etc
```

---

# 22. DevOps Interview Questions

### Q1. What is `/etc` used for?

`/etc` stores system-wide configuration files.

### Q2. What is the difference between `/var` and `/tmp`?

`/var` contains changing system/application data such as logs and caches, while `/tmp` is intended for temporary files.

### Q3. What is `/proc`?

`/proc` is a virtual filesystem that exposes process and kernel information.

### Q4. Is `/data` a standard Linux directory?

No. `/data` is commonly created by administrators or applications for persistent or application-specific data.

### Q5. Why is `/boot` usually not important inside a container?

Containers share the host's kernel and do not normally boot their own Linux operating system.

### Q6. Where would you normally look for Nginx configuration?

Typically:

```text
/etc/nginx/
```

### Q7. Where would you look for Linux logs?

Usually:

```text
/var/log/
```

---

# 23. Key Takeaways

For DevOps and Cloud engineers, focus especially on:

```text
/etc     → Configuration
/var     → Logs & changing data
/usr     → Applications & libraries
/opt     → Third-party applications
/home    → User files
/root    → Root user's home
/tmp     → Temporary files
/run     → Runtime information
/proc    → Processes & kernel information
/sys     → Hardware & kernel information
/dev     → Devices
/mnt     → Temporary mounts
/data    → Custom persistent/application data
```

> **Remember:** Linux filesystem knowledge is not just an administration topic. It is directly useful when troubleshooting **Docker containers, Kubernetes nodes, cloud VMs, Nginx, Jenkins, Prometheus, Node Exporter, databases, CI/CD pipelines, and production servers.**
