---
created: 2026-04-10 21:51
tags:
  - Devops
  - linux
source:
---
> [!quote] A goal without a plan is just a wish.
> — Larry Elder

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


This is your central hub for all Linux commands. Each note below covers a specific topic in detail.

---

## 📚 Core Concepts

|Topic|Note Location|
|---|---|
|Linux Filesystem Structure|[[Linux Filesystem Structure]]|
|Linux File Permissions|[[Linux File Permissions]]|
|Linux Shell Overview|[[Linux Shell Overview]]|

---

## 📁 File Management Commands

### Navigation & Creation

|Command|Note Location|
|---|---|
|List files (`ls`)|[[ls - List Files]]|
|Navigate, print working directory ([[cd]], [[pwd]])|[[cd pwd mkdir touch - Navigation and Creation]]|
|Create/move/delete files [[cp mv rm]]|[[cp mv rm - Copy Move Delete]]|

### File Viewing

|Command|Note Location|
|---|---|
|View files (`cat`, `less`, `more`, `head`, `tail`)|[[cat less more head tail - File Viewing]]|
|File Globbing (`*`, `?`, `[]`)|[[File Globbing]]|

---

## 🔍 Search & Text Processing

|Command|Note Location|
|---|---|
|`grep` - Search patterns|[[grep]]|
|`sed` - Stream editor|[[sed]]|
|`awk` -Text processing|[[awk]]|
|`find` - Search files|[[find and wc]]|
|`wc` - Word count|[[find and wc]]|

---

## ⚙️ System & Network

|Command|Note Location|
|---|---|
|Process management (`ps`)|[[ps - Process Management]]|
|Network commands (`ip`, `ping`, `netstat`)|[[Network Commands]]|

---

## ✏️ Text Editors

|Command|Note Location|
|---|---|
|`vi` / `vim`|[[vi-vim]]|

---

## 👤 User & Group

|Command|Note Location|
|---|---|
|`id` - User/Group info|[[id - User and Group]]|

---

## 📋 Quick Command Reference

### File Operations

```bash
ls -lah              # List all files with details
cd /path            # Change directory
pwd                 # Print working directory
mkdir -p a/b/c      # Create nested directories
touch file.txt       # Create empty file
cp src dest         # Copy file
mv old new          # Move/rename file
rm -rf dir/        # Force delete directory
```

### File Viewing

```bash
cat file.txt         # Show entire file
head -n 10 file.txt # First 10 lines
tail -n 10 file.txt # Last 10 lines
tail -f log.txt     # Follow log in real-time
less file.txt       # Page through file
```

### Searching

```bash
grep "pattern" file  # Search in file
grep -r "pattern" /dir # Recursive search
find . -name "*.txt" # Find files
sed 's/old/new/g'   # Replace text
awk '{print $1}'    # Print column
```

### Permissions

```bash
chmod +x script.sh   # Make executable
chmod 755 file     # Set permissions
chown user:group file # Change owner
```

### Process Management

```bash
ps aux              # Show all processes
top                 # Real-time monitor
kill PID            # Stop process
kill -9 PID        # Force stop
```

### Network

```bash
ip addr            # Show IPs
ping -c 4 host    # Test connectivity
netstat -tuln      # Show ports
```

---

## 🎯 Related Notes outside Linux

- [[Bash Script]]
- [[Regular Expressions]]

---

## 💡 Learning Path

1. **Start Here:** [[Linux Filesystem Structure]] → [[Linux File Permissions]]
2. **Navigation:** [[ls - List Files]] → [[cd pwd mkdir touch - Navigation and Creation]]
3. **File Ops:** [[cp mv rm - Copy Move Delete]] → [[cat less more head tail - File Viewing]]
4. **Search:** [[grep]] → [[sed]] → [[awk]] → [[find and wc]]
5. **System:** [[ps - Process Management]] → [[Network Commands]]

---

> [!tip] Pro Tip
> Create your own [[Bash Script]] scripts to automate common task combinations!