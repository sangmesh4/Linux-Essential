<img width="1536" height="1024" alt="9c954cc7-118f-4ea9-8092-4665933c600b" src="https://github.com/user-attachments/assets/393be8a3-fed9-4880-9771-5d8fd4cf869e" />



# vi & vim Editor — Modes & Commands

`vi` (Visual Editor) is a powerful, lightweight, terminal-based text editor available on almost every Linux system. It is commonly used by system administrators and DevOps engineers to edit configuration files, scripts, logs, and other system files directly from the command line.

**Vim (Vi Improved)** is an enhanced version of `vi` that provides additional features such as syntax highlighting, plugins, code navigation, multiple windows, improved editing capabilities, and customization.

> **Quick Difference:** `vi` provides the classic editor experience, while `vim` extends `vi` with many additional features for modern development and DevOps workflows.

---

## 📌 vi vs vim

| Feature                         | `vi`                 | `vim`                          |
| ------------------------------- | -------------------- | ------------------------------ |
| Full Name                       | Visual Editor        | Vi Improved                    |
| Basic Editing                   | ✅                    | ✅                              |
| Normal / Insert / Command Modes | ✅                    | ✅                              |
| Syntax Highlighting             | Limited              | ✅                              |
| Multiple Windows                | Basic                | Advanced                       |
| Plugins                         | Limited              | ✅                              |
| Customization                   | Limited              | Extensive                      |
| Code Navigation                 | Basic                | Advanced                       |
| Development Features            | Basic                | Extensive                      |
| Availability                    | Common on Linux/Unix | Common on modern Linux systems |

### Check Which Editor is Installed

```bash
vi --version
vim --version
```

### Open a File with Vim

```bash
vim config.yaml
```

If `vim` is not installed, it can usually be installed using the system package manager.

**Debian / Ubuntu:**

```bash
sudo apt update
sudo apt install vim
```

**RHEL / CentOS / Rocky / AlmaLinux:**

```bash
sudo dnf install vim
```

---

## 📌 Modes in VI Editor

VI works primarily through different modes, where each mode has a specific purpose.

| Mode             | Purpose                                         | How to Enter         | How to Exit     |
| ---------------- | ----------------------------------------------- | -------------------- | --------------- |
| **Normal Mode**  | Navigation and command execution                | `Esc`                | —               |
| **Insert Mode**  | Creating and editing text                       | `i`, `a`, `o` etc.   | `Esc`           |
| **Command Mode** | Saving, quitting, searching, and other commands | `:` from Normal Mode | `Enter` / `Esc` |

### Normal Mode

The default mode when you open a file. It is mainly used for navigation and executing editing commands.

### Insert Mode

Used to enter or modify text.

* Press `i` to enter Insert Mode.
* Press `Esc` to return to Normal Mode.

### Command Mode

Used for operations such as saving, quitting, searching, and replacing text.

* Press `:` from Normal Mode to enter Command Mode.

### 🎯 Vim Additional Mode — Visual Mode

Vim also provides **Visual Mode**, which makes selecting and manipulating blocks of text easier.

| Command    | Description                     |
| ---------- | ------------------------------- |
| `v`        | Select characters               |
| `V`        | Select entire lines             |
| `Ctrl + v` | Select a rectangular/block area |
| `Esc`      | Exit Visual Mode                |

Example:

```text
v        → Start character selection
V        → Select entire lines
Ctrl+v   → Start block selection
```

---

## 🧭 Basic Navigation

Navigation commands allow you to move quickly through a file without using the mouse.

| Command | Description                                           |
| ------- | ----------------------------------------------------- |
| `h`     | Move **left**                                         |
| `l`     | Move **right**                                        |
| `j`     | Move **down**                                         |
| `k`     | Move **up**                                           |
| `0`     | Move to the **beginning** of the line                 |
| `^`     | Move to the **first non-blank** character of the line |
| `$`     | Move to the **end** of the line                       |
| `w`     | Move to the **next word**                             |
| `b`     | Move to the **previous word**                         |
| `gg`    | Move to the **start** of the file                     |
| `G`     | Move to the **end** of the file                       |
| `:n`    | Move to **line number `n`**                           |

### Example

```bash
vi config.yaml
```

Or with Vim:

```bash
vim config.yaml
```

Then use:

```text
gg      → Go to beginning of file
G       → Go to end of file
10G     → Go to line 10
```

### 💡 Useful Vim Navigation

Vim provides additional navigation shortcuts that are useful when working with source code and configuration files.

```text
Ctrl + f   → Move one screen forward
Ctrl + b   → Move one screen backward
Ctrl + d   → Move half screen down
Ctrl + u   → Move half screen up
```

