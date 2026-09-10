<img width="1536" height="1024" alt="df52e614-8267-4905-aeba-6e3ca19c8483" src="https://github.com/user-attachments/assets/46b3a8b3-5667-4fde-824d-5fb001ba5af8" />


# 👥 Linux User Management

> **A practical guide to Linux users, groups, permissions, sudo access, SSH, service accounts, and real-world DevOps practices.**

---

## 📌 Overview

Linux is a **multi-user operating system**, allowing multiple users and services to operate on the same system simultaneously.

Effective user management is essential for:

* 🔐 System security
* 👥 Access control
* 🛡️ Privilege management
* 📋 Auditing and compliance
* ⚙️ DevOps automation
* 🖥️ Server administration
* 🔑 SSH-based access management

In production environments, Linux administrators and DevOps engineers regularly create users, manage groups, configure `sudo` privileges, control SSH access, and disable unused accounts.

---

# 👤 Types of Linux Users

Linux users can generally be categorized into **three major types** based on their purpose, UID, and level of access.

| User Type              | Typical UID | Primary Purpose       | Access                    |
| ---------------------- | ----------: | --------------------- | ------------------------- |
| 👑 **Root User**       |         `0` | System administration | Full / unrestricted       |
| ⚙️ **System User**     |    `1–999`* | Services and daemons  | Limited / non-interactive |
| 👨‍💻 **Regular User** |    `1000+`* | Human users           | Limited by default        |

> **Note:** UID ranges can vary by distribution and configuration. The ranges above are common defaults, not universal rules.

---

## 👑 1. Root User

The **root user** is the Linux superuser and always has:

```text
UID = 0
```

Root has unrestricted access to the operating system and can:

* Create and delete users
* Install and remove software
* Modify system configuration
* Change file ownership and permissions
* Start and stop services
* Access protected files
* Manage disks and networking
* Modify almost any system resource

Check the root user's UID:

```bash
id root
```

Example:

```text
uid=0(root) gid=0(root) groups=0(root)
```

### ⚠️ Production Best Practice

Avoid using the root account for routine activities.

Instead, use a normal administrative account with controlled `sudo` privileges:

```bash
sudo systemctl restart nginx
```

This provides better accountability and reduces the risk of accidental system-wide changes.

---

## ⚙️ 2. System Users

**System users** are generally created for operating-system components, applications, services, and daemons.

Common examples include:

```text
mysql
www-data
nginx
redis
prometheus
jenkins
```

They typically:

* Have lower UIDs than regular users
* Run background services
* Own application/service files
* Do not require interactive login
* Have restricted permissions

Check a service user's information:

```bash
id www-data
```

Example:

```text
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

### Create a System User

```bash
useradd -r -s /usr/sbin/nologin appuser
```

Here:

* `-r` → Creates a system account
* `-s /usr/sbin/nologin` → Prevents normal interactive login

### 💡 Real-World DevOps Example

A monitoring application such as Prometheus can run under a dedicated service account instead of `root`.

```text
Prometheus
    │
    ▼
prometheus user
    │
    ▼
Limited permissions
    │
    ▼
Better security
```

---

## 👨‍💻 3. Regular Users

**Regular users** are normally human users who interact with the Linux system.

On many Linux distributions, regular users start at:

```text
UID >= 1000
```

They generally have:

* Their own home directory
* Limited system permissions
* Their own files and processes
* No unrestricted root access
* `sudo` access only when explicitly granted

Example:

```bash
id john
```

Output:

```text
uid=1001(john) gid=1001(john) groups=1001(john)
```

The user's home directory is typically:

```text
/home/john
```

### Grant Administrative Access

On Debian/Ubuntu:

```bash
usermod -aG sudo john
```

On RHEL-based systems:

```bash
usermod -aG wheel john
```

The user can then perform authorized administrative operations using:

```bash
sudo command
```

---

## 🔎 Identify User Type

You can use the user's UID to understand what type of account it generally represents:

```bash
id username
```

Or:

```bash
getent passwd username
```

Example:

```text
root:x:0:0:root:/root:/bin/bash
mysql:x:999:999:MySQL Server:/nonexistent:/usr/sbin/nologin
john:x:1001:1001:John:/home/john:/bin/bash
```

Conceptually:

```text
UID 0
 │
 └── Root User
       Full System Access

UID 1–999*
 │
 └── System Users
       Services / Daemons

UID 1000+*
 │
 └── Regular Users
       Human / Interactive Users
