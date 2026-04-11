---
created: 2026-04-10 21:51
tags:
  - Devops
  - linux
source:
---
> [!quote] Love vanquishes time. To lovers, a moment can be eternity, eternity can be the tick of a clock.
> — Mary Parrish

**"اللهم إني أسألك فهم النبيين، وحفظ المرسلين، والملائكة المقربين، اللهم اجعل ألسنتنا عامرة بذكرك، وقلوبنا بخشيتك، إنك على كل شيء قدير"**


`vi` and `vim` are terminal-based **text editors** available on almost all Linux systems. They're powerful but **modal** — meaning they have different modes for different tasks.

---

## 🎯 What is Vi/Vim?

- **vi** = Original Unix text editor
- **vim** = **Vi iMproved** (enhanced version)
- Available on **virtually every Linux system**
- Essential for quick edits without a GUI

---

## 📋 Modes

Vi has different modes:

|Mode|Purpose|
|---|---|
|**Normal**|Navigate and delete text (default)|
|**Insert**|Type and edit text (press `i`)|
|**Command**|Run commands like save/quit (press `:`)|

---

## ⌨️ Essential Navigation (Normal Mode)

|Key|Action|
|---|---|
|`h`|Move left|
|`j`|Move down|
|`k`|Move up|
|`l`|Move right|
|`w`|Word forward|
|`b`|Word backward|
|`0`|Start of line|
|`$`|End of line|
|`gg`|First line|
|`G`|Last line|
|`:N`|Go to line N|

---

## ✏️ Switching to Insert Mode

|Key|Action|
|---|---|
|`i`|Insert before cursor|
|`a`|Insert after cursor|
|`I`|Insert at beginning of line|
|`A`|Insert at end of line|
|`o`|New line below|
|`O`|New line above|
|`Esc`|Return to Normal mode|

---

## 🗑️ Deleting Text

|Key|Action|
|---|---|
|`x`|Delete character|
|`dw`|Delete word|
|`dd`|Delete entire line|
|`dNd`|Delete N lines|
|`D`|Delete to end of line|

### Special Deletes

```vim
:%d           " Delete all lines (clear file)
:d            " Delete current line
:3,7d        " Delete lines 3 to 7
```

---

## 💾 Saving and Quitting

These commands are entered in **Command mode** (press `:` first):

|Command|Action|
|---|---|
|`:w`|Save (write)|
|`:q`|Quit (if no changes)|
|`:wq`|Save and quit|
|`:q!`|Quit without saving|
|`:x`|Save and quit (shorthand)|
|`:w file`|Save to new file|

---

## ✂️ Copy and Paste

|Key|Action|
|---|---|
|`yy`|Yank (copy) line|
|`p`|Paste below|
|`P`|Paste above|
|`yiw`|Yank inner word|

---

## 🔍 Search and Replace

```vim
/"word"         " Search forward for "word"
?n             " Search backward for "n"
:%s/old/new/g  " Replace all "old" with "new"
```

---

## 💡 Practical Examples

```bash
# Open a file
vi file.txt

# Open multiple files
vi file1.txt file2.txt

# Create new file
vi newfile.txt

# Open at specific line
vi +10 file.txt

# Open at last line
vi + file.txt
```

---

## 📊 Quick Reference

|Command|Action|
|---|---|
|`i`|Insert mode|
|`Esc`|Normal mode|
|`:w`|Save|
|`:q`|Quit|
|`:wq`|Save & quit|
|`:q!`|Force quit|
|`dd`|Delete line|
|`yy`|Copy line|
|`p`|Paste|
|`u`|Undo|
|`Ctrl+r`|Redo|

---
