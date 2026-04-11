---
created: 2026-04-10 13:17
tags:
  - Devops
  - linux
source:
---
> [!quote] Action may not always bring happiness; but there is no happiness without action.
> — Benjamin Disraeli

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**

File permissions control who can access and what they can do with a file or directory. They are fundamental to Linux security.

---

## 🔐 Permission Categories

Permissions are assigned to three categories:

|Category|Abbreviation|Description|
|---|---|---|
|**Owner**|`u`|The user who owns the file.|
|**Group**|`g`|Users belonging to the file's group.|
|**Others**|`o`|All other users on the system.|

---

## 📋 Permission Types

|Permission|Abbreviation|Numeric Value|Description|
|---|---|---|---|
|**Read**|`r`|4|View file content or list directory contents.|
|**Write**|`w`|2|Modify, delete, rename file. Create/delete files in directory.|
|**Execute**|`x`|1|Run file as program/script. Enter directory.|

---

## 🔍 Reading Permissions

When you run `ls -l`, permissions are shown as a 10-character string:

```Bash
-rw-r--r-- 1 user group 4096 Jan 10 12:30 file.txt
```

**Breaking down `-rw-r--r--`:**

- **First character:** File type
    - `-` = regular file
    - `d` = directory
    - `l` = symbolic link

- **Next 3 characters (`rw-`):** Owner permissions
- **Next 3 characters (`r--`):** Group permissions
- **Last 3 characters (`r--`):** Others permissions

### Example Breakdown

`rwxr-xr--` means:

|Category|Permission|Meaning|
|---|---|---|
|**Owner**|`rwx`|Read, Write, Execute (full control)|
|**Group**|`r-x`|Read and Execute only|
|**Others**|`r--`|Read only|

---

## 🛠️ Changing Permissions with `chmod`

### Symbolic Method

```Bash
chmod [who][+/-][permission] file
```

|Symbol|Meaning|
|---|---|
|`u`|User (owner)|
|`g`|Group|
|`o`|Others|
|`a`|All (user + group + others)|
|`+`|Add permission|
|`-`|Remove permission|
|`=`|Set exact permission|

**Examples:**

```Bash
chmod +x script.sh        # Make executable for everyone
chmod u+x file.txt        # User can execute
chmod g-w file.txt        # Group cannot write
chmod o+r file.txt        # Others can read
chmod a+rw file.txt       # Everyone can read and write
```

---

### Numeric Method

Use numbers to set all permissions at once:

```Bash
chmod 755 myfile.txt
```

**Calculation:**

|Category|Value|Calculation|Result|
|---|---|---|---|
|User|7|4 + 2 + 1|`rwx`|
|Group|5|4 + 0 + 1|`r-x`|
|Others|5|4 + 0 + 1|`r-x`|

**Common Combinations:**

|Numeric|Permissions|Use Case|
|---|---|---|
|`755`|`rwxr-xr-x`|Executable scripts|
|`644`|`rw-r--r--`|Regular files|
|`600`|`rw-------`|Private files (SSH keys)|
|`700`|`rwx------`|Private directories|
|`777`|`rwxrwxrwx`|Everyone has full access (dangerous)|

---

## 📊 Permission Values Quick Reference

|Permission|Binary|Decimal|
|---|---|---|
|`---`|000|0|
|`--x`|001|1|
|`-w-`|010|2|
|`-wx`|011|3|
|`r--`|100|4|
|`r-x`|101|5|
|`rw-`|110|6|
|`rwx`|111|7|

---

## 💡 Best Practices

> [!warning] Security Warning
> Never use `chmod 777` on production systems. It gives everyone full access to the file.

> [!tip] Common Permission Mistakes
> - Forgetting to make scripts executable: `chmod +x script.sh`
> - Setting too permissive permissions on sensitive files
> - Not understanding the difference between file and directory permissions

**Directory permissions reminder:**
- **Read** = List contents (`ls`)
- **Write** = Create/delete files inside
- **Execute** = Enter the directory (`cd`)

---

## 🔗 Related Notes

- [[Linux Filesystem Structure]]
- [[ls - List Files]]
- [[Bash Script]] (for setting permissions in scripts)