```

> **Important:** UID ranges are distribution-dependent. Always verify the actual system configuration rather than assuming a fixed range.

---

# 📂 Important User Management Files

Linux stores user and group information in several important files.

| File              | Purpose                                               |
| ----------------- | ----------------------------------------------------- |
| `/etc/passwd`     | Contains basic user account information               |
| `/etc/shadow`     | Stores password hashes and password aging information |
| `/etc/group`      | Contains group information                            |
| `/etc/gshadow`    | Stores secure group information                       |
| `/etc/sudoers`    | Defines `sudo` privileges                             |
| `/etc/sudoers.d/` | Recommended location for custom sudo configurations   |
| `/etc/login.defs` | Defines default login and password policies           |

### View User Information

```bash
cat /etc/passwd
```

Example:

```text
john:x:1001:1001:John:/home/john:/bin/bash
```

The fields represent:

```text
username : password : UID : GID : comment : home : shell
```

> **Important:** Password hashes are normally stored in `/etc/shadow`, not `/etc/passwd`.

---

# 👤 Creating Users

## Using `useradd`

`useradd` is commonly used for creating users in scripts and automation.

```bash
useradd username
```

Create a user with a home directory:

```bash
useradd -m username
```

Create a user with a specific shell:

```bash
useradd -m -s /bin/bash username
```

Create a user with a specific UID:

```bash
useradd -u 1050 username
```

Create a user with a specific primary group:

```bash
useradd -g developers username
```

### Set Password

```bash
passwd username
```

### Verify User

```bash
id username
```

Example:

```text
uid=1050(username) gid=1050(username) groups=1050(username)
```

---

# ➕ `adduser` Command

On Debian/Ubuntu systems, `adduser` provides an interactive interface for creating users.

```bash
adduser username
```

It can ask for:

* Password
* Full name
* Additional user information

### `useradd` vs `adduser`

| Command   | Typical Usage                                        |
| --------- | ---------------------------------------------------- |
| `useradd` | Automation, scripts, low-level user creation         |
| `adduser` | Interactive user creation, commonly on Debian/Ubuntu |

> **Company/DevOps Practice:** `useradd` is often preferred in automation because it is easier to use predictably inside scripts.

---

# 🔐 Managing User Passwords

Set or change a password:

```bash
passwd username
```

Change your own password:

```bash
passwd
```

### Lock User Account

```bash
passwd -l username
```

### Unlock User Account

```bash
passwd -u username
```

### Check Password Status

```bash
passwd -S username
```

---

# ⏳ Password Expiration

Linux provides the `chage` command for password aging.

Set maximum password age to 90 days:

```bash
chage -M 90 username
```

Set minimum password age:

```bash
chage -m 1 username
```

Set warning period:

```bash
chage -W 7 username
```

Force password change at next login:

```bash
chage -d 0 username
```

View password aging information:

```bash
chage -l username
```

---

# ✏️ Modifying Users

The `usermod` command modifies an existing user account.

### Change Username

```bash
usermod -l new_username old_username
```

### Change Home Directory

```bash
usermod -d /new/home/directory -m username
```

The `-m` option moves the existing home directory content.

### Change Default Shell

```bash
usermod -s /bin/zsh username
```

### Change UID

```bash
usermod -u 1050 username
```

> **Caution:** Changing UIDs on production systems can affect file ownership and application permissions.

---

# 👥 Working with Groups

Groups simplify permission management by allowing multiple users to share access.

## Create a Group

```bash
groupadd developers
```

## Add User to a Secondary Group

```bash
usermod -aG developers username
```

### Important

Always use `-aG` when adding a user to an additional group:

```bash
usermod -aG developers username
```

Avoid:

```bash
usermod -G developers username
```

because it can replace the user's existing supplementary group memberships.

---

# 🔎 View Group Membership

Check the groups of a user:

```bash
groups username
```

More detailed information:

```bash
id username
```

View group information:

```bash
getent group developers
```

---

# ⭐ Change Primary Group

```bash
usermod -g developers username
```

Verify:

```bash
id username
```

---

# 🗑️ Removing Users

Delete a user:

```bash
userdel username
```

Delete the user and their home directory:

```bash
userdel -r username
```

> **Production Practice:** Before deleting an account, verify whether the user owns files, runs processes, or is associated with scheduled jobs.

Find files owned by a user:

```bash
find / -user username 2>/dev/null
```

---

# 🛡️ Sudo Access

`sudo` allows authorized users to execute commands with elevated privileges.

## Debian/Ubuntu

Add user to the `sudo` group:

```bash
usermod -aG sudo username
```

## RHEL/CentOS/Rocky/AlmaLinux

Add user to the `wheel` group:

```bash
usermod -aG wheel username
```

Verify:

```bash
groups username
```

The user may need to log out and log back in for the new group membership to take effect.

---

# ⚙️ Sudoers Configuration

Edit the sudoers configuration safely using:

```bash
visudo
```

Example:

```text
username ALL=(ALL) ALL
```

This allows the user to execute commands with `sudo`.

### Passwordless Sudo

```text
username ALL=(ALL) NOPASSWD: ALL
```

### Allow Only a Specific Command

```text
username ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx
```

This follows the **principle of least privilege** better than granting unrestricted sudo access.

---

# 📁 `/etc/sudoers.d/` — Recommended Practice

Instead of modifying the main `/etc/sudoers` file for every custom rule, create a dedicated configuration file:

```bash
visudo -f /etc/sudoers.d/devops
```

Example:

```text
devops ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx
```

This approach makes permissions:

* Easier to manage
* Easier to audit
* Easier to remove
* Less likely to interfere with the main sudo configuration

---

# 🔑 SSH User Management

In real-world DevOps environments, engineers commonly access Linux servers through SSH.

Connect to a server:

```bash
ssh username@server-ip
```

Example:

```bash
ssh devops@192.168.1.100
```

### SSH Key-Based Authentication

Generate an SSH key pair:

```bash
ssh-keygen
```

Copy the public key to a server:

```bash
ssh-copy-id username@server-ip
```

This enables key-based authentication instead of relying only on passwords.

---

# 🔐 SSH Authorized Keys

User-specific SSH public keys are normally stored in:

```text
~/.ssh/authorized_keys
```

Example:

```bash
cat ~/.ssh/authorized_keys
```

Recommended permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

---

# ⚙️ Service Accounts

Production applications often use dedicated service accounts instead of running applications as `root`.

Example:

```bash
useradd -r -s /usr/sbin/nologin appuser
```

The `-r` option creates a system account.

The `nologin` shell prevents normal interactive login.

Example use cases:

* Nginx
* Jenkins
* Prometheus
* Node Exporter
* Application services
* Monitoring agents

### Why Service Accounts?

Using dedicated accounts provides:

* Better security
* Process isolation
* Clear ownership
* Reduced root privileges
* Easier auditing

---

# 🔍 Find User Information

Check user ID and groups:

```bash
id username
```

Find a user's home directory:

```bash
getent passwd username
```

Check currently logged-in users:

```bash
who
```

More detailed login information:

```bash
w
```

Show the current user:

```bash
whoami
```

Show recent login history:

```bash
last
```

---

# 👀 Find Currently Logged-In Users

```bash
who
```

Example:

```text
john     pts/0    2026-09-10 10:20
admin    pts/1    2026-09-10 10:35
```

Check active sessions:

```bash
w
```

This can help administrators identify unexpected user activity.

---

# 🔎 Check User Processes

View processes belonging to a user:

```bash
ps -u username
```

Example:

```bash
ps -fu username
```

This is useful before disabling or deleting a user.

---

# 🚫 Disable User Login

Instead of immediately deleting an employee or service account, access can be disabled.

Lock the password:

```bash
passwd -l username
```

Disable interactive shell access:

```bash
usermod -s /usr/sbin/nologin username
```

Check the configured shell:

```bash
getent passwd username
```

> **Real-World Practice:** For employee offboarding, access is commonly disabled first so that files, ownership, logs, and audit requirements can be reviewed before account deletion.

---

# 📂 File Ownership and Users

Linux permissions are closely connected with user management.

Check file ownership:

```bash
ls -l
```

Example:

```text
-rw-r--r-- 1 john developers 1024 app.log
```

Here:

```text
Owner → john
Group → developers
```

Change file owner:

```bash
chown username file.txt
```

Change owner and group:

```bash
chown username:developers file.txt
```

Change group:

```bash
chgrp developers file.txt
```

---

# 🔒 User and Group Permissions

Example:

```bash
chmod 750 script.sh
```

Permission structure:

```text
7 → Owner
5 → Group
0 → Others
```

Where:

```text
7 = rwx
5 = r-x
0 = ---
```

This is frequently used to control access to application directories and deployment scripts.

---

# 🚀 User Management in DevOps

User management is commonly required when setting up:

### Jenkins

Create a dedicated service account rather than running workloads unnecessarily as `root`.

### Prometheus

Run Prometheus and exporters with dedicated service accounts.

### Docker

Users may need membership in the `docker` group:

```bash
usermod -aG docker username
```

> **Security Note:** Membership in the `docker` group can provide privileges equivalent to root in many configurations. Grant it only when appropriate.

### Application Servers

Create application-specific accounts:

```bash
useradd -r -s /usr/sbin/nologin appuser
```

---

# 🤖 User Management Automation

In DevOps environments, user creation can be automated using:

* Bash scripts
* Ansible
* Terraform workflows
* Configuration management tools
* Cloud-init
* IAM/SSO integrations

Example Bash automation:

```bash
#!/bin/bash

