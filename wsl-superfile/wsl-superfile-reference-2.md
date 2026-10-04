---
title: "wsl-superfile-reference-2"
doc-id: "mb-773d936b"
updated: "2026-10-04"
---
# WSL Superfile Reference

A practical beginner guide to **Superfile** (`spf`) for browsing and managing files from a WSL Bash terminal.

> **Important:** Superfile operates on real files. Copying, moving, renaming, and deleting in Superfile changes the real files on disk, much like performing the same action in Windows Explorer.

---

## What Superfile Is

**Superfile** is a terminal file manager. It gives you a visual way to browse folders and manage files inside a Bash/WSL terminal window.

It is similar in purpose to Windows Explorer, but it is keyboard-driven and runs inside the terminal.

Use Superfile to:

- Browse folders without repeatedly typing `cd` and `ls`
- Find files and inspect a project folder structure
- Copy, move, rename, create, or delete ordinary files
- Compare or transfer files between two folders using multiple panels
- View hidden files such as `.gitignore` and `.env`
- Search within a folder

Superfile starts with:

```bash
spf
```

### Superfile, OpenCode, and Bash

These tools have different jobs:

| Tool | Primary use |
|---|---|
| Superfile (`spf`) | Browse folders, locate files, inspect paths, and organise ordinary assets |
| OpenCode (`opencode`) | Understand the project, plan work, review files, and make AI-assisted code or documentation changes |
| Bash | Run explicit commands such as `git status`, `git diff`, `rg`, and scripts |

Superfile is not an editor and it is not a Git client. OpenCode is your coding environment.

---

## Project Paths

### MakePDLF project library

Windows Explorer path:

```text
C:\dev\dev-makepdlf\makepdlf-project-library
```

Equivalent WSL Bash path:

```text
/mnt/c/dev/dev-makepdlf/makepdlf-project-library
```

### Understand Windows and WSL paths

In WSL, the Windows `C:` drive is normally available under `/mnt/c`.

