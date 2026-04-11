---
created: 2026-04-10 21:50
tags:
  - Devops
  - linux
source:
---
 > [!quote] We may encounter many defeats but we must not be defeated.
> — Maya Angelou

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


`awk` is a powerful scripting language designed for **text processing and report generation**.

While `sed` treats text as a stream of characters, **`awk` treats text like a spreadsheet** (rows and columns).

> [!tip] Analogy
> - `grep` = Flashlight (finds the row)
> - `sed` = Red Pen (edits the text)
> - `awk` = Calculator (understands table structures and math)

---

## 📋 Basic Syntax

```Bash
awk [options] 'pattern { action }' file
```

### Simple Example

```Bash
awk '{print $1}' names.txt
# Prints the FIRST column of every line
```

---

## 📋 How awk "Thinks": Fields and Rows

When `awk` reads a line, it splits it into **fields** (columns) based on whitespace.

### Field Variables

|Variable|Meaning|
|---|---|
|`$1`|First column|
|`$2`|Second column|
|`$3`|Third column|
|`$0`|Everything (whole line)|
|`$NF`|Last column (very useful!)|

---

## 🔧 Common Operations

### 1. Print Specific Columns

```Bash
# Print first column
awk '{print $1}' file.txt

# Print first and third columns
awk '{print $1, $3}' file.txt

# Print with custom text
awk '{print "User:", $1, "ID:", $3}' file.txt
```

### 2. Change the Delimiter (`-F`)

By default, `awk` looks for spaces. Use `-F` for other delimiters.

```Bash
# For CSV files
awk -F"," '{print $2}' data.csv

# For system user list (colon-separated)
awk -F":" '{print $1}' /etc/passwd
```

### 3. Filtering (The "Where" Clause)

```Bash
# Print lines where column 3 > 50
awk '$3 > 50 {print $0}' sales.txt

# Print lines where column 1 equals "Admin"
awk '$1 == "Admin" {print $2}' users.txt
```

### 4. Built-in Variables

|Variable|Meaning|
|---|---|
|`NR`|Number of Records (current line number)|
|`NF`|Number of Fields (columns in current line)|

```Bash
# Print line number before each line
awk '{print NR, $0}' file.txt

# Print only lines with more than 3 columns
awk 'NF > 3 {print $0}' file.txt

# Print last column (whatever it is)
awk '{print $NF}' file.txt
```

---

## 🔧 BEGIN and END Blocks

```Bash
awk 'BEGIN {print "Starting..."} {print $0} END {print "Done"}' file.txt
```

- `BEGIN`: Runs once before processing
- `END`: Runs once after processing

### Use Case: Calculate Totals

```Bash
# Sum column 3
awk '{sum += $3} END {print "Total:", sum}' numbers.txt

# Count lines
awk 'END {print "Lines:", NR}' file.txt
```

---

## 📊 Quick Reference Table

|Command|Purpose|
|---|---|
|`awk '{print $1}'`|Print first column|
|`awk -F"," '{print $2}'`|Print second column (CSV)|
|`awk '$3 > 50 {print $0}'`|Filter by value|
|`awk '{print NR, $0}'`|Print with line numbers|
|`awk '{print $NF}'`|Print last column|

---

## 🔗 Related Notes

- [[grep]] - Search for text
- [[sed]] - Text editing
- [[Bash Script]]