---

## ✍️ Insert Mode Shortcuts

Use these commands to start inserting or editing text.

| Command | Description                         |
| ------- | ----------------------------------- |
| `i`     | Insert before cursor                |
| `I`     | Insert at the beginning of the line |
| `a`     | Append after cursor                 |
| `A`     | Append at the end of the line       |
| `o`     | Open a new line below               |
| `O`     | Open a new line above               |
| `Esc`   | Exit Insert Mode                    |

---

## ✂️ Editing Text

VI provides powerful commands for deleting, copying, pasting, and undoing changes.

| Command    | Description                                 |
| ---------- | ------------------------------------------- |
| `x`        | Delete a **character**                      |
| `X`        | Delete a **character before cursor**        |
| `dw`       | Delete a **word**                           |
| `dd`       | Delete a **line**                           |
| `d$`       | Delete from **cursor to end of line**       |
| `d0`       | Delete from **cursor to beginning of line** |
| `D`        | Delete from **cursor to end of line**       |
| `u`        | **Undo** last action                        |
| `Ctrl + r` | **Redo** an undone change                   |
| `yy`       | Copy (yank) a **line**                      |
| `yw`       | Copy (yank) a **word**                      |
| `p`        | Paste **after** the cursor                  |
| `P`        | Paste **before** the cursor                 |

### Common Examples

```text
dd      → Delete current line
yy      → Copy current line
p       → Paste copied content
u       → Undo
Ctrl+r  → Redo
```

### 💡 Useful Vim Editing Commands

Vim provides additional commands that make repetitive editing faster.

| Command | Description                              |
| ------- | ---------------------------------------- |
| `D`     | Delete from cursor to end of line        |
| `C`     | Change text from cursor to end of line   |
| `r`     | Replace a single character               |
| `R`     | Enter Replace Mode                       |
| `J`     | Join the current line with the next line |
| `.`     | Repeat the last change                   |

The `.` command is particularly useful in Vim when the same editing operation needs to be repeated multiple times.

---

## 🔍 Search and Replace

VI provides powerful search and replacement capabilities, which are especially useful when working with configuration files and scripts.

| Command         | Description                                     |
| --------------- | ----------------------------------------------- |
| `/pattern`      | Search **forward** for a pattern                |
| `?pattern`      | Search **backward** for a pattern               |
| `n`             | Repeat last search **forward**                  |
| `N`             | Repeat last search **backward**                 |
| `:%s/old/new/g` | Replace **all occurrences** of `old` with `new` |
| `:s/old/new/g`  | Replace **all occurrences** in the current line |

### Example

Replace all occurrences of `http` with `https`:

```vim
:%s/http/https/g
```

Search for `server`:

```vim
/server
```

### 💡 Vim Search Features

Vim also provides useful search options for development and configuration work.

```vim
:set hlsearch
```

Highlights search results.

```vim
:set nohlsearch
```

Removes search highlighting.

Search for the word under the cursor:

```text
*
```

Search backward for the word under the cursor:

```text
#
```

---

## 📂 Working with Multiple Files

VI can open and manage multiple files using commands and split windows.

| Command            | Description                                         |
| ------------------ | --------------------------------------------------- |
| `:e filename`      | Open a **new file**                                 |
| `:w`               | Save file                                           |
| `:wq`              | Save and exit                                       |
| `:q!`              | Quit **without saving**                             |
| `:split filename`  | Split screen **horizontally** and open another file |
| `:vsplit filename` | Split screen **vertically**                         |
| `Ctrl + w + w`     | Switch between split screens                        |

### Example

Open another file:

```vim
:e config.yaml
```

Create a horizontal split:

```vim
:split nginx.conf
```

Create a vertical split:

```vim
:vsplit docker-compose.yml
```

Switch between split windows:

```text
Ctrl + w + w
```

### 💡 Vim Multiple-Window Features

Vim provides more flexible window navigation.

```text
Ctrl + w + h   → Move to left window
Ctrl + w + l   → Move to right window
Ctrl + w + j   → Move to lower window
Ctrl + w + k   → Move to upper window
```

Vim can also work with multiple files using **buffers**.

```vim
:buffers
```

Shows currently opened buffers.

```vim
:bnext
```

Move to the next buffer.

```vim
:bprev
```

Move to the previous buffer.

---

## 🖥️ Vim Syntax Highlighting

One of Vim's major advantages over traditional `vi` is **syntax highlighting**.

Syntax highlighting makes configuration files, scripts, YAML files, and source code easier to read.

Enable syntax highlighting:

```vim
:syntax on
```

Disable syntax highlighting:

```vim
:syntax off
```

For example, when editing:

```bash
vim docker-compose.yml
```

