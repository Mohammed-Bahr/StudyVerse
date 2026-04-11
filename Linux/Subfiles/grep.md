---
created: 2026-04-10 21:50
tags:
  - Devops
  - linux
source:
---
> [!quote] The industrial landscape is already littered with remains of once successful companies that could not adapt their strategic vision to altered conditions of competition.
> — Abernathy

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


`grep` stands for **Global Regular Expression Print**. It's a command-line tool used to **search for text patterns** inside files or input streams.

---

## 📋 What Does grep Do?

`grep` **looks for specific words or patterns in text** and prints the lines that match.

> [!tip] Analogy
> Think of `grep` as a **flashlight** — it finds the needle in the haystack.

---

## 📋 Basic Syntax

```Bash
grep [options] pattern file
```

### Simple Examples

```Bash
grep "error" log.txt              # Find "error" in file
grep -i "error" log.txt         # Case-insensitive
grep -n "error" log.txt         # Show line numbers
grep -c "error" log.txt        # Count matches
```

---

## 🔧 Common Options

### Search Types

|Option|Purpose|Example|
|---|---|---|
|`-i`|Ignore case|`grep -i "error" log.txt`|
|`-v`|Invert match (show non-matching)|`grep -v "debug" log.txt`|
|`-w`|Match whole word only|`grep -w "cat" animals.txt`|
|`-x`|Match whole line only|`grep -x "exact match" file.txt`|

### Output Control

|Option|Purpose|Example|
|---|---|---|
|`-n`|Show line numbers|`grep -n "error" log.txt`|
|`-c`|Count matches only|`grep -c "error" log.txt`|
|`-o`|Show only matched text|`grep -o "error" log.txt`|

### File Search

|Option|Purpose|Example|
|---|---|---|
|`-r`|Recursive search|`grep -r "config" /etc`|
|`-l`|Show filenames only|`grep -l "error" *.log`|
|`-L`|Show non-matching files|`grep -L "error" *.log`|

---

## 💡 Practical Examples

```Bash
# Find all errors in a log file
grep -n "ERROR" /var/log/syslog

# Case-insensitive search
grep -i "failed" auth.log

# Find lines that don't contain "debug"
grep -v "debug" app.log

# Find whole words only
grep -w "test" file.txt

# Search multiple files
grep "TODO" *.py *.js

# Recursive search
grep -r "database" /var/www/html/

# Show matching text only
grep -o "[0-9]\+" file.txt
```

---

## 🔗 Combining with Pipes

```Bash
# Find processes
ps aux | grep nginx

# Find files modified today
ls -la | grep "Nov 10"

# Combine with other commands
cat log.txt | grep -i "error" | wc -l
```

---

## 📊 Regular Expressions in grep

|Pattern|Meaning|
|---|---|
|`^text`|Line starts with "text"|
|`text$`|Line ends with "text"`|
|`.`|Any single character|
|`*`|Zero or more|
|`[abc]`|a, b, or c|
|`[^abc]`|Not a, b, or c|
|`\`|Escape special character|

### Examples

```Bash
# Lines starting with "Error"
grep "^Error" log.txt

# Lines ending with "failed"
grep "failed$" log.txt

# IP addresses
grep -E "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" log.txt

# Email addresses
grep -E "[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}" file.txt
```

> [!tip] Use `-E` for Extended Regex
> Use `grep -E` (or `egrep`) for more powerful regex patterns.

---

## 📊 Quick Reference Table

|Command|Purpose|
|---|---|
|`grep "word" file`|Find "word" in file|
|`grep -i "word" file`|Case-insensitive search|
|`grep -n "word" file`|Show line numbers|
|`grep -v "word" file`|Show lines WITHOUT "word"|
|`grep -r "word" dir`|Search recursively|
|`ps aux \| grep process`|Find running process|

---

## 🔗 Related Notes

- [[sed]] - Text editing
- [[awk]] - Text processing
- [[Regular Expressions]]