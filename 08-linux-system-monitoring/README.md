<img width="1536" height="1024" alt="00c9eb1d-4355-49de-986e-bea9c8066d85" src="https://github.com/user-attachments/assets/a0195d30-6ab3-43a4-a46d-4e64edf575ac" />



# 📊 Linux System Monitoring 👨🏻‍💻

## 1. 💻 Introduction to System Monitoring

Monitoring system resources is essential to ensure optimal performance, detect issues, and troubleshoot problems in Linux.

In real-world **DevOps, Cloud, SRE, and production environments**, system monitoring helps teams identify:

* 🔥 High CPU utilization
* 🧠 Memory exhaustion
* 💾 Disk space issues
* ⚙️ Disk I/O bottlenecks
* 🌐 Network connectivity problems
* 🔌 Open or unexpected ports
* 🚨 Application and system failures
* 🖥️ Kernel-level issues

Various tools allow us to monitor **CPU, memory, disk usage, network activity, and running processes**.

---

# 2. 📚 Index of Commands Covered

## 2.1 🧠 CPU and Memory Monitoring

| No. | Command   | Purpose                                               |
| --- | --------- | ----------------------------------------------------- |
| 1️⃣ | `top`     | 🔄 Real-time system monitoring                        |
| 2️⃣ | `htop`    | 📊 Interactive process viewer (requires installation) |
| 3️⃣ | `vmstat`  | ⚙️ Report system performance statistics               |
| 4️⃣ | `free -m` | 🧠 Show memory usage                                  |

## 2.2 💾 Disk Monitoring

| No. | Command        | Purpose                                    |
| --- | -------------- | ------------------------------------------ |
| 5️⃣ | `df -h`        | 💽 Check disk space usage                  |
| 6️⃣ | `du -sh /path` | 📁 Show disk usage of a specific directory |
| 7️⃣ | `iostat`       | ⚙️ Display CPU and disk I/O statistics     |

## 2.3 🌐 Network Monitoring

| No.    | Command               | Purpose                                             |
| ------ | --------------------- | --------------------------------------------------- |
| 8️⃣    | `ifconfig`            | 🔌 Show network interfaces (deprecated, use `ip a`) |
| 9️⃣    | `ip a`                | 🌐 Show network interface details                   |
| 🔟     | `netstat -tulnp`      | 🔎 Show active connections and listening ports      |
| 1️⃣1️⃣ | `ss -tulnp`           | 🔌 Alternative to `netstat` for socket statistics   |
| 1️⃣2️⃣ | `ping hostname`       | 📡 Test network connectivity                        |
| 1️⃣3️⃣ | `traceroute hostname` | 🛣️ Show network path to a host                     |
| 1️⃣4️⃣ | `nslookup domain`     | 🌍 Get DNS resolution details                       |

## 2.4 📝 Log Monitoring

| No.    | Command                   | Purpose                                       |
| ------ | ------------------------- | --------------------------------------------- |
| 1️⃣5️⃣ | `tail -f /var/log/syslog` | 📜 Live monitoring of system logs             |
| 1️⃣6️⃣ | `journalctl -f`           | 📖 Live system logs for systemd-based distros |
| 1️⃣7️⃣ | `dmesg \| tail`           | 🖥️ View kernel logs                          |

---

# 3. 💻⚙️ CPU and Memory Monitoring

CPU and memory monitoring is one of the first steps when troubleshooting a slow or unresponsive Linux server.

---

## 3.1 🔄 Using `top`

To view real-time CPU and memory usage:

```bash
top
```

Press `q` to quit.

### 🔍 What `top` Helps You Monitor

* 🔥 CPU utilization
* 🧠 Memory utilization
* ⚙️ Running processes
* 🆔 Process IDs (PIDs)
* 📈 Load average
* ⏱️ System uptime
* 📊 Individual process resource consumption

### 🏢 Real-World Company Use Case

**Production Web Server**

Suppose a company's production application suddenly becomes slow.

A DevOps engineer connects to the Linux server and runs:

```bash
top
```

They may discover that a Java, Node.js, Python, or Nginx-related process is consuming unusually high CPU.

The engineer can then investigate the process and determine whether the problem is caused by:

* 📈 Application traffic
* 🧠 Memory pressure
* ⚠️ A runaway process
* 🐛 An application bug
* 🔄 Unexpected workload

---

## 3.2 📊 Using `htop`

A user-friendly alternative:

```bash
htop
```

Use arrow keys to navigate and `F9` to kill processes.

> ⚠️ **Note:** `htop` may need to be installed separately depending on the Linux distribution.

### 🏢 Real-World Company Use Case

**Application Server Troubleshooting**

A company operates multiple application servers behind a load balancer.

One server starts responding slowly.

An engineer runs:

```bash
htop
```

They can quickly identify:

* 🔥 Which process consumes the most CPU
* 🧠 Which process consumes the most memory
* ⚙️ Number of running processes
* 🆔 Process IDs

This provides a quick starting point before deeper investigation.

---

## 3.3 ⚙️ Using `vmstat`

To check CPU, memory, and I/O stats:

