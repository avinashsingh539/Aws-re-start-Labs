# 231-[LX]-Lab - Editing Files

## 📌 Overview

This lab focuses on learning how to create, edit, save, and inspect text files in a Linux environment using two popular terminal-based text editors: **Vim** and **Nano**.

The exercises were performed on an Amazon Linux Command Host accessed through SSH as part of the AWS Academy/Vocareum Linux lab series.

## 🎯 Objectives

- Learn the basics of Vim using the interactive `vimtutor`.
- Create and edit text files using Vim.
- Understand Vim's Insert mode and command mode.
- Save changes using `:wq`.
- Discard unsaved changes using `:q!`.
- Create, edit, save, and reopen files using Nano.
- Understand the basic differences between Vim and Nano.

## 🛠️ Environment and Tools

| Component | Description |
|---|---|
| Operating system | Amazon Linux |
| Lab environment | AWS Academy / Vocareum |
| Remote access | SSH |
| Text editor 1 | Vim |
| Text editor 2 | Nano |
| Shell | Linux command line |

## 1. Connect to the Command Host

Connect to the lab's Command Host using SSH from Windows PowerShell.

```powershell
ssh -i "$HOME\Downloads\labsuser.pem" ec2-user@YOUR_PUBLIC_IP
```

Replace `YOUR_PUBLIC_IP` with the current Public IP displayed in the Vocareum lab details.

**Important:** Use the private key and IP address assigned to your current lab session. Do not publish your private key or reuse an expired lab IP.

## 2. Learn Vim Using vimtutor

Start the interactive Vim tutorial:

```bash
vimtutor
```

Complete lessons 1–3 to practice the fundamentals of navigating, editing, and working with text in Vim.

Exit the tutorial with:

```vim
:q!
```

Press `Enter` to return to the Linux terminal.

If Vim is unavailable in the lab environment, the lab instructions suggest installing it with:

```bash
sudo yum install vim
```

## 3. Create and Edit a File Using Vim

### Step 1: Create the file

```bash
vim helloworld
```

This opens the file in Vim. If `helloworld` does not already exist, Vim creates it when the file is saved.

### Step 2: Enter Insert mode

Press:

```text
i
```

Type the following content:

```text
Hello World!
This is my first file in Linux and I am editing it in Vim!
```

### Step 3: Save and exit

Press `Esc` to leave Insert mode, then execute:

```vim
:wq
```

Press `Enter`.

**Explanation:**

- `i` — enters Insert mode.
- `Esc` — returns to Normal mode.
- `:w` — writes (saves) the file.
- `:q` — quits Vim.
- `:wq` — saves changes and exits Vim.

## 4. Practice Editing and Discarding Changes

Reopen the file:

```bash
vim helloworld
```

Move to the last line and open a new line:

```text
G
o
```

Type the following additional line:

```text
I learned how to create a file, edit and save them too!
```

Press `Esc`, then execute:

```vim
:q!
```

Press `Enter`.

### Verify the result

Reopen the file:

```bash
vim helloworld
```

The original two lines should still be present, while the newly added line should be missing.

Exit without making any further changes:

```vim
:q!
```

### What this exercise demonstrates

The `:q!` command quits Vim **without saving unsaved modifications**. Because the added line was not saved, it disappears when the file is reopened.

This demonstrates the difference between saving a file and discarding changes.

## 5. Create and Edit a File Using Nano

### Step 1: Open a new file

```bash
nano cloudworld
```

Nano opens the file for editing. Unlike Vim, you can start typing immediately without entering a separate Insert mode.

### Step 2: Enter the content

Type the following sentence:

```text
We are using nano this time! We can simply start typing! No insert mode needed.
```

### Step 3: Save the file

Press the following keys:

1. `Ctrl + O` — write the file to disk.
2. `Enter` — confirm the filename `cloudworld`.
3. `Ctrl + X` — exit Nano.

### Step 4: Reopen and verify

```bash
nano cloudworld
```

Verify that the sentence was saved correctly, then exit with:

```text
Ctrl + X
```

## 6. Vim vs. Nano

| Feature | Vim | Nano |
|---|---|---|
| Editing style | Uses distinct modes | Direct text editing |
| Start typing | Enter Insert mode first | Type immediately |
| Save file | `:w` | `Ctrl + O` |
| Save and exit | `:wq` | `Ctrl + O`, `Enter`, then `Ctrl + X` |
| Exit without saving | `:q!` | `Ctrl + X`, then respond to the save prompt |
| Best suited for | Efficient keyboard-driven editing | Straightforward terminal text editing |

## 7. Important Commands Cheat Sheet

### Linux commands

| Command | Purpose |
|---|---|
| `vimtutor` | Start the Vim tutorial |
| `vim helloworld` | Open or create a file in Vim |
| `nano cloudworld` | Open or create a file in Nano |

### Vim commands and keys

| Command / Key | Purpose |
|---|---|
| `i` | Enter Insert mode |
| `Esc` | Return to Normal mode |
| `G` | Move to the last line |
| `o` | Open a new line below the current line |
| `:w` | Save changes |
| `:q` | Quit Vim |
| `:wq` | Save and quit |
| `:q!` | Quit without saving changes |
| `dd` | Delete the current line |
| `u` | Undo the last change |

### Nano shortcuts

| Shortcut | Purpose |
|---|---|
| `Ctrl + O` | Save the file |
| `Enter` | Confirm the filename |
| `Ctrl + X` | Exit Nano |

## 8. Key Learnings

- Linux provides terminal-based text editors for creating and modifying files.
- Vim separates navigation and editing into modes.
- Nano allows direct editing without a separate Insert mode.
- Saving a file preserves changes for future use.
- Exiting with `:q!` in Vim discards unsaved changes.
- Reopening files is a simple way to verify whether changes were saved.

## ✅ Lab Completion Checklist

- [x] Connected to the Command Host using SSH.
- [x] Completed the Vim tutorial lessons 1–3.
- [x] Created and saved `helloworld` using Vim.
- [x] Practiced editing a file and discarding unsaved changes.
- [x] Reopened the file to verify the saved content.
- [x] Created `cloudworld` using Nano.
- [x] Saved, reopened, and verified the Nano file.

## 🏁 Conclusion

This lab provided hands-on experience with two essential Linux text editors: Vim and Nano. By practicing file creation, editing, saving, and discarding changes, I developed a better understanding of terminal-based file management and the importance of verifying saved content.

These skills are useful for editing configuration files, working with scripts, modifying application settings, and managing files on remote Linux servers.

---

**Topics:** Linux · Vim · Nano · SSH · Command Line · File Editing · AWS Academy
