---
created: 2026-04-10 21:50
tags:
  - Devops
  - linux
source:
---

> [!quote] Winners have simply formed the habit of doing things losers don't like to do.
> — Albert Gray

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**

Commands for viewing the contents of files in different ways.

---

## 📋 `cat` — Display Entire File

Shows the **whole file at once**. Best suited for small files.

### Basic Usage

```Bash
cat file.txt
```

### Options

|Option|Purpose|Example|
|---|---|---|
|`-n`|Number all output lines|`cat -n file.txt`|
|`-b`|Number non-blank lines only|`cat -b file.txt`|
|`-s`|Squeeze multiple blank lines into one|`cat -s file.txt`|
|`-A`|Show hidden characters (tabs, etc.)|`cat -A file.txt`|

### Advanced Uses

```Bash
# Read single file
cat file.txt

# Combine multiple files
cat file1.txt file2.txt > combined.txt

# Create new file (redirect input)
cat > newfile.txt
# Type content, press Ctrl+D to save

# Append to existing file
cat >> file.txt
# Type content, press Ctrl+D to append
```

### With Pipes

```Bash
# Search within file
cat file.txt | grep "search_term"

# Count lines
cat file.txt | wc -l

# Display with line numbers
cat -n file.txt | less
```

> [!tip] Best for Small Files
> `cat` loads the entire file into memory. For large files, use `less` or `more`.

---

## 📋 `less` — View File Page by Page

Allows **forward and backward** scrolling. Best for large files. Also supports search.

### Basic Usage

```Bash
less file.txt
less large_log.txt
```

### Navigation Keys

|Key|Action|
|---|---|
|`Space` or `f`|Page forward|
|`b`|Page backward|
|`Enter`|Line forward|
|`d`|Half page forward|
|`q`|Quit|
|`/`|Search forward|
|`?`|Search backward|
|`n`|Next search result|
|`N`|Previous search result|
|`g`|Go to beginning|
|`G`|Go to end|

### Options

|Option|Purpose|Example|
|---|---|---|
|`-N`|Show line numbers|`less -N file.txt`|
|`-m`|Show more status (percentage)|`less -m file.txt`|
|`-i`|Case-insensitive search|`less -i file.txt`|

### Examples

```Bash
# View with line numbers
less -N file.txt

# Start at specific line
less -n +50 largefile.txt

# Search for pattern
less log.txt
# Then type: /error

# Case-insensitive search
less -i log.txt
```

> [!tip] Best for Large Files
> `less` is preferred over `more` because it allows both forward AND backward navigation.

---

## 📋 `more` — View File Forward Only

Shows file **one screen at a time**. Scrolls forward only.

### Basic Usage

```Bash
more file.txt
```

### Navigation Keys

|Key|Action|
|---|---|
|`Space`|Next page|
|`Enter`|Next line|
|`q`|Quit|
|`b`|Back (go back one page)|

### Difference from `less`

|Feature|`more`|`less`|
|---|---|---|
|Forward scroll|✓|✓|
|Backward scroll|✗|✓|
|Search in file|✗|✓|
|Speed|Slower|Faster (doesn't load entire file)|
|History|NO|YES|

> [!note] Recommendation
> Use `less` instead of `more`. It's more modern and powerful.

---

## 📋 `head` — First Lines

Shows the **first lines** of a file (default: 10 lines).

### Basic Usage

```Bash
head file.txt           # Show first 10 lines
head -n 5 file.txt     # Show first 5 lines
head file1.txt file2.txt # Show first lines of multiple files
```

### Options

|Option|Purpose|Example|
|---|---|---|
|`-n`|Number of lines|`head -n 20 file.txt`|
|`-c`|Number of bytes|`head -c 100 file.txt`|

### Examples

```Bash
# Default (10 lines)
head file.txt

# First 20 lines
head -n 20 file.txt

# First 1KB
head -c 1024 file.txt

# First line only
head -n 1 file.txt

# Multiple files
head file1.txt file2.txt
```

---

## 📋 `tail` — Last Lines

Shows the **last lines** of a file (default: 10 lines).

### Basic Usage

```Bash
tail file.txt           # Show last 10 lines
tail -n 5 file.txt    # Show last 5 lines
```

### Options

|Option|Purpose|Example|
|---|---|---|
|`-n`|Number of lines|`tail -n 20 file.txt`|
|`-f`|Follow live updates (great for logs)|`tail -f app.log`|
|`-F`|Follow and retry on file rotation|`tail -F app.log`|

### Follow Mode (`-f`)

```Bash
# Monitor a log file in real-time
tail -f /var/log/syslog

# Show last 20 lines and follow
tail -n 20 -f app.log

# Follow multiple logs
tail -f /var/log/*.log
```

### Use Cases

```Bash
# See most recent errors
tail -n 100 error.log | grep ERROR

# Monitor server logs
tail -f /var/log/nginx/access.log

# Check end of large file
tail -n 50 hugefile.txt
```

---

## 📊 Quick Reference Table

|Command|Purpose|Best For|
|---|---|---|
|`cat`|Show entire file|Small files, combining files|
|`less`|Page through file|Large files, searching|
|`more`|Page through file|Older systems, simple use|
|`head`|First lines|Quick peek at format|
|`tail`|Last lines, live monitoring|Log files, latest entries|

---

## 💡 Common Workflows

```Bash
# See file structure
head -n 5 largefile.txt   # First few lines
tail -n 5 largefile.txt  # Last few lines

# Monitor logs in real-time
tail -f /var/log/syslog

# Grep in tail output
tail -f app.log | grep ERROR

# Combine head and tail
head -n 1 file.txt && tail -n 1 file.txt
```

---

