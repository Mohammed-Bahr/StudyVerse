---
created: 2026-04-10 21:51
tags:
  - Devops
  - linux
source:
---
> [!quote] If facts are the seeds that later produce knowledge and wisdom, then the emotions and the impressions of the senses are the fertile soil in which the seeds must grow.
> — Rachel Carson

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


Two useful utility commands for searching files and counting text.

---

## 🔍 `find` — Search for Files

Powerful command to search for files and directories based on various criteria.

### Basic Syntax

```Bash
find [path] [options] [expression]
```

### Search by Name

```Bash
find /home -name "file.txt"          # By exact name
find . -name "*.log"               # All .log files
find . -iname "FILE.TXT"          # Case-insensitive
```

### Search by Type

```Bash
find /var -type f                 # Files only
find /etc -type d                 # Directories only
find . -type l                    # Symbolic links only
```

### Search by Size

```Bash
find / -size +100M               # Larger than 100MB
find . -size -1k                # Smaller than 1KB
find . -size +1G -size -5G       # Between 1GB and 5GB
```

### Search by Modification Time

```Bash
find /home -mtime -7              # Modified in last 7 days
find /tmp -mtime +30             # Modified more than 30 days ago
find . -mmin -30                # Modified in last 30 minutes
```

### Search by Permissions

```Bash
find . -perm 755                # Exact permissions
find . -perm -u+w               # User has write permission
```

### Search by Owner

```Bash
find /var -user www-data         # Owned by www-data
find . -group developers       # Owned by developers group
```

### Execute Commands on Results

```Bash
# Find and delete
find . -name "*.tmp" -delete

# Find and change permissions
find . -type f -exec chmod 644 {} \;

# Find and show details
find . -name "*.py" -exec ls -l {} \;
```

---

## 📊 `wc` — Word Count

Counts **lines, words, and characters** in files or input.

### Basic Syntax

```Bash
wc [options] [file]
```

### Options

|Option|Purpose|
|---|---|
|`-l`|Count lines only|
|`-w`|Count words only|
|`-c`|Count bytes|
|`-m`|Count characters|
|`-L`|Length of longest line|

### Examples

```Bash
# Count everything
wc file.txt
# Output: 15  120  850 file.txt
#        (lines, words, characters)

# Count lines only
wc -l file.txt

# Count words from multiple files
wc -w file1.txt file2.txt

# Count lines from command output
ls -l | wc -l
```

### Use Cases

```Bash
# Count files in directory
ls | wc -l

# Count processes
ps aux | wc -l

# Count error lines in log
grep "ERROR" app.log | wc -l

# Find longest line
wc -L file.txt
```

---

## 📊 Quick Reference Table

|Command|Purpose|
|---|---|
|`find . -name "*.txt"`|Find .txt files|
|`find . -type d`|Find directories|
|`find . -size +100M`|Find large files|
|`find . -mtime -7`|Find recent files|
|`wc -l file.txt`|Count lines|
|`wc -w file.txt`|Count words|
|`ls \| wc -l`|Count files|

---
