# ❓ Linux FAQs for DevOps & Cloud

Linux is one of the most widely used operating systems in **DevOps, Cloud Computing, SRE, Kubernetes, Docker, CI/CD, and production infrastructure**.

These frequently asked questions cover the Linux concepts and commands commonly encountered by beginners and during DevOps/Cloud interviews.

---

## 🐧 General Linux FAQs

### 1. What is Linux?

Linux is an **open-source operating system kernel**. Linux distributions such as Ubuntu, Debian, Rocky Linux, Amazon Linux, and Fedora combine the Linux kernel with system utilities and applications to provide a complete operating system.

---

### 2. Why is Linux important for DevOps?

Linux is heavily used for servers, containers, cloud workloads, automation, and CI/CD systems.

DevOps engineers commonly use Linux for:

* Server administration
* Docker and Kubernetes
* Jenkins and CI/CD
* Shell scripting
* Infrastructure automation
* Monitoring
* Application deployment
* Cloud virtual machines

---

### 3. What is a Linux distribution?

A Linux distribution is a complete operating system built around the Linux kernel.

Examples:

```text
Ubuntu
Debian
Rocky Linux
AlmaLinux
Fedora
Amazon Linux
Arch Linux
Alpine Linux
```

---

### 4. What is the difference between Linux and Ubuntu?

**Linux** refers primarily to the kernel, while **Ubuntu** is a Linux distribution that packages the kernel with system tools, libraries, applications, and package-management tools.

```text
 Linux Kernel
     +
System Utilities
     +
 Applications
     ↓
Linux Distribution
     ↓
   Ubuntu
```

---

# 💻 Linux Commands FAQs

### 5. How do you check the current directory?

```bash
pwd
```

`pwd` means **Print Working Directory**.

---

### 6. How do you list files?

```bash
ls
```

Useful options:

```bash
ls -l
ls -la
ls -lh
```

---

### 7. How do you find files?

```bash
find /var/log -name "*.log"
```

Example:

```bash
find /etc -name "nginx.conf"
```

---

### 8. How do you search text inside files?

```bash
grep "error" application.log
```

Recursive search:

```bash
grep -r "error" /var/log/
```

---

### 9. How do you view a file?

```bash
cat file.txt
```

For large files:

```bash
less file.txt
```

---

### 10. How do you monitor logs in real time?

```bash
tail -f application.log
```

For systemd services:

```bash
journalctl -f
```

---

# 👤 Linux Users & Permissions FAQs

### 11. What is the root user?

The root user is the Linux superuser with **UID 0** and has extensive administrative privileges.

---

### 12. What is sudo?

`sudo` allows an authorized user to execute commands with elevated privileges.

Example:

```bash
sudo systemctl restart nginx
```

---

### 13. How do you check file permissions?

```bash
ls -l
```

Example:

```text
-rwxr-xr--
```

This represents permissions for:

```text
Owner | Group | Others
```

---

### 14. How do you change file permissions?

```bash
chmod 755 script.sh
```

For a script:

```bash
chmod +x script.sh
```

---

### 15. How do you change file ownership?

```bash
sudo chown user:group file.txt
```

---

# ⚙️ Processes & Services FAQs

### 16. What is a process?

A process is an instance of a running program.

Every process normally has a unique **Process ID (PID)**.

Check processes:

```bash
ps aux
```

---

### 17. How do you find a specific process?

```bash
ps aux | grep nginx
```

You can also use:

```bash
pgrep nginx
```

---

### 18. How do you terminate a process?

```bash
kill <PID>
```

If necessary:

```bash
kill -9 <PID>
```

Use `SIGKILL` carefully because it does not allow the process to perform normal cleanup.

---

### 19. What is systemd?

`systemd` is a system and service manager commonly used as **PID 1** on modern Linux distributions.

It manages:

* Services
* System initialization
* Targets
* Mounts
* Timers
* Logging integration

---

### 20. How do you check a service?

```bash
systemctl status nginx
```

---

