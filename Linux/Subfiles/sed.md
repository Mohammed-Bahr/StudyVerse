---
created: 2026-04-10 21:50
tags:
  - Devops
  - linux
source:
---
> [!quote] The way is not in the sky. The way is in the heart.
> — Buddha

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


`sed` stands for **Stream EDitor**. It's a command-line tool used to **search, modify, replace, insert, or delete text** in a stream (files or command output).

> [!tip] Analogy
> Think of `sed` as a **red pen** — it crosses out and rewrites words.

---

## 📋 Basic Syntax

```Bash
sed [options] 'command' file
```

### Simple Example

```Bash
sed 's/cat/dog/' animals.txt
# Replaces first "cat" with "dog" on each line
```

> [!warning] By default, sed does NOT change the file — it only prints the result.

---

## 🔧 How sed "Thinks"

**sed is a loop** that processes one line at a time:

1. **Read** — Read one line from input
2. **Buffer** — Place in Pattern Space
3. **Execute** — Run your commands
4. **Print** — Output result
5. **Repeat** — Move to next line

> [!tip] Why this matters
> sed only looks at one line at a time, making it incredibly fast for massive log files.

---

## 🔧 Common Operations

### 1. Substitution (Search & Replace)

```Bash
# Replace first match per line
sed 's/old/new/' file.txt

# Replace ALL matches (global)
sed 's/old/new/g' file.txt

# Case-insensitive
sed 's/hello/hi/gi' file.txt
```

### 2. Edit File In Place

```Bash
# Modify the actual file
sed -i 's/localhost/127.0.0.1/g' config.txt

# Backup before editing
sed -i.bak 's/old/new/g' file.txt
# Creates: file.txt.bak
```

### 3. Delete Lines

```Bash
# Delete lines containing a word
sed '/error/d' log.txt

# Delete specific line number
sed '5d' file.txt

# Delete range of lines
sed '2,5d' file.txt

# Delete lines from pattern to end
sed '/ERROR/,$d' log.txt
```

### 4. Print Specific Lines

```Bash
# Print line 3
sed -n '3p' file.txt

# Print range
sed -n '1,5p' file.txt

# Print lines NOT matching pattern (inverted)
sed -n '/error/!p' log.txt
```

### 5. Insert and Append

```Bash
# Insert BEFORE line 3
sed '3i\This is inserted' file.txt

# Append AFTER line 3
sed '3a\This is appended' file.txt

# Insert at beginning
sed '1i\Header line' file.txt

# Append at end
sed '$a\Footer line' file.txt
```

### 6. Replace Only on Specific Lines

```Bash
# By line number
sed '2s/apple/orange/' file.txt

# By pattern (only lines containing "ERROR")
sed '/ERROR/s/fail/warn/' log.txt
```

---

## 🔧 The Delimiter Trick

You can use any character as the separator (useful for paths):

```Bash
# Instead of escaping slashes:
sed 's/\/var\/www\/html/\/home\/user\/sites/' config.conf

# Use a different delimiter:
sed 's|/var/www/html|/home/user/sites|' config.conf
```

---

## 🔧 With Pipes

```Bash
# Process command output
ps aux | sed 's/root/admin/'

# Replace in file content
cat file.txt | sed 's/[0-9]/#/g'
```

---

## 📊 Quick Reference Table

|Command|Purpose|
|---|---|
|`sed 's/old/new/'`|Replace first match|
|`sed 's/old/new/g'`|Replace all matches|
|`sed -i 's/old/new/'`|Edit file in place|
|`sed '/word/d'`|Delete matching lines|
|`sed '3d'`|Delete line 3|
|`sed '1,5d'`|Delete lines 1-5|
|`sed -n '3p'`|Print line 3|

---

## 🔗 Related Notes

- [[grep]] - Search for text
- [[awk]] - Text processing (columns)
- [[Bash Script]]