Vim can visually distinguish YAML keys, values, comments, strings, and other syntax elements.

---

## ⚙️ Vim Configuration

Vim can be customized using the user's Vim configuration file:

```text
~/.vimrc
```

Example:

```vim
syntax on
set number
set autoindent
set tabstop=4
set shiftwidth=4
```

### Common Settings

| Setting            | Purpose                            |
| ------------------ | ---------------------------------- |
| `syntax on`        | Enable syntax highlighting         |
| `set number`       | Show line numbers                  |
| `set autoindent`   | Automatically maintain indentation |
| `set tabstop=4`    | Set tab width                      |
| `set shiftwidth=4` | Set indentation width              |

> **Note:** Vim configuration allows users to customize the editor according to their development and DevOps workflow.

---

## 🚀 VI Commands Useful for DevOps

VI is frequently used in DevOps environments because engineers often work directly with Linux servers through SSH.

Common files edited with `vi` include:

```text
/etc/hosts
/etc/ssh/sshd_config
/etc/nginx/nginx.conf
/etc/fstab
/etc/environment
Dockerfile
Jenkinsfile
*.yaml
*.yml
*.sh
```

### Typical DevOps Workflow

```text
SSH into Linux Server
        ↓
Open Configuration File
        ↓
       vi / vim
        ↓
Edit Configuration
        ↓
Save Changes
        ↓
Restart / Reload Service
```

---

## 🔧 Vim in DevOps & Cloud

Vim is particularly useful when working with:

* 🐧 Linux servers
* ☁️ AWS / Azure / GCP virtual machines
* 🐳 Docker containers
* ⚙️ Kubernetes configuration files
* 🔄 CI/CD pipelines
* 🔨 Jenkinsfiles
* 🌐 Nginx configuration
* 📜 Shell scripts
* 📝 YAML files
* 🏗️ Infrastructure-as-Code files

Example:

```bash
vim deployment.yaml
```

Edit the configuration:

```yaml
apiVersion: apps/v1
kind: Deployment
```

Save and exit:

```vim
:wq
```

Then apply the configuration:

```bash
kubectl apply -f deployment.yaml
```

---

## 💡 Quick VI & Vim Cheat Sheet

```text
┌─────────────────────────────────────────────┐
│        vi / vim QUICK CHEAT SHEET           │
├─────────────────────────────────────────────┤
│ i       → Insert                            │
│ Esc     → Normal Mode                       │
│ :w      → Save                              │
│ :wq     → Save & Exit                       │
│ :q!     → Exit without saving               │
│ dd      → Delete line                       │
│ yy      → Copy line                         │
│ p       → Paste                             │
│ u       → Undo                              │
│ Ctrl+r  → Redo                              │
│ /text   → Search                            │
│ gg      → Start of file                     │
│ G       → End of file                       │
│ :n      → Go to line n                      │
│ v       → Visual character selection        │
│ V       → Visual line selection             │
│ Ctrl+v  → Visual block selection            │
│ .       → Repeat last change                │
│ *       → Search word under cursor          │
│ :set nu → Show line numbers                 │
└─────────────────────────────────────────────┘
```

---

## 🎯 Why VI & Vim are Important for DevOps

Learning VI/Vim is valuable for DevOps and Cloud engineers because Linux servers are often managed through terminal-based SSH sessions where graphical editors may not be available.

**Key benefits:**

* 🐧 Works on almost every Linux distribution
* 🔐 Useful when managing remote servers via SSH
* ⚙️ Ideal for editing configuration files
* 🚀 Fast once the commands are familiar
* 📦 Commonly available in cloud and container environments
* 🛠️ Useful for troubleshooting and system administration
* 🎨 Vim provides syntax highlighting and customization
* 🪟 Vim supports advanced split-window and multi-file editing
* 🔄 Useful for CI/CD, Kubernetes, Docker, and automation files
* 💻 Helps engineers work efficiently in terminal-only environments

> **Tip:** Beginners should first become comfortable with **Normal Mode, Insert Mode, navigation, saving, quitting, and basic editing commands** before learning advanced VI/Vim features.

### ⭐ Recommended Learning Path

```text
VI Fundamentals
      ↓
Modes & Navigation
      ↓
Editing Commands
      ↓
Search & Replace
      ↓
Multiple Files & Windows
      ↓
Vim Features
      ↓
Vim Configuration
      ↓
DevOps / Cloud Usage
```

> **Key Takeaway:**
> `vi` is an essential Linux skill, while `vim` builds on the same foundation and provides a richer editing experience. For DevOps engineers, knowing both makes it easier to manage servers, configurations, scripts, YAML files, and automation directly from the terminal.
