---
created: 2026-04-10 21:50
tags:
  - Devops
  - linux
source:
---
> [!quote] All our knowledge begins with the senses, proceeds then to the understanding, and ends with reason. There is nothing higher than reason.
> — Immanuel Kant

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


**File globbing** is a way to use special symbols to match multiple files at once, so you don't have to type every filename. It's like a shortcut for selecting files in the terminal.

> [!tip] Analogy
> Think of `*` as a **wildcard**, `?` as a **single-character blank**, and `[]` as a **choice box** for characters.

---

## 📋 Wildcard Symbols

### `*` — Match Any Characters

Matches **any number of characters** (including zero).

```Bash
# All text files
ls *.txt

# All Python files
ls *.py

# All files with "log" anywhere in name
ls *log*

# All files
ls *
```

> [!note] Does NOT match hidden files (starting with `.`)
> Use `.*` for hidden files.

---

### `?` — Match Exactly One Character

Matches **exactly one character**.

```Bash
# file1.txt, file2.txt but NOT file10.txt
ls file?.txt

# fileA.txt, fileB.txt but NOT fileAB.txt
ls file?.txt

# match1.txt, match2.txt, etc.
ls match?.dat
```

---

### `[]` — Match Any One Character in Brackets

Matches **any single character inside the brackets**.

#### Single Characters

```Bash
# Matches: afile, bfile, cfile
ls [abc]file

# Matches: file1.txt to file9.txt
ls file[0-9].txt

# Matches: testA, testB, testC
ls test[A-C]
```

#### Ranges

```Bash
# Any digit
ls file[0-9].txt

# Any lowercase letter
ls file[a-z].txt

# Any uppercase letter
ls file[A-Z].txt

# Any letter (case-insensitive on some systems)
ls file[a-zA-Z].txt
```

#### Inverted Sets

```Bash
# Any character that is NOT a, b, or c
ls [!abc]file

# Any character that is NOT a vowel
ls [!aeiou]*
```

---

## 📊 Globbing Patterns Quick Reference

|Pattern|Matches|Example|
|---|---|---|
|`*`|Any characters|`*.txt` matches all .txt files|
|`?`|Exactly one|`file?.txt` matches file1.txt|
|`[abc]`|One of a, b, c|`[123]file` matches 1file, 2file|
|`[a-z]`|Range|`file[a-z].txt` matches filea.txt to filez.txt|
|`[!abc]`|Not a, b, c|`[!123]file` excludes 1file, 2file|

---

## 💡 Practical Examples

```Bash
# Copy all text files
cp *.txt backup/

# Remove all log files
rm *error*.log

# List all Python and JavaScript files
ls *.{py,js}

# Move all files starting with "report" (2020-2024)
mv report20*.pdf documents/

# Archive all files modified today
cp *$(date +%Y%m%d)* archive/

# Copy all files except .bak
cp !(*.bak) backup/
```

---

## ⚠️ Common Mistakes

> [!warning] Things Globbing Does NOT Do
> - Match path separators (`/` inside patterns)
> - Match hidden files (starts with `.`)
> - Execute commands on matched files

> [!tip] Safe handling
> Always `echo` first to see what will be matched:
> ```Bash
> echo *.txt  # See what files will match before acting
> ```

---

## 🔗 Related Notes

- [[ls - List Files]]
- [[find]]
- [[cp mv rm - Copy Move Delete]]