```bash
vmstat 1 5  # Update every 1 sec, show 5 updates
```

`vmstat` is useful when engineers need a quick overview of system performance.

It can provide information about:

* ⚙️ Processes
* 🧠 Memory
* 🔄 Paging
* 💽 Block I/O
* 🔥 CPU activity

### 🏢 Real-World Company Use Case

**Production Performance Investigation**

A backend service is experiencing intermittent performance issues.

Instead of monitoring only CPU, an engineer uses:

```bash
vmstat 1 5
```

This helps determine whether the server is experiencing:

* 🔥 CPU pressure
* 🧠 Memory pressure
* 🔄 Excessive swapping
* 💽 I/O activity

This is particularly useful during incident troubleshooting.

---

## 3.4 🧠 Checking Memory Usage

```bash
free -m
```

Shows free and used memory in megabytes.

### 🏢 Real-World Company Use Case

**Memory Exhaustion**

A production application starts getting killed unexpectedly.

The DevOps engineer checks:

```bash
free -m
```

If available memory is extremely low and swap usage is increasing, the team can investigate:

* 🐛 Memory leaks
* ⚙️ Incorrect application configuration
* 🖥️ Insufficient server capacity
* 📈 Unexpected traffic

This can help prevent **Out-Of-Memory (OOM)** incidents.

---

# 4. 💾 Disk Monitoring

Disk monitoring is critical because a server can become unstable when important filesystems reach their capacity.

---

## 4.1 💽 Using `df`

Check available disk space:

```bash
df -h
```

The `-h` option displays values in a human-readable format.

### 🏢 Real-World Company Use Case

**Production Server Disk Full**

A company's application suddenly stops writing files.

An engineer checks:

```bash
df -h
```

They discover that `/var` or `/` is almost 100% full.

Possible causes may include:

* 📝 Application logs
* 🗑️ Temporary files
* 💾 Old backups
* 🐳 Docker images/containers
* 🗄️ Large database files

The engineer can then identify and safely clean up unnecessary data.

---

## 4.2 📁 Using `du`

Find the size of a directory:

```bash
du -sh /var/log
```

### 🏢 Real-World Company Use Case

**Finding What Consumes Disk Space**

Suppose:

```bash
df -h
```

shows that the disk is almost full.

The engineer can investigate specific directories:

```bash
du -sh /var/log
```

If `/var/log` is unusually large, they can investigate individual log files and determine whether log rotation or centralized logging needs attention.

---

## 4.3 ⚙️ Using `iostat`

Check disk and CPU usage:

```bash
iostat
```

`iostat` is useful for identifying potential CPU and disk I/O bottlenecks.

### 🏢 Real-World Company Use Case

**Database Performance Issue**

A production database becomes slow even though CPU utilization is not extremely high.

An engineer checks:

```bash
iostat
```

High disk utilization or I/O wait may indicate that storage performance is becoming a bottleneck.

The team can then investigate:

* 💽 Disk performance
* 🗄️ Database queries
* ⚙️ Storage configuration
* 📈 Application workload
* 🔄 I/O-intensive processes

---

# 5. 🌐 Network Monitoring

Network monitoring helps engineers troubleshoot connectivity, DNS, routing, ports, and application communication.

---

## 5.1 🔌 Checking Network Interfaces

```bash
ip a  # Show IP addresses and interfaces
```

### 🏢 Real-World Company Use Case

**Cloud Server Network Troubleshooting**

A newly launched Linux server cannot communicate with another service.

An engineer checks:

```bash
ip a
```

They can verify:

* 🔌 Network interfaces
* 🌐 Assigned IP addresses
* 🟢 Interface state
* 🌍 IPv4/IPv6 configuration

This is commonly used during troubleshooting of **AWS EC2, Azure VM, and on-premises Linux servers**.

---

## 5.2 🔎 Viewing Open Ports and Connections

```bash
netstat -tulnp  # Show listening ports
ss -tulnp  # Alternative to netstat
```

`ss` is commonly preferred on modern Linux systems.

### 🏢 Real-World Company Use Case

**Application Port Troubleshooting**

Suppose an application is expected to listen on port `8080`, but users cannot access it.

An engineer checks:

```bash
ss -tulnp
```

They can verify whether the application is actually listening on the expected port.

For example:

```text
LISTEN  0  128  0.0.0.0:8080  0.0.0.0:*
```

If the port is not listening, the engineer can investigate the application or service configuration.

This is especially useful when troubleshooting:

* 🌐 Nginx
* 🌐 Apache
* 🔧 Jenkins
* 🐳 Docker applications
* ☸️ Kubernetes services
* ☕ Java applications

---

## 5.3 📡 Testing Connectivity

```bash
ping google.com  # Test internet connection
traceroute google.com  # Trace the path to Google
```

### 🏢 Real-World Company Use Case

**Server Connectivity Issue**

An application server cannot reach an external API.

An engineer may first test:

```bash
ping google.com
```

Then investigate the network path:

```bash
traceroute google.com
```

This helps determine whether the issue may involve:

* 🌐 Network connectivity
* 🛣️ Routing
* 🔥 Firewall rules
* 🚪 Gateway configuration
* 🛣️ Network path problems