### 21. How do you start, stop, and restart a service?

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
```

---

### 22. How do you make a service start automatically after reboot?

```bash
sudo systemctl enable nginx
```

Verify:

```bash
systemctl is-enabled nginx
```

---

# 📊 Linux Monitoring FAQs

### 23. How do you check CPU and memory usage?

```bash
top
```

or:

```bash
htop
```

---

### 24. How do you check memory usage?

```bash
free -h
```

---

### 25. How do you check disk usage?

```bash
df -h
```

---

### 26. How do you check which directories consume the most space?

```bash
du -sh /*
```

For a specific directory:

```bash
du -sh /var/*
```

---

### 27. How do you check system load?

```bash
uptime
```

Example:

```text
load average: 0.20, 0.35, 0.40
```

The values represent system load averages over approximately:

```text
1 minute | 5 minutes | 15 minutes
```

---

# 🌐 Linux Networking FAQs

### 28. How do you check the IP address?

Modern command:

```bash
ip addr
```

Short form:

```bash
ip a
```

---

### 29. How do you check the routing table?

```bash
ip route
```

---

### 30. How do you test network connectivity?

```bash
ping google.com
```

---

### 31. How do you check DNS resolution?

```bash
nslookup google.com
```

or:

```bash
dig google.com
```

---

### 32. How do you check listening ports?

```bash
ss -tulnp
```

Example:

```text
LISTEN 0 128 0.0.0.0:80
```

This can help identify whether a service is listening on HTTP port `80`.

---

### 33. What is `curl` used for?

`curl` is commonly used to make HTTP requests and test endpoints.

Example:

```bash
curl http://localhost
```

Check only HTTP headers:

```bash
curl -I https://example.com
```

This is particularly useful when troubleshooting **load balancers, APIs, web servers, and application endpoints**.

---

# 💾 Linux Storage FAQs

### 34. How do you list disks and partitions?

```bash
lsblk
```

---

### 35. How do you check mounted filesystems?

```bash
findmnt
```

---

### 36. What is `/etc/fstab`?

`/etc/fstab` defines filesystems that Linux can mount automatically, typically during boot.

Example:

```text
UUID=xxxx  /data  ext4  defaults  0  2
```

An incorrect `/etc/fstab` entry can contribute to boot problems.

---

### 37. What is the difference between `df` and `du`?

| Command  | Purpose                               |
| -------- | ------------------------------------- |
| `df -h`  | Shows filesystem disk usage           |
| `du -sh` | Shows space used by files/directories |

---

# 📦 Package Management FAQs

### 38. What is a package manager?

A package manager installs, updates, removes, and manages software packages and their dependencies.

Examples:

| Distribution Family       | Package Manager |
| ------------------------- | --------------- |
| Debian / Ubuntu           | `apt`           |
| RHEL / Rocky / AlmaLinux  | `dnf`           |
| Older RHEL/CentOS systems | `yum`           |
| Alpine                    | `apk`           |

Example:

```bash
sudo apt install nginx
```

or:

```bash
sudo dnf install nginx
```

---

# 🐳 Linux & Docker FAQs

### 39. Why is Linux important for Docker?

Docker containers commonly use Linux kernel features such as:

* Namespaces
* cgroups
* Capabilities
* Linux networking
* Filesystems

Containers share the host kernel rather than running a separate kernel for each container.

---

### 40. What is the difference between a VM and a container?

```text
Virtual Machine
────────────────
Application
Guest OS
Libraries
Virtual Hardware
Hypervisor
Host OS/Hardware
```

```text
Container
────────────────
Application
Libraries
Container
Docker/Container Runtime
Linux Kernel
Hardware
```

Containers are generally more lightweight than full virtual machines because containers share the host kernel.

---

# ☁️ Linux & Cloud FAQs

### 41. Why are Linux servers widely used in Cloud?

Linux is commonly used for cloud workloads because of its:

* Open-source ecosystem
* Automation capabilities
* Strong networking tools
* Container support
* Extensive server tooling
* Wide cloud-provider support

---

### 42. How do you connect to a Linux cloud server?

For SSH:

```bash
ssh -i key.pem user@server-ip
```

Example:

```bash
ssh -i my-key.pem ubuntu@203.0.113.10
```

The exact username depends on the cloud image.

---

### 43. What should you check if you cannot SSH into a cloud VM?

Use a layered troubleshooting approach:

```text
Cloud VM Running?
       ↓
Correct IP / DNS?
       ↓
Network Route?
       ↓
Security Group / Firewall?
       ↓
Port 22 Accessible?
       ↓
SSH Service Running?
       ↓
Correct Username / Key?
       ↓
Login
```

Useful Linux-side checks include:

```bash
systemctl status ssh
ss -tulnp | grep :22
```

---

# 🔄 Linux & DevOps FAQs

### 44. Why is Linux important for CI/CD?

CI/CD tools frequently execute builds, tests, scripts, containers, and deployments on Linux-based systems.

A typical pipeline might look like:

```text
Developer
    ↓
Git
    ↓
CI/CD Pipeline
    ↓
Linux Build Agent
    ↓
Build
    ↓
Test
    ↓
Docker
    ↓
Deployment
```

---

### 45. How is Linux used with Jenkins?

Jenkins can run on Linux and execute shell commands during pipeline stages.

Example:

```bash
#!/bin/bash

echo "Building application..."
docker build -t myapp .
docker run -d -p 8080:8080 myapp
```

---

### 46. Why is shell scripting important for DevOps?

Shell scripting helps automate repetitive Linux tasks.

Example:

```bash
#!/bin/bash

echo "Checking disk usage..."
df -h

echo "Checking memory..."
free -h

echo "Checking services..."
systemctl --failed
```

---

# 🔐 Linux Security FAQs

### 47. How do you check who is currently logged in?

```bash
who
```

or:

```bash
w
```

---

### 48. How do you check recent login history?

```bash
last
```

---

### 49. Why should you avoid using root for everything?

Using root unnecessarily increases the impact of mistakes or compromised processes.

Prefer:

```bash
sudo <command>
```

when administrative privileges are actually required.

---

# 🚨 Linux Troubleshooting FAQs

### 50. What is your approach when a Linux server is slow?

Use a structured approach:

```text
1. Check CPU
2. Check Memory
3. Check Disk Space
4. Check Disk I/O
5. Check Processes
6. Check Network
7. Check Logs
8. Identify Root Cause
9. Fix
10. Verify
```

Useful commands:

```bash
top
free -h
df -h
ps aux
uptime
ss -tulnp
journalctl -p err
```

---

### 51. How do you find failed systemd services?

```bash
systemctl --failed
```

---

### 52. How do you check system logs?

```bash
journalctl
```

Current boot:

```bash
journalctl -b
```

Errors:

```bash
journalctl -p err
```

Specific service:

```bash
journalctl -u nginx
```

---

### 53. How do you troubleshoot a service that is not starting?

Follow this sequence:

```bash
systemctl status <service>
journalctl -u <service>
```

Then:

```text
Check configuration
       ↓
Check permissions
       ↓
  Check ports
       ↓
Check dependencies
       ↓
Check resources
       ↓
Fix root cause
       ↓
   Restart
       ↓
    Verify
```

---

# 🎯 Essential Linux Commands for DevOps

| Category    | Commands                            |
| ----------- | ----------------------------------- |
| Files       | `ls`, `cp`, `mv`, `rm`, `find`      |
| Text        | `cat`, `less`, `grep`, `awk`, `sed` |
| Users       | `id`, `who`, `useradd`, `passwd`    |
| Permissions | `chmod`, `chown`                    |
| Processes   | `ps`, `top`, `pgrep`, `kill`        |
| Services    | `systemctl`, `journalctl`           |
| Storage     | `lsblk`, `df`, `du`, `findmnt`      |
| Networking  | `ip`, `ss`, `ping`, `curl`, `dig`   |
| Performance | `top`, `free`, `uptime`, `iostat`   |
| Packages    | `apt`, `dnf`, `yum`, `apk`          |
| Archives    | `tar`, `gzip`, `zip`, `unzip`       |
| Security    | `sudo`, `ssh`, `chmod`, `chown`     |

---

# 💼 What Recruiters Expect from a DevOps Engineer

A DevOps engineer should be able to do more than memorize Linux commands.

You should be comfortable with:

```text
Linux Fundamentals
       ↓
Users & Permissions
       ↓
Processes & Services
       ↓
   Networking
       ↓
    Storage
       ↓
Logs & Monitoring
       ↓
Shell Scripting
       ↓
    Docker
       ↓
     CI/CD
       ↓
     Cloud
       ↓
 Troubleshooting
```

### ⭐ The Most Important Skill

**Troubleshooting with a structured approach.**

When something fails, think:

> **What changed? → What is failing? → What evidence do the logs show? → What is the root cause? → How can I verify the fix?**

---

# 🏆 Quick Interview Revision

```text
Linux Kernel        → Core of the operating system
systemd             → System & service manager
PID 1               → Usually systemd
SSH                 → Remote administration
sudo                → Execute commands with elevated privileges
chmod               → Change permissions
chown               → Change ownership
ps                  → Process information
top                 → Real-time system/process monitoring
df                  → Filesystem disk usage
du                  → Directory/file disk usage
ss                  → Network sockets/listening ports
journalctl          → systemd journal logs
systemctl           → Manage systemd services
grep                → Search text
curl                → HTTP/API testing
lsblk               → Block devices/storage
```

---

# 🚀 Final Takeaway

Linux is not just another technology to learn for DevOps.

It is a **core foundation** for working with servers, containers, CI/CD platforms, cloud infrastructure, monitoring systems, and production environments.

```text
                DEVOPS
                   │
        ┌──────────┴──────────┐
        │                     │
      CLOUD                 CI/CD
        │                     │
        └──────────┬──────────┘
                   │
                 LINUX
                   │
       ┌───────────┼───────────┐
       │           │           │
    Networking  Processes   Automation
       │           │           │
     Storage     Services    Scripting
       │           │           │
       └───────────┼───────────┘
                   │
             Troubleshooting
```

> **Learn Linux → Automate Linux → Troubleshoot Linux → Build reliable infrastructure.**