| Windows path | WSL path |
|---|---|
| `C:\` | `/mnt/c/` |
| `C:\dev` | `/mnt/c/dev` |
| `C:\dev\dev-makepdlf` | `/mnt/c/dev/dev-makepdlf` |
| `C:\dev\dev-makepdlf\makepdlf-project-library` | `/mnt/c/dev/dev-makepdlf/makepdlf-project-library` |

Windows uses backslashes: `\`

WSL/Linux uses forward slashes: `/`

### Confirm the current folder

Before starting Superfile, run:

```bash
pwd
ls -la
```

When you are in the MakePDLF project library, `pwd` should display:

```text
/mnt/c/dev/dev-makepdlf/makepdlf-project-library
```

To open the exact current WSL folder in Windows Explorer:

```bash
explorer.exe .
```

The folder shown by Superfile should match the folder opened by `explorer.exe .`.

---

## Start Superfile

### Open Superfile in the MakePDLF project library

Run these commands in WSL Bash:

```bash
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
spf
```

You can also start Superfile by providing the folder path directly:

```bash
spf /mnt/c/dev/dev-makepdlf/makepdlf-project-library
```

### Quit Superfile

Press either:

```text
q
```

or:

```text
Esc
```

You return to your Bash terminal.

### Show Superfile's built-in help

While Superfile is open, press:

```text
?
```

Use this whenever you forget a key. It shows the hotkeys supported by your installed version.

---

## Screen Layout

Superfile normally has several areas:

| Area | What it does |
|---|---|
| Sidebar | Shortcuts, pinned folders, and disks |
| File panel | Files and folders in the current location |
| Preview panel | Preview of the selected file where supported |
| Metadata area | Information such as size, type, and modified date |
| Footer | Status, selection, and operation information |

The highlighted row is the currently selected file or folder.

---

## Basic Navigation

Start with these keys. Arrow keys are completely fine; you do not need to learn Vim-style keys immediately.

| What you want to do | Key |
|---|---|
| Move up | `Up Arrow` or `k` |
| Move down | `Down Arrow` or `j` |
| Move up one page | `Page Up` |
| Move down one page | `Page Down` |
| Open a folder or file | `Enter`, `Right Arrow`, or `l` |
| Go to the parent folder | `Backspace`, `Left Arrow`, or `h` |
| Quit | `q` or `Esc` |
| Show help | `?` |

### Browse a folder

1. Use `Up Arrow` or `Down Arrow` to highlight a folder.
2. Press `Enter` to open it.
3. Press `Backspace` to return to the folder above it.

---

## Hidden Files

On Linux and WSL, files whose names begin with a dot are hidden by default.

Examples:

```text
.git
.gitignore
.env
.env.example
.vscode
```

To show or hide these files in Superfile, press:

```text
.
```

Press `.` again to hide them.

> **Caution:** `.env` files may contain passwords, tokens, or other secrets. Do not copy, publish, or commit them unless you deliberately intend to do so.

---

## Search, Sorting, and Preview

### Search the current folder

Press:

```text
/
```

Type part of a name to find it. For example, type `design` to find a file named `makepdlf-design-notes.md`.

Press `Esc` to cancel the search.

> This searches the currently displayed folder. It is not a full-project content search.

### Change sorting

Press:

```text
o
```

Choose an order such as name, size, modified date, or file type.

### Reverse the current sort direction

Press:

```text
R
```

### Show or hide the preview panel

Press:

```text
f
```

Text files such as Markdown, JSON, YAML, Python, JavaScript, HTML, and CSS usually preview well. Other preview types depend on your terminal and installed support.

---

## Work With Files Safely

These actions operate on real files.

### Copy a file or folder

1. Highlight the item.
2. Press `Ctrl + C`.
3. Navigate to the destination folder.
4. Press `Ctrl + V`.

Copying leaves the original in place.

### Move a file or folder

1. Highlight the item.
2. Press `Ctrl + X`.
3. Navigate to the destination folder.
4. Press `Ctrl + V`.

Moving relocates the original item.

### Rename a file or folder

1. Highlight the item.
2. Press `Ctrl + R`.
3. Type the new name.
4. Press `Enter` to confirm or `Esc` to cancel.

### Create a file

Press:

```text
Ctrl + N
```

Type a filename, for example:

```text
notes.md
```

Then press `Enter`.

### Create a folder

Press:

```text
Ctrl + N
```

Type a name ending in `/`, for example:

```text
research/
```

Then press `Enter`.

### Delete a file or folder

Highlight the item and press:

```text
Delete
```

or:

```text
Ctrl + D
```

Confirm only after checking that the selected item is correct.

### Permanently delete

```text
Shift + D
```

> **Warning:** Permanent deletion is risky. Do not use it unless you are certain the item is no longer needed.

### Beginner safety checklist

Before copying, moving, renaming, or deleting:

- Confirm you are in the intended folder
- Confirm the highlighted or selected file is correct
- Copy first rather than move if you are unsure
- Avoid changing `.git`, `.env`, project configuration, or source files casually
- Run `git status` after meaningful work in a Git repository

---

## Select Multiple Files

Superfile has a selection mode for operating on several items.

### Enter or leave selection mode

Press:

```text
v
```

Press `v` again to return to normal mode.

### Select more items

| What you want to do | Key |
|---|---|
| Extend selection down | `Shift + Down Arrow` or `J` |
| Extend selection up | `Shift + Up Arrow` or `K` |
| Select all items in the panel | `A` |

Before applying an operation to several files, pause and check the selection carefully.

---

## Use Two Folder Panels

Superfile can display multiple file panels. This is particularly useful for copying or moving files between locations.

### Create a second panel

Press:

```text
n
```

### Switch between panels

Press:

```text
Tab
```

You can also use `Shift + L` for the next panel or `Shift + H` for the previous panel.

### Close the active panel

Press:

```text
w
```

### Example: copy an asset to the project

1. Start Superfile in the project library.
2. Press `n` to add another panel.
3. In one panel, navigate to the source folder containing the asset.
4. Press `Tab` to switch to the other panel.
5. Navigate to the destination folder in the MakePDLF project library.
6. Press `Tab` to return to the source panel.
7. Highlight the asset and press `Ctrl + C`.
8. Press `Tab` to switch to the destination panel.
9. Press `Ctrl + V`.

Check the active panel before pasting.

---

## Paths and Commands

### Copy a selected item's path

Highlight a file or folder and press:

```text
Ctrl + P
```

### Copy the current folder path

Press:

```text
c
```

This is useful for pasting a path into a Bash command, a document, or an OpenCode prompt.

### Open a shell command prompt in the current folder

Press:

```text
:
```

For example, you can run:

```bash
git status
```

or:

```bash
ls -la
```

> **Caution:** This runs a real shell command in the current folder. As a beginner, use it only for commands you understand.

### Pin or unpin the current folder

Press:

```text
P
```

Pinned folders appear in Superfile’s sidebar. A useful folder to pin is:

```text
/mnt/c/dev/dev-makepdlf/makepdlf-project-library
```

---

## Use Superfile With OpenCode

OpenCode is your coding environment. Superfile helps you find and browse files; OpenCode helps you understand, plan, and modify the project.

### Start OpenCode in the MakePDLF project library

From Bash:

```bash
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
opencode
```

Or pass the path directly:

```bash
opencode /mnt/c/dev/dev-makepdlf/makepdlf-project-library
```

### Recommended workflow

1. Use Superfile to browse the project and find the relevant folder or file.
2. Note the project-relative file path, such as `docs/overview.md`.
3. Quit Superfile with `q`, or keep it open in a separate Windows Terminal pane.
4. Start OpenCode from the MakePDLF project root.
5. Refer to the file in an OpenCode request using `@`.
6. Ask OpenCode to inspect, explain, plan, review, or change the file.
7. Use `git status` and `git diff` to inspect resulting changes.

### Example OpenCode prompts

```text
Review @docs/overview.md for clarity and factual consistency. Do not make changes; list recommendations first.
```

```text
Explain the purpose of @README.md and the main folders in this project. Do not change anything.
```

```text
Compare @docs/current-process.md with @docs/proposed-process.md and list the practical differences.
```

### Use a separate terminal pane

A simple arrangement in Windows Terminal is:

| Pane | Command | Purpose |
|---|---|---|
| Superfile pane | `cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library && spf` | Browse and organise files |
| OpenCode pane | `cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library && opencode` | AI-assisted project work |
| Bash/Git pane | `cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library` | Explicit commands and Git checks |

### Do not use OpenCode as `EDITOR`

Do not add this to `.bashrc`:

```bash
export EDITOR="opencode"
```

OpenCode is not a conventional text editor. It is an AI coding agent with a terminal interface, so it should be launched deliberately in the project directory.

The Superfile `e` key is for an ordinary external text editor configured through `EDITOR`. For your normal Superfile plus OpenCode workflow, you can ignore `e` and `E`.

---

## Git Safety

Superfile manages files and folders, but it does not show Git status or explain Git implications.

After important changes, leave Superfile and run:

```bash
git status
git diff
git diff --stat
```

### Moving or renaming tracked files

For significant source, documentation, configuration, or template files already tracked by Git, prefer `git mv`:

```bash
git mv old-name.md new-name.md
```

For moving into another folder:

```bash
git mv old-folder/file.md new-folder/file.md
```

### Deleting a tracked file intentionally

Prefer:

```bash
git rm path/to/file.md
```

Then check:

```bash
git status
```

### Practical rule

Use Superfile freely to browse and handle ordinary assets. For important tracked project files, use Git-aware commands or ask OpenCode to plan and make the change, then inspect it with `git status` and `git diff`.

---

## Optional Shortcuts

### `mpl`: open Superfile in MakePDLF project library

Run once:

```bash
echo "alias mpl='cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library && spf'" >> ~/.bashrc
source ~/.bashrc
```

After that, from any Bash folder, run:

```bash
mpl
```

### `sf`: short name for Superfile

Run once:

```bash
echo "alias sf='spf'" >> ~/.bashrc
source ~/.bashrc
```

Then run:

```bash
sf
```

### Remove an alias later

Open Bash configuration:

```bash
nano ~/.bashrc
```

Delete the alias line, save, exit, then reload:

```bash
source ~/.bashrc
```

---

## Common Problems

### Superfile shows unexpected files

You likely opened the wrong path.

1. Quit Superfile with `q`.
2. Run:

   ```bash
   pwd
   ls -la
   explorer.exe .
   ```

3. Confirm Explorer shows the folder you expect.
4. For MakePDLF, start again with:

   ```bash
   cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
   spf
   ```

### I cannot see `.gitignore` or `.env`

Press:

```text
.
```

### Icons look incorrect

Superfile can use Nerd Font icons. If they display as boxes or misaligned characters, configure a Nerd Font in Windows Terminal or disable icons in Superfile configuration.

### I accidentally opened a prompt or menu

Press:

```text
Esc
```

or:

```text
Ctrl + C
```

### I forgot a key

Press:

```text
?
```

### `spf` says `command not found`

Check whether it is in your Bash path:

```bash
command -v spf
which spf
spf --version
```

---

## Quick Reference

```text
Start Superfile in MakePDLF project library:
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
spf

Move selection:
Up Arrow / Down Arrow

Open folder or file:
Enter

Go to parent folder:
Backspace

Quit:
q

Help:
?

Show or hide hidden files:
.

Search current folder:
/

Copy:
Ctrl + C

Move:
Ctrl + X, then Ctrl + V

Paste:
Ctrl + V

Rename:
Ctrl + R

Create a file or folder:
Ctrl + N
Use a trailing / for a folder

Delete:
Delete or Ctrl + D

Add second panel:
n

Switch panels:
Tab

Open Windows Explorer at current Bash folder:
explorer.exe .

Start OpenCode at project root:
opencode

Check Git status:
git status
```

---

## Official Resources

- Superfile: <https://superfile.dev/>
- Superfile hotkey list: <https://superfile.dev/list/hotkey-list/>
- Superfile custom hotkeys: <https://superfile.dev/configure/custom-hotkeys/>
- Superfile source repository: <https://github.com/yorukot/superfile>
- OpenCode CLI documentation: <https://opencode.ai/docs/cli/>
- OpenCode terminal interface documentation: <https://opencode.ai/docs/tui/>