> ⚠️ **Note:** Some servers or networks block ICMP, so a failed `ping` does not always mean that the destination is unreachable.

---

## 5.4 🌍 Checking DNS Resolution

```bash
nslookup example.com
```

### 🏢 Real-World Company Use Case

**Application Cannot Resolve a Domain**

Suppose an application cannot connect to:

```text
api.company.com
```

An engineer checks:

```bash
nslookup api.company.com
```

This helps determine whether DNS is returning the expected IP address.

DNS troubleshooting is important when working with:

* ☁️ Cloud applications
* ⚖️ Load balancers
* 🌐 Route 53
* 🏢 Internal DNS
* 🔗 Microservices
* 🌍 External APIs

---

# 6. 📝 Log Monitoring

Logs provide valuable information about system events, application failures, authentication problems, and infrastructure issues.

---

## 6.1 📜 Live Monitoring of System Logs

```bash
tail -f /var/log/syslog  # Follow logs in real-time
journalctl -f  # Systemd logs
```

### 🏢 Real-World Company Use Case

**Production Application Deployment**

After deploying a new application version, the DevOps engineer wants to monitor server activity in real time.

They can use:

```bash
journalctl -f
```

or:

```bash
tail -f /var/log/syslog
```

This allows the engineer to immediately notice:

* ❌ Service failures
* 🔐 Permission errors
* ⚙️ Configuration errors
* 🌐 Network errors
* ⚠️ Unexpected system events

---

## 6.2 🖥️ Checking Kernel Logs

```bash
dmesg | tail
```

### 🏢 Real-World Company Use Case

**Infrastructure-Level Troubleshooting**

If a Linux server experiences unexpected hardware, storage, driver, or kernel-related problems, engineers can inspect kernel messages:

```bash
dmesg | tail
```

This can provide useful clues about:

* 💽 Disk problems
* 🌐 Network interface issues
* 🔧 Drivers
* 🖥️ Kernel events
* ⚠️ Hardware-related errors

---

# 7. 🚨 Real-World Production Monitoring Workflow

In a company environment, engineers rarely use only one command. Multiple commands are often combined during incident troubleshooting.

### 🐌 Example: Production Server Becomes Slow

A DevOps engineer might follow this workflow:

### Step 1️⃣ — 🔥 Check CPU and processes

```bash
top
```

### Step 2️⃣ — 🧠 Check memory

```bash
free -m
```

### Step 3️⃣ — ⚙️ Check system performance

```bash
vmstat 1 5
```

### Step 4️⃣ — 💾 Check disk space

```bash
df -h
```

### Step 5️⃣ — 📁 Find large directories

```bash
du -sh /var/log
```

### Step 6️⃣ — 💽 Check disk I/O

```bash
iostat
```

### Step 7️⃣ — 🌐 Check network interfaces

```bash
ip a
```

### Step 8️⃣ — 🔌 Check listening ports

```bash
ss -tulnp
```

### Step 9️⃣ — 📡 Test connectivity

```bash
ping google.com
```

### Step 🔟 — 🌍 Check DNS

```bash
nslookup example.com
```

### Step 1️⃣1️⃣ — 📝 Monitor logs

```bash
journalctl -f
```

This provides a structured approach to identifying the source of a production issue.

---

# 8. ☁️ Linux Monitoring in DevOps & Cloud

Linux monitoring commands are fundamental skills for:

* 👨🏻‍💻 **DevOps Engineers**
* ☁️ **Cloud Engineers**
* 🖥️ **System Administrators**
* 🚀 **SRE Engineers**
* ⚙️ **Platform Engineers**
* 🏗️ **Infrastructure Engineers**

In modern companies, these commands are often used alongside monitoring platforms such as:

* 🔥 Prometheus
* 📊 Grafana
* ☁️ Amazon CloudWatch
* ☁️ Azure Monitor
* 📈 Datadog
* 📊 New Relic
* 🔎 ELK / Elastic Stack

Command-line monitoring is especially valuable because engineers can use it directly on a server during **production incidents and troubleshooting**.

---

# 9. 📌 Key Takeaways

| Area            | Important Commands               | Primary Purpose               |
| --------------- | -------------------------------- | ----------------------------- |
| 🧠 CPU & Memory | `top`, `htop`, `vmstat`, `free`  | Identify resource pressure    |
| 💾 Disk         | `df`, `du`, `iostat`             | Find storage and I/O problems |
| 🌐 Network      | `ip`, `ss`, `ping`, `traceroute` | Troubleshoot connectivity     |
| 🌍 DNS          | `nslookup`                       | Verify DNS resolution         |
| 📝 Logs         | `tail`, `journalctl`, `dmesg`    | Investigate system events     |

---

## 💡♾️ DevOps Tip 

> 🚨 **When a production server has a problem, don't immediately restart it. First collect evidence.**

Start by checking:

```bash
top
free -m
df -h
iostat
ss -tulnp
journalctl -f
```

Understanding these basic Linux monitoring commands gives you a strong foundation for **production troubleshooting, DevOps, Cloud, and SRE operations**. 🚀