USERNAME="devops"

if id "$USERNAME" &>/dev/null; then
    echo "User already exists"
else
    useradd -m -s /bin/bash "$USERNAME"
    echo "User created successfully"
fi
```

---

# 🏢 Real-World Company Scenario

## Scenario

A company wants to create a `devops` user who can restart Nginx but should not have unrestricted root access.

### Step 1 — Create User

```bash
useradd -m -s /bin/bash devops
```

### Step 2 — Set Password

```bash
passwd devops
```

### Step 3 — Create Sudo Rule

```bash
visudo -f /etc/sudoers.d/devops
```

Add:

```text
devops ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx
```

### Step 4 — Test

Switch to the user:

```bash
su - devops
```

Run:

```bash
sudo systemctl restart nginx
```

The user can restart Nginx without receiving unrestricted administrative access.

---

# 🔄 User Management Flow

```text
                         Linux Server
                              │
                    ┌─────────┴─────────┐
                    │                   │
                 👥 Users            👥 Groups
                    │                   │
                    └─────────┬─────────┘
                              │
                       🔐 Permissions
                              │
                 ┌────────────┴────────────┐
                 │                         │
              📂 Files                  ⚙️ Services
                 │                         │
                 └────────────┬────────────┘
                              │
                       🛡️ Sudo / SSH
                              │
                         🔒 Security
