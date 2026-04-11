---
created: 2026-04-10 21:50
tags:
  - Devops
  - linux
source:
---
> [!quote] Work while you have the light. You are responsible for the talent that has been entrusted to you.
> — Henri-Frederic Amiel

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


The `ps` command is used to **list the processes** currently running on the system. It's essential for system monitoring and debugging.

---

## 📋 What is a Process?

A **process** is a running program. Each process has:
- **PID** (Process ID) - unique identifier
- **User** - who started it
- **CPU/Memory** - resource usage
- **Command** - what program is running

---

## 📋 Basic Usage

### Show Current User's Processes

```Bash
ps
```

**Output:**
```
  PID TTY          TIME CMD
 1234 pts/0    00:00:00 bash
 5678 pts/0    00:00:00 ps
```

---

## 🔧 Show All Processes

```Bash
ps -e      # or ps -A
```

---

## 🔍 `ps aux` — The Most Useful Command

```Bash
ps aux
```

### Output Breakdown

```
USER       PID %CPU %MEM    VSZ   RSS TTY  STAT START   TIME COMMAND
root         1  0.0  0.1 168520 13408 ?    Ss   10:23   0:01 /sbin/init
www-data 1234  2.5  1.3 245680 54320 ?    S    10:45  0:15 /usr/sbin/apache2
```

|Column|Meaning|
|---|---|
|`USER`|Username of process owner|
|`PID`|Process ID (unique)|
|`%CPU`|CPU usage percentage|
|`%MEM`|Memory (RAM) usage|
|`VSZ`|Virtual memory size (KB)|
|`RSS`|Actual physical memory used (KB)|
|`TTY`|Terminal (? = no terminal)|
|`STAT`|Process state|
|`START`|When process started|
|`TIME`|Total CPU time|
|`COMMAND`|Command that started process|

### Process States (STAT)

|State|Meaning|
|---|---|
|`R`|Running|
|`S`|Sleeping (idle)|
|`Z`|Zombie (terminated but not reaped)|
|`T`|Stopped (paused)|
|`D`|Waiting ( uninterruptible)|

---

## 🔧 Breaking Down `aux`

- **`a`** — Show processes for **all users**
- **`u`** — Display in **user-oriented format** (shows user, CPU%, MEM%, etc.)
- **`x`** — Include processes **not attached to a terminal** (background/daemon processes)

---

## 💡 Common Use Cases

### Find a Specific Process

```Bash
# Find nginx processes
ps aux | grep nginx

# Find process by name (more precise)
ps aux | grep "[n]ginx"
```

### Sort by CPU Usage

```Bash
ps aux --sort=-%cpu | head
```

### Sort by Memory Usage

```Bash
ps aux --sort=-%mem | head
```

### Find Process by User

```Bash
ps -u username    # All processes for specific user
ps -U username   # All processes owned by user
```

### Kill a Process

```Bash
kill 1234       # Gracefully stop process 1234
kill -9 1234    # Force kill (if needed)
kill -15 1234   # Same as kill (SIGTERM)
```

### Full Process List with Threads

```Bash
ps -eLf          # Show threads
ps -eF           # Full format
ps auxww         # Wide output (truncates less)
```

---

## 📊 Quick Reference Table

|Command|Purpose|
|---|---|
|`ps`|Current shell processes|
|`ps -e`|All system processes|
|`ps aux`|All processes with details|
|`ps aux --sort=-%cpu`|Sort by CPU (highest first)|
|`ps aux --sort=-%mem`|Sort by memory (highest first)|
|`ps -u username`|Processes for specific user|
|`ps aux \| grep nginx`|Find nginx process|

---

## 🔗 Related Notes

- [[top]] - Real-time process monitor
- [[kill]] - Terminate processes
- [[Bash Script]] - Process management in scripts