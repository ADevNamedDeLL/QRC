# QRC — Robocopy GUI (Dark-Themed Windows Forms Wrapper)

A friendly, fast, and power-user-oriented Windows Forms GUI for **robocopy**, featuring safe path handling, threaded execution, and **Fast Fucking Mode (FFM)** defaults.

[![Build Status](https://img.shields.io/badge/build-passing-brightgreen.svg)](#)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](#)
[![Latest Release](https://img.shields.io/badge/release-latest-orange.svg)](#)

---

## Table of Contents
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation / Build](#installation--build)
- [Running QRC](#running-qrc)
- [Usage Guide](#usage-guide)
  - [Example 1: Copy folder contents to a root drive](#example-1-copy-folder-contents-to-a-root-drive)
  - [Example 2: Create destination folder automatically](#example-2-create-destination-folder-automatically)
  - [Example 3: Fast Fucking Mode on/off](#example-3-fast-fucking-mode-onoff)
  - [Other Notes: /E vs /S, filters, logs](#other-notes-e-vs-s-filters-logs)
- [Troubleshooting](#troubleshooting)
- [Security & Safety Notes](#security--safety-notes)
- [FAQ](#faq)
- [License](#license)
- [Credits](#credits)

---

## Features

- **Fast Fucking Mode (FFM)** *(default ON)*  
  Uses high-performance robocopy flags:  
  /MT:128 /J /R:0 /W:0 /NP /NFL /NDL /NJH /NJS
  When **Include subdirectories** is checked, QRC adds `/E` (all subfolders including empty).

- **Safe path quoting & escaping**  
Automatically applies correct quoting and escaping so paths like `D:\` and `Z:\Music\` work reliably even with trailing backslashes.

- **Create folder at destination**  
When enabled, QRC appends the source folder name to the destination.  
Example: Source `Z:\Music`, Destination `D:\` → actual target becomes `D:\Music`.

- **Threaded robocopy execution**  
Output and error streams are redirected and displayed live in the UI.  
QRC also shows a pretty UI command:  
robocopy "Z:\Music" "D:\Music" /E /MT:128 /J ...
while passing the correctly escaped argument array internally.

- **Start / Cancel controls**  
Cancel cleanly terminates the robocopy process.

- **Log to file option**

- **Manual advanced args**  
A free-form box for additional flags.

---

## Prerequisites

- **Windows 10 or Windows 11**
- **robocopy**  
  Included by default in Windows (`C:\Windows\System32\robocopy.exe`).

---

## Installation / Build

Clone the repository:

```powershell
git clone https://github.com/ADevNamedDeLL/QRC
```
OR

Download the repo directly, and Run exe

## Running QRC

Run ```QRC.exe```

# Usage Guide

## Example 1: Copy folder contents to a root drive

**Source:**
Z:\Music

**Destination:**
D:\

**UI displays something like:**
> robocopy "Z:\Music" "D:\" /E /MT:128 /J /R:0 /W:0 /NP /NFL /NDL /NJH /NJS

This copies all subdirectories (because /E) into D:\, resulting in:
D:\<all Music subfolders and files>

---

## Example 2: Create destination folder automatically

Check Create folder at destination.

**Source:**
Z:\Music

**Destination:**
D:\

**QRC resolves target to:**
D:\Music

**Final UI command:**
> robocopy "Z:\Music" "D:\Music" /E /MT:128 /J /R:0 /W:0 /NP /NFL /NDL /NJH /NJS

This copies the folder itself, not just its contents, into the destination.

---

## Example 3: Fast Fucking Mode ON/OFF

### FFM ON (default):
> robocopy "C:\Src" "E:\Dst" /E /MT:128 /J /R:0 /W:0 /NP /NFL /NDL /NJH /NJS

### FFM OFF:
> robocopy "C:\Src" "E:\Dst" /E

### Internal escaping example:
Internally, QRC may pass escaped arguments such as:
C:\\Src  
E:\\Dst  

This is normal—UI displays the human-friendly version; the process runner escapes backslashes for correctness.

---

# Other Notes: /E vs /S, filters, logs

**/E** — include all subdirectories, including empty  
**/S** — include subdirectories except empty  

**File filter — robocopy syntax:**
robocopy src dst *.mp3 *.wav

QRC supports adding filter patterns in the advanced args box.

**Logs —** enable the log checkbox to pipe output to a file. Internally uses:
`/LOG:<path>`

---

# Troubleshooting

**Exit code 3**  
Files copied successfully. Not an error.

**Exit code 16**  
Robocopy encountered a serious failure. Check permissions, locks, or invalid flags.

**"Invalid Parameter" errors**  
Often caused by missing quotes or trailing backslash ambiguity. QRC’s quoting system (QuoteArg + FixPath) should prevent this—verify you did not include mismatched manual args.

**Folders not copying**  
- Did you intend to copy subfolders? Ensure /E or Include subdirectories is enabled.  
- If using Create folder at destination, confirm the computed final path in the command preview.

---

# Security & Safety Notes

### Beware of /MIR (mirror):
This can delete files in the destination to match the source. Only use it if you absolutely know the impact.

### Do not run QRC as Administrator unless required.
Elevated robocopy operations can modify critical system areas quickly.

---

# FAQ

**Q: Why are paths shown as D:\\?**  
A: The UI shows clean paths ("D:\"), but the underlying process arguments escape backslashes (D:\\) because .NET passes them as literal strings.

**Q: Why is the exit code non-zero?**  
A: Robocopy uses exit codes to signal file changes, not only errors. Codes 0–7 often indicate success or partial success.

**Q: How do I copy the folder itself instead of its contents?**  
A: Enable Create folder at destination or manually set the destination to include the folder name (e.g., D:\Music).

---

# License

QRC is distributed under the MIT License:

---

# Credits

QRC is built to make Robocopy safer, faster, and less annoying for power users.

