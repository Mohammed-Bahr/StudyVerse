---
created: 2026-04-10 21:45
tags:
  - Devops
  - linux
source:
---
> [!quote] There is no failure except in no longer trying.
> — Elbert Hubbard

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**



Basic commands for navigating the filesystem and creating files and directories.

---

## 📍 `cd` — Change Directory

Move between directories in the filesystem.

### Basic Usage

```Bash
cd /home/user       # Go to specific directory
cd ..                # Go up one level
cd ../..             # Go up two levels
cd ~                 # Go to home directory
cd -                 # Go to previous directory
cd                   # Go to home directory (same as cd ~)
```

### Useful Shortcuts

| Command    | Action                   |
| ---------- | ------------------------ |
| `cd ~`     | Go to home directory     |
| `cd -`     | Go to previous directory |
| `cd ..`    | Go up one level          |
| `cd ../..` | Go up two levels         |
| `cd /`     | Go to root directory     |

### Examples

```Bash
# Navigate to Documents
cd ~/Documents

# Navigate relative to current location
cd ./projects/web

# Go back to where you were
cd -
```

---

## 📍 `pwd` — Print Working Directory

Shows the full path of your current location in the filesystem.

### Usage

```Bash
pwd
# Output: /home/user/projects
```

### Use Cases

```Bash
# Check current location before running commands
pwd

# Store current directory in a variable
current_dir=$(pwd)

# Use in scripts
echo "Running script from: $(pwd)"
```

---

## 📁 `mkdir` — Make Directory

Create new directories.

### Basic Usage

```Bash
mkdir new_folder           # Create single directory
mkdir dir1 dir2 dir3       # Create multiple directories
```

### Important Options

|Option|Description|Example|
|---|---|---|
|`-p`|Create parent directories as needed|`mkdir -p a/b/c`|
|`-v`|Verbose - show what was created|`mkdir -v new_folder`|
|`-m`|Set permissions while creating|`mkdir -m 755 new_folder`|

### Examples

```Bash
# Create nested directories
mkdir -p project/src/main/python
# Creates: project/ → src/ → main/ → python/

# Create with specific permissions
mkdir -m 700 private_folder

# Create multiple directories at once
mkdir dir1 dir2 dir3

# Create directory with spaces in name
mkdir "My Documents"
```

> [!tip] Use `-p` for Nested Directories
> Always use `mkdir -p` when creating nested directories. It won't error if a parent directory already exists.

---

## 📄 `touch` — Create Empty Files

Create new empty files or update file timestamps.

### Basic Usage

```Bash
touch file.txt        # Create single file
touch file1 file2 file3  # Create multiple files
```

### Timestamps

If the file already exists, `touch` updates its:
- **Access time** (last read)
- **Modification time** (last write)

### Examples

```Bash
# Create empty file
touch notes.txt

# Create multiple files
touch file1.txt file2.txt file3.txt

# Create file with specific extension
touch script.sh

# Update timestamp of existing file
touch existing_file.txt

# Create files in nested directories (must exist)
touch project/src/main.py
```

### Creating Files with Content

```Bash
# Using echo
echo "Initial content" > newfile.txt

# Using cat
cat > newfile.txt <<EOF
Line 1
Line 2
Line 3
EOF

# Using touch for placeholder
touch placeholder.json
```

---

## 📊 Quick Reference Table

|Command|Purpose|Example|
|---|---|---|
|`cd`|Change directory|`cd /home/user`|
|`cd ~`|Go home|`cd ~`|
|`cd -`|Go to previous directory|`cd -`|
|`pwd`|Print working directory|`pwd`|
|`mkdir`|Create directory|`mkdir newfolder`|
|`mkdir -p`|Create nested directories|`mkdir -p a/b/c`|
|`touch`|Create empty file|`touch file.txt`|

---

## 💡 Common Workflows

### Starting a New Project

```Bash
cd ~/projects
mkdir -p myapp/{src,tests,docs}
cd myapp
touch src/main.py tests/test_main.py
ls -R
```

### Navigating Complex Paths

```Bash
# Go to a deep directory
cd /var/log/nginx

# Go back to previous directory
cd -

# Check where you are
pwd

# Go up two levels
cd ../..
pwd
```

---

## 🔗 Related Notes

- [[ls - List Files]]
- [[cp mv rm - Copy Move Delete]]
- [[Linux Filesystem Structure]]