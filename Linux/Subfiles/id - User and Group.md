---
created: 2026-04-10 21:51
tags:
  - Devops
  - linux
source:
---
> [!quote] Meditation is the dissolution of thoughts in eternal awareness or Pure consciousness without objectification, knowing without thinking, merging finitude in infinity.
> — Voltaire

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


The `id` command displays **user and group identity information**.

---

## 📋 Basic Usage

```Bash
id
```

**Output Example:**
```
uid=1000(bahr) gid=1000(bahr) groups=1000(bahr),10(wheel),984(docker)
```

---

## 🔍 What Each Part Means

|Field|Meaning|
|---|---|
|`uid`|User ID (numeric ID)|
|`(bahr)`|Username|
|`gid`|Primary Group ID|
|`groups`|All groups the user belongs to|

---

## 🔧 Common Options

```Bash
id              # Full output
id -u           # User ID only
id -g           # Group ID only
id -G           # All group IDs (space-separated)
id -un          # Username only
id -gn          # Group name only
id -Gn          # All group names
```

### Examples

```Bash
# Current user
id

# Specific user
id username
id root

# User ID only (useful in scripts)
id -u

# Group ID only
id -g
```

---

## 💡 Common Use Cases

### Check if Running as Root

```Bash
if [[ $(id -u) -eq 0 ]]; then
    echo "Running as root"
else
    echo "Not root"
fi
```

### Verify Permissions

```Bash
# Check if user is in docker group
id -Gn | grep docker

# Check if user is in wheel group
id -Gn | grep wheel
```

---

## 📊 Quick Reference

|Command|Output|
|---|---|
|`id`|uid, gid, and groups|
|`id -u`|User ID only|
|`id -g`|Primary group ID|
|`id -G`|All group IDs|
|`id -un`|Username only|
|`id root`|Info for root user|

