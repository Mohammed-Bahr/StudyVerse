---
created: 2026-04-10 21:42
tags:
  - Devops
  - linux
source:
---
> [!quote] You were not born a winner, and you were not born a loser. You are what you make yourself be.
> — Lou Holtz

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


A **shell** is a program that serves as an interface between a user and the operating system's kernel. Its primary function is to interpret commands entered by the user and execute them by communicating with the underlying operating system services.

---

## 🖥️ What is a Shell?

![[92ede691-594c-42f7-a976-9d3a6ce6b538.png]]

### Key Functions

|Function|Description|
|---|---|
|**Command interpretation**|Translates user commands to the OS|
|**Program execution**|Runs programs like `ls` or `dir`|
|**Scripting**|Automates tasks with scripts|
|**Input/output management**|Redirects input and output as needed|

---

## 📋 Types of Shells

|Type|Description|Examples|
|---|---|---|
|**CLI Shells**|Text-based interface for commands and automation|Bash, Zsh, Ksh, PowerShell|
|**GUI Shells**|Visual interface with windows, icons, and menus|GNOME Shell, KDE Plasma, Windows Shell|

> [!note] Important Distinction
> **Shell is NOT something under Bash and PowerShell.**  
> **Bash and PowerShell ARE shells themselves** — they are just types of shell.

---

## 🐚 Common CLI Shells

### Bash (Bourne Again SHell)
- Most common shell on Linux systems
- Default on many distributions
- Great for scripting and automation
- See [[Bash Script]] for detailed usage

### Zsh (Z Shell)
- Enhanced version of Bash
- Better auto-completion and theming
- Popular among developers (used with Oh My Zsh)

### Ksh (KornShell)
- Created by David Korn at Bell Labs
- Features from both Bourne shell and C shell
- Used in some enterprise environments

### PowerShell
- Developed by Microsoft
- Object-oriented shell
- Available on Windows, Linux, and macOS
- Uses cmdlets (verb-noun format)

---

## 🔄 Shell vs Terminal

Many people confuse these terms:

|Term|What it is|
|---|---|
|**Shell**|The program that interprets commands (Bash, Zsh)|
|**Terminal**|The application/window that displays the shell|
|**Console**|Physical terminal or the main interface|

**Analogy:**
- Terminal = The TV screen
- Shell = The Netflix app running on it

---

## 📊 Shell Comparison Table

|Feature|Bash|Zsh|PowerShell|
|---|---|---|---|
|Default on|Most Linux|macOS (Catalina+)|Windows|
|Scripting|Bash scripts|Bash + Zsh features|PowerShell scripts|
|Auto-completion|Basic|Advanced|Advanced|
|Object-oriented|No|No|Yes|
|Cross-platform|Yes|Yes|Yes|
|Learning curve|Medium|Medium|Steep|

---

## 💡 How to Check Your Shell

```Bash
# Check current shell
echo $SHELL

# List available shells
cat /etc/shells

# Check Bash version
bash --version
```

---

## 🔗 Related Notes

- [[Bash Script]] - Detailed Bash scripting reference
- [[Linux Filesystem Structure]]
- [[Linux File Permissions]]