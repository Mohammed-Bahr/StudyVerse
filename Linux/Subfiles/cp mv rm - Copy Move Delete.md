---
created: 2026-04-10 21:47
tags:
  - Devops
  - linux
source:
---

> [!quote] I have an everyday religion that works for me. Love yourself first, and everything else falls into line.
> — Lucille Ball

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


Commands for copying, moving/renaming, and deleting files and directories.

---

## 📋 `cp` — Copy Files and Directories

Copy files or directories from one location to another.

### Basic Syntax

```Bash
cp [options] source destination
```

### Copying Files

```Bash
cp file.txt /path/to/destination/
cp file1.txt file2.txt /path/to/destination/  # Copy multiple files
```

### Copying Directories

```Bash
cp -r folder/ /path/to/destination/  # Use -r for directories
```

### Important Options

|Option|Purpose|Example|
|---|---|---|
|`-i`|Interactive - ask before overwriting|`cp -i file.txt dest/`|
|`-r`|Recursive - copy directories and contents|`cp -r folder/ dest/`|
|`-v`|Verbose - show what's being copied|`cp -v file.txt dest/`|
|`-p`|Preserve attributes (permissions, timestamps)|`cp -p file.txt dest/`|
|`-u`|Update - copy only if source is newer|`cp -u file.txt dest/`|

### Examples

```Bash
# Copy single file
cp document.txt backup/

# Copy directory recursively
cp -r project/ backup/project_backup/

# Copy with confirmation
cp -i important.txt /backup/

# Copy preserving attributes
cp -p config.conf /etc/app/

# Copy only if newer
cp -u app.py /production/
```

> [!warning] Important
> Without `-r`, `cp` will NOT copy directories. It will give an error.

---

## 📋 `mv` — Move or Rename Files/Directories

Move files to different locations OR rename them.

### Basic Syntax

```Bash
mv [options] source destination
```

### Moving Files

```Bash
mv file.txt /path/to/destination/
mv file1.txt file2.txt /path/to/destination/  # Move multiple files
```

### Renaming Files

```Bash
mv oldname.txt newname.txt
mv folder_oldname/ folder_newname/
```

### Important Options

|Option|Purpose|Example|
|---|---|---|
|`-i`|Interactive - ask before overwriting|`mv -i file.txt dest/`|
|`-v`|Verbose - show what's being moved|`mv -v file.txt dest/`|
|`-u`|Update - move only if source is newer|`mv -u file.txt dest/`|
|`-n`|No clobber - don't overwrite existing|`mv -n file.txt dest/`|

### Examples

```Bash
# Move file to another directory
mv document.txt /home/user/documents/

# Rename a file
mv draft.txt final.txt

# Move and rename in one step
mv /tmp/config.json /home/user/myapp/settings.json

# Move directory
mv old_project/ archive/old_project/

# Interactive move (safer)
mv -i important.txt /backup/
```

> [!tip] Moving vs Copying
> - `mv` moves the file (original is removed from source)
> - `cp` makes a copy (original stays in place)

---

## 📋 `rm` — Remove Files and Directories

Delete files and directories.

### Basic Syntax

```Bash
rm [options] filename
```

> [!danger] Warning
> **There is no recycle bin in Linux!** Once deleted with `rm`, files cannot be recovered.

### Removing Files

```Bash
rm file.txt              # Remove single file
rm file1.txt file2.txt   # Remove multiple files
rm *.log                 # Remove all .log files (use carefully!)
```

### Removing Directories

```Bash
rm -r folder/            # Remove directory and contents
rm -rf folder/           # Remove directory forcefully (no confirmation)
rmdir empty_folder/     # Remove empty directory only
```

### Important Options

|Option|Purpose|Example|
|---|---|---|
|`-r`|Recursive - remove directories and contents|`rm -r folder/`|
|`-f`|Force - no confirmation prompts|`rm -f file.txt`|
|`-i`|Interactive - ask before each deletion|`rm -i file.txt`|
|`-v`|Verbose - show what's being removed|`rm -v file.txt`|

### Examples

```Bash
# Remove single file
rm unwanted.txt

# Remove directory and all contents
rm -r project_old/

# Force remove (dangerous!)
rm -rf unwanted_folder/

# Interactive removal (safer)
rm -i *.txt

# Remove all files in current directory (very dangerous!)
rm *

# Remove files with confirmation
rm -i *
```

---

## 📋 `rmdir` — Remove Empty Directories

Only removes directories that are completely empty.

### Usage

```Bash
rmdir empty_folder/
```

### Difference from `rm -r`

|Command|What it does|
|---|---|
|`rmdir folder/`|Removes ONLY if empty; fails otherwise|
|`rm -r folder/`|Removes folder and ALL contents|

### Example

```Bash
# Create and remove empty directory
mkdir temp
rmdir temp

# This will FAIL if directory has files
rmdir documents/  # Error: Directory not empty
```

---

## 📊 Quick Reference Table

|Command|Purpose|Example|
|---|---|---|
|`cp file dest/`|Copy file|`cp data.txt backup/`|
|`cp -r folder/ dest/`|Copy directory|`cp -r project/ backup/`|
|`mv file dest/`|Move file|`mv data.txt archive/`|
|`mv old new`|Rename file|`mv draft.txt final.txt`|
|`rm file`|Delete file|`rm unwanted.txt`|
|`rm -r folder/`|Delete directory|`rm -r old_project/`|
|`rm -rf folder/`|Force delete (dangerous!)|`rm -rf temp/`|
|`rmdir folder/`|Remove empty directory|`rmdir empty/`|

---

## ⚠️ Safety Tips

> [!warning] Danger Zone
> - **Never run `rm -rf /`** — This deletes your entire system
> - **Always double-check before pressing Enter**
> - **Use `-i` flag for important files**
> - **Consider using `trash-cli` instead of `rm` for safety**

### Safe Deletion Practices

```Bash
# Use -i for safety
rm -i important_file.txt

# Check what will be deleted first
ls *.tmp
# Then delete
rm *.tmp

# Create alias for safer rm (add to .bashrc)
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
```

---

## 🔗 Related Notes

- [[ls - List Files]]
- [[cd pwd mkdir touch - Navigation and Creation]]
- [[Linux Filesystem Structure]]
- [[Linux File Permissions]]