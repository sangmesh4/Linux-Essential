<img width="1536" height="1024" alt="3dce60c2-f724-4113-8f77-accad1b72c91" src="https://github.com/user-attachments/assets/161f2bd0-d32b-43d0-bc75-2af6ac60e577" />


# 📝 Linux File Permissions 🔐

Linux uses **file permissions** to control who can **read, write, or execute** files and directories.

Understanding permissions is essential for **Linux administration, DevOps, Cloud, security, and troubleshooting**.

---

## 📌 Permission Types

Linux provides three basic permissions:

| Permission  | Symbol | Meaning                                           |
| ----------- | ------ | ------------------------------------------------- |
| **Read**    | `r`    | View file contents / list directory contents      |
| **Write**   | `w`    | Modify file / create or delete directory contents |
| **Execute** | `x`    | Run a file / access a directory                   |

---

## 👥 Permission Categories

Permissions are assigned to three categories of users:

| Category   | Symbol | Description                    |
| ---------- | ------ | ------------------------------ |
| **Owner**  | `u`    | User who owns the file         |
| **Group**  | `g`    | Group associated with the file |
| **Others** | `o`    | Everyone else                  |

### Example

```text
-rwxr-xr--
```

Breakdown:

```text
-   rwx   r-x   r--
│    │     │     │
│    │     │     └── Others
│    │     └──────── Group
│    └────────────── Owner
└────────────────── File type
```

The owner can **read, write, and execute**.

The group can **read and execute**.

Others can **only read**.

---

## 🔍 Check File Permissions

Use `ls -l`:

```bash
ls -l file.txt
```

Example:

```text
-rw-r--r-- 1 user developers 1200 Sep 15 10:30 file.txt
```

The first 10 characters represent the file type and permissions.

---

## 🛠️ Change Permissions — `chmod`

`chmod` is used to **change file and directory permissions**.

### Symbolic Method

```bash
chmod u+x script.sh
```

Adds execute permission for the owner.

```bash
chmod g+w file.txt
```

Adds write permission for the group.

```bash
chmod o-r file.txt
```

Removes read permission from others.

### Numeric Method

Linux permissions use these values:

| Permission | Value |
| ---------- | ----: |
| `r`        |     4 |
| `w`        |     2 |
| `x`        |     1 |

Example:

```bash
chmod 755 script.sh
```

Means:

```text
Owner   → 7 = rwx
Group   → 5 = r-x
Others  → 5 = r-x
```

So:

```text
755 = rwxr-xr-x
```

---

## 🏢 Real-World Numeric Permission Examples

Numeric permissions are commonly used in **production Linux servers, web servers, application servers, CI/CD environments, and cloud infrastructure**.

### `755` — Executable Scripts & Application Directories

```bash
chmod 755 deploy.sh
```

```text
Owner   → rwx
Group   → r-x
Others  → r-x
```

**Typical use:** Shell scripts, executable files, and application directories where users need to access/execute but should not modify the files.

---

### `644` — Configuration & Static Files

```bash
chmod 644 nginx.conf
```

```text
Owner   → rw-
Group   → r--
Others  → r--
```

**Typical use:** Configuration files, HTML, CSS, JavaScript, documentation, and other files that most users only need to read.

---

### `600` — Private Files & Credentials

```bash
chmod 600 application.env
```

```text
Owner   → rw-
Group   → ---
Others  → ---
```

**Typical use:** Files containing sensitive information such as private configuration, credentials, or secrets.

For SSH private keys:

```bash
chmod 600 my-key.pem
```

---

### `775` — Shared Application Directories

```bash
chmod 775 /var/www/application
```

```text
Owner   → rwx
Group   → rwx
Others  → r-x
```

**Typical use:** Application directories where the owner and an authorized group need to create or modify files, while others only need access.

---

### `664` — Group-Shared Files

```bash
chmod 664 application.log
```

```text
Owner   → rw-
Group   → rw-
Others  → r--
```

**Typical use:** Files that need to be modified by both the application owner and members of a specific Linux group.

---

### `750` — Restricted Application Directories

```bash
chmod 750 /opt/application
```

```text
Owner   → rwx
Group   → r-x
Others  → ---
```

**Typical use:** Application or administrative directories where access should be limited to the owner and an authorized group.

> **💡 Production Tip:** Avoid using `777` unless there is a specific, well-understood requirement. `777` gives **read, write, and execute permissions to everyone** and can create unnecessary security risks.

---

## 👤 Change Ownership

### `chown`

Changes the owner of a file:

```bash
sudo chown user file.txt
```

Change owner and group:

```bash
sudo chown user:developers file.txt
```

### `chgrp`

Changes only the group:

```bash
sudo chgrp developers file.txt
```

---

## 📁 Directory Permissions

Directory permissions work slightly differently:

| Permission | Directory Meaning            |
| ---------- | ---------------------------- |
| `r`        | List directory contents      |
| `w`        | Create/delete/rename entries |
| `x`        | Enter/access the directory   |

Example:

```bash
chmod 755 /var/www
```

---

## ⚡ Common Commands

```bash
# View permissions
ls -l

# Change permissions
chmod 644 file.txt

# Make script executable
chmod +x script.sh

# Change owner
sudo chown user file.txt

# Change owner and group
sudo chown user:group file.txt

# Change group
sudo chgrp group file.txt
```

---

## 🚀 File Permissions in DevOps

File permissions are commonly used when working with:

* 🐧 Linux servers
* 🔑 SSH keys
* 🌐 Nginx / Apache
* 🐳 Docker
* ☁️ Cloud VMs
* 🔄 CI/CD pipelines
* 📜 Shell scripts
* ⚙️ Configuration files

### Example: SSH Key Permission

Private SSH keys should not be accessible to other users:

```bash
chmod 600 my-key.pem
```

---

## 🎯 Quick Cheat Sheet

```text
r = 4  → Read
w = 2  → Write
x = 1  → Execute

u = Owner
g = Group
o = Others

chmod 755 file  → rwxr-xr-x
chmod 644 file  → rw-r--r--
chmod 600 file  → rw-------
chmod +x file   → Add execute permission

chown user file
chown user:group file
chgrp group file
```

## 💡 Key Takeaway: 
 Linux file permissions provide a fundamental layer of **access control and security**. Mastering `ls -l`, `chmod`, `chown`, and `chgrp` is essential for every Linux and DevOps engineer.  
