---
created: 2026-04-10 21:44
tags:
  - Devops
  - linux
source:
---
> [!quote] Time is the wisest counsellor of all.
> — Pericles

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


The `ls` command lists files and directories in the current or specified directory. It's one of the most frequently used Linux commands.

---

## 📋 Basic Usage

```Bash
ls              # List files in current directory
ls /path/to/dir # List files in specific directory
```

---

## 🔧 Common Options

### Display Format

|Option|Description|Example|
|---|---|---|
|`-l`|Long format with details|`ls -l`|
|`-h`|Human-readable sizes (KB, MB, GB)|`ls -lh`|
|`-a`|Show hidden files (starting with `.`)|`ls -a`|
|`-la`|Long format + hidden files|`ls -la`|

### Sorting

|Option|Description|Example|
|---|---|---|
|`-S`|Sort by file size (large to small)|`ls -lS`|
|`-t`|Sort by modification time (newest first)|`ls -lt`|
|`-r`|Reverse order|`ls -lr`|

### Recursive Search

|Option|Description|Example|
|---|---|---|
|`-R`|List recursively (all subdirectories)|`ls -R`|
|`-d */`|Show only directories|`ls -d */`|

---

## 📖 Understanding `ls -l` Output

```Bash
ls -l
```

**Output:**

```Bash
-rw-r--r--  1  user  group  4096  Jan 10 12:30  file.txt
││││││││ │  │    │      │       │          └── File name
││││││││ │  │    │      │       └───────────── Date and time
││││││││ │  │    │      └───────────────────── File size (bytes)
││││││││ │  │    └──────────────────────────── Group owner
││││││││ │  └───────────────────────────────── User (owner)
││││││││ └──────────────────────────────────── Number of links
└┴┴┴┴┴┴┴────────────────────────────────────── File type + permissions
```

### Breaking Down Permissions

**File type (first character):**
- `-` = regular file
- `d` = directory
- `l` = symbolic link

**Permissions (next 9 characters):**
- Characters 1-3: Owner permissions
- Characters 4-6: Group permissions
- Characters 7-9: Others permissions

Example: `rw-r--r--`

|Category|Permission|Meaning|
|---|---|---|
|Owner|`rw-`|Read + Write|
|Group|`r--`|Read only|
|Others|`r--`|Read only|

> [!tip] See [[Linux File Permissions]] for detailed permission explanation.

---

## ⭐ Most Useful Combinations

### `ls -lah`
Everything you usually need:
- Long format (details)
- Human-readable sizes
- Includes hidden files

### `ls -lt`
Sort by newest files (most recent first).

### `ls -laS`
Sort by size (largest first), including hidden files.

### `ls -d */`
Show only directories in current location.

---

## 🔍 Practical Examples

```Bash
# List all files in detailed format with human-readable sizes
ls -lah

# List only hidden files
ls -la | grep "^\."

# List files sorted by size
ls -lS

# List files sorted by modification time
ls -lt

# List specific file details
ls -l filename.txt

# List all Python files
ls *.py

# List files with specific permission
ls -la | grep "rwx"

# Count files in directory
ls -1 | wc -l
```

---

## 📊 Quick Reference Table

|Command|Purpose|
|---|---|
|`ls`|List files|
|`ls -l`|Detailed list|
|`ls -a`|Show hidden files|
|`ls -h`|Human-readable sizes|
|`ls -lah`|All details + hidden + readable|
|`ls -R`|Recursive listing|
|`ls -S`|Sort by size|
|`ls -t`|Sort by time|
|`ls -d */`|List directories only|

---