```

---

# ⭐ Production Best Practices

### 🔐 1. Follow Least Privilege

Give users only the permissions they actually require.

Avoid giving unrestricted `sudo` access unless necessary.

### 👤 2. Avoid Using Root Directly

Use a normal administrative account with `sudo` whenever possible.

### 🔑 3. Prefer SSH Keys

For server access, SSH key authentication is generally preferred over password-only authentication.

### 🚫 4. Disable Unused Accounts

Inactive accounts increase the attack surface.

### ⚙️ 5. Use Dedicated Service Accounts

Applications and monitoring tools should use dedicated accounts with only the required permissions.

### 👥 6. Use Groups for Access Control

Instead of managing permissions individually:

```text
Users → Groups → Resources
```

This makes access management easier at scale.

### 📋 7. Audit User Activity

Useful commands include:

```bash
last
who
w
```

On systems using `systemd`, authentication logs can also be investigated with:

```bash
journalctl
```

### ⚠️ 8. Validate Sudo Configuration

Always use:

```bash
visudo
```

rather than directly editing `/etc/sudoers`.

### 💾 9. Review Ownership Before Deleting Users

Before:

```bash
userdel -r username
```

check whether the user owns important files or processes.

---

# 🧰 Useful Real-Time Commands

| Requirement             | Command                          |
| ----------------------- | -------------------------------- |
| Current user            | `whoami`                         |
| User details            | `id username`                    |
| Create user             | `useradd -m username`            |
| Set password            | `passwd username`                |
| Modify user             | `usermod`                        |
| Delete user             | `userdel username`               |
| Delete user + home      | `userdel -r username`            |
| Create group            | `groupadd groupname`             |
| Add user to group       | `usermod -aG groupname username` |
| List groups             | `groups username`                |
| Check password aging    | `chage -l username`              |
| Lock account            | `passwd -l username`             |
| Unlock account          | `passwd -u username`             |
| Current sessions        | `who`                            |
| Detailed sessions       | `w`                              |
| Login history           | `last`                           |
| User processes          | `ps -fu username`                |
| Edit sudo configuration | `visudo`                         |
| User database lookup    | `getent passwd username`         |
| Group database lookup   | `getent group groupname`         |

---

# 🎯 Key Takeaways

* Linux is a **multi-user operating system**.
* Linux users can generally be categorized as **root, system, and regular users**.
* Root always has **UID `0`**.
* System users are primarily used by **services and daemons**.
* Regular users are generally human accounts with **UIDs starting at `1000`** on many distributions.
* `/etc/passwd` contains basic user account information.
* `/etc/shadow` stores password hashes and aging information.
* `useradd` creates users.
* `usermod` modifies users.
* `userdel` removes users.
* `groupadd` creates groups.
* `usermod -aG` adds users to supplementary groups.
* `passwd` manages passwords and account locking.
* `chage` manages password expiration.
* `sudo` provides controlled administrative access.
* `/etc/sudoers.d/` is useful for modular sudo policies.
* Dedicated service accounts improve security.
* SSH keys are widely used for secure server access.
* Least privilege should be followed in production.
* User management is an important Linux foundation for **DevOps, Cloud, System Administration, and Security**.

---

## 💼 DevOps Perspective

```text
Linux User Management
        │
        ├── 👤 Users
        │     ├── Root
        │     ├── System Users
        │     └── Regular Users
        │
        ├── 👥 Groups
        │
        ├── 🔐 Permissions
        │
        ├── 🛡️ Sudo
        │
        ├── 🔑 SSH
        │
        ├── ⚙️ Service Accounts
        │
        └── 🤖 Automation
              ├── Bash
              ├── Ansible
              ├── Cloud-init
              └── IAM / SSO
```

> **Mastering Linux user management is essential for building secure, scalable, and production-ready DevOps & Cloud environments.**
