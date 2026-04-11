---
created: 2026-04-10 21:41
tags:
  - Devops
  - linux
source:
---
> [!quote] The best way to pay for a lovely moment is to enjoy it.
> — Richard Bach

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


The Linux filesystem is organized as a hierarchical tree structure starting from the root directory (`/`). Understanding this structure is essential for navigating and managing files in Linux.

---

## 🌳 Directory Tree Structure

```c
/ (Root)
├── bin         (Essential User Binaries)
├── boot        (Boot Loader Files)
├── dev         (Device Files)
├── etc         (Configuration Files)
├── home        (User Home Directories)
├── lib         (Essential Shared Libraries)
├── media       (Removable Media)
├── mnt         (Temporary Mount Points)
├── opt         (Optional Add-on Software)
├── proc        (Process Information)
├── root        (Root User Home Directory)
├── run         (Application State Files)
├── sbin        (System Binaries)
├── srv         (Service Data)
├── sys         (System/Kernel Information)
├── tmp         (Temporary Files)
├── usr         (User Utilities & Applications)
│   ├── bin
│   ├── lib
│   └── local
└── var         (Variable Data Files)
    ├── log
    └── mail
```

---

## 📂 Directory Breakdown by Function

### 1. Core System

|Directory|Purpose|
|---|---|
|`/`|The root of everything. System starts here.|
|`/boot`|Files needed to start the system (kernel, GRUB).|
|`/etc`|All system configuration files.|

---

### 2. System Programs

|Directory|Purpose|
|---|---|
|`/bin`|Basic user commands (`ls`, `cp`, `cat`).|
|`/sbin`|Admin system tools (`reboot`, `fdisk`).|
|`/lib`|Libraries needed by programs in `/bin` and `/sbin`.|

---

### 3. User Data

|Directory|Purpose|
|---|---|
|`/home`|Personal files for each user.|
|`/root`|Home directory of the root (admin) user.|

---

### 4. Applications & Installed Software

|Directory|Purpose|
|---|---|
|`/usr`|Most installed programs and libraries (largest directory).|
|`/usr/local`|Software manually installed by you.|

---

### 5. Changing/Temporary Data

|Directory|Purpose|
|---|---|
|`/var`|Data that changes: logs, web server files, printer queues.|
|`/tmp`|Temporary files (auto-deleted on reboot).|

---

### 6. Virtual & Device Files

|Directory|Purpose|
|---|---|
|`/dev`|Hardware represented as files (drives, USB, etc.).|
|`/proc`|Virtual system info (processes, CPU, memory).|
|`/sys`|Interface to the kernel and hardware info.|

---

### 7. Mount Points

|Directory|Purpose|
|---|---|
|`/media`|Where the OS automatically mounts removable media like USB drives or CD-ROMs.|
|`/mnt`|Traditionally used by system admins to manually mount temporary filesystems (like a network drive or a recovery disk).|

---

## 📊 Quick Reference Table

|**Directory**|**Primary Function**|**Analogy**|
|---|---|---|
|`/bin`|Essential commands|Toolbox|
|`/etc`|Configuration|Control Panel|
|`/home`|User data|"My Documents"|
|`/var`|Logs and changing data|Logbook / Journal|
|`/dev`|Hardware devices|Device Manager|
|`/usr`|Applications|"Program Files"|

---
