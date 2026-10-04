---
title: "wsl-superfile-reference"
doc-id: "mb-4576a90a"
updated: "2026-10-04"
---
# WSL Superfile Reference

A practical beginner guide to **Superfile** (`spf`) for browsing and managing files from the WSL Bash terminal.

> **Important:** Superfile works on real files. Copying, moving, renaming, and deleting in Superfile changes the actual files on disk—just as if you did it in Windows Explorer.

---

## What Superfile is

**Superfile** is a terminal file manager. It gives you a visual way to browse folders and manage files inside a Bash/WSL terminal window.

It is similar in purpose to Windows Explorer, but it is keyboard-driven and runs inside the terminal.

Use Superfile when you want to:

- Browse folders without repeatedly typing `cd` and `ls`
- Open project files in your editor
- Copy, move, rename, create, or delete ordinary files
- Compare or transfer files between two folders using multiple panels
- Inspect hidden files such as `.gitignore` and `.env`
- Search within a folder
- Work inside WSL without opening a separate Linux GUI application

Superfile starts with this command:

```bash
spf
```

---

## Paths

### Launch Superfile with the MakePDLF project library

Run these commands in your WSL Bash terminal:

```bash
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
spf
```

This opens Superfile at the MakePDLF project-library folder.

### Project path reference

Windows Explorer path:

```text
C:\dev\dev-makepdlf\makepdlf-project-library
```

Equivalent WSL Bash path:

```text
/mnt/c/dev/dev-makepdlf/makepdlf-project-library
```

### Understand the Windows-to-WSL path conversion

In WSL, Windows drive letters are normally mounted under `/mnt`.

| Windows path                                   | WSL path                                           |
| ---------------------------------------------- | -------------------------------------------------- |
| `C:\`                                          | `/mnt/c/`                                          |
| `C:\dev`                                       | `/mnt/c/dev`                                       |
| `C:\dev\dev-makepdlf`                          | `/mnt/c/dev/dev-makepdlf`                          |
| `C:\dev\dev-makepdlf\makepdlf-project-library` | `/mnt/c/dev/dev-makepdlf/makepdlf-project-library` |

Windows uses backslashes: `\`

WSL/Linux uses forward slashes: `/`

### Confirm that you are in the correct folder

Before starting Superfile, use:

```bash
pwd
ls -la
```

Expected `pwd` output:

```text
/mnt/c/dev/dev-makepdlf/makepdlf-project-library
```

To open the exact current WSL folder in Windows Explorer:

```bash
explorer.exe .
```

The folder shown by Superfile should match the folder opened by `explorer.exe .`.

---

## Starting and quitting

### Start Superfile

From your current folder:

```bash
spf
```

From a specific folder:

```bash
spf /mnt/c/dev/dev-makepdlf/makepdlf-project-library
```

Recommended everyday method:

```bash
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
spf
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

You will return to the Bash terminal.

### Get built-in help

While Superfile is open, press:

```text
?
```

This opens Superfile’s own hotkey reference. It is the quickest way to check the keys available in the version you have installed.

---

## Screen layout

Superfile normally has several areas.

| Area          | What it does                                                       |
| ------------- | ------------------------------------------------------------------ |
| Sidebar       | Provides shortcuts such as Home, pinned folders, and mounted disks |
| File panel    | Displays the files and folders in the current location             |
| Preview panel | Shows a preview of the selected file where supported               |
| Metadata area | May show file information such as size, type, and dates            |
| Footer        | Shows status information, sort settings, and selection mode        |

The highlighted row is the currently selected file or folder.

A folder is a directory containing other files and folders.

---

## Basic navigation

These are the only navigation keys you need to learn first.

| What you want to do     | Key                               |
| ----------------------- | --------------------------------- |
| Move up one item        | `Up Arrow` or `k`                 |
| Move down one item      | `Down Arrow` or `j`               |
| Move up one page        | `Page Up`                         |
| Move down one page      | `Page Down`                       |
| Open a folder or file   | `Enter`, `Right Arrow`, or `l`    |
| Go to the parent folder | `Backspace`, `Left Arrow`, or `h` |
| Quit Superfile          | `q` or `Esc`                      |
| Show the help screen    | `?`                               |

### Example: browse into a folder

1. Use the `Up Arrow` or `Down Arrow` key to highlight a folder.
2. Press `Enter`.
3. Superfile opens that folder and displays its contents.
4. Press `Backspace` to return to the folder above it.

### Recommended beginner habit

Use the arrow keys and `Enter` first. You do not need to use Vim-style keys such as `j`, `k`, `h`, and `l` unless you want to.

---

## Hidden files

On Linux and WSL, files beginning with a dot are hidden by default.

Common examples:

```text
.git
.gitignore
.env
.env.example
.vscode
```

To show or hide dotfiles in Superfile, press:

```text
.
```

Press `.` again to switch them back off.

### Why this matters for projects

A `.gitignore` file controls which files Git ignores.

A `.git` folder contains Git repository data.

An `.env` file often contains local environment settings or secrets. Treat it carefully and do not copy it into public repositories unless you deliberately intend to do so.

---

## Open files safely

### Open a folder

Highlight a folder and press:

```text
Enter
```

### Open a file

Highlight a file and press:

```text
Enter
```

What happens depends on the file type and your Superfile/editor configuration.

For code, Markdown, text, JSON, and configuration files, it is usually preferable to use your editor rather than trying to edit content inside Superfile.

### Open a file in your configured editor

Highlight a file and press:

```text
e
```

By default, Superfile uses the `EDITOR` environment variable if one is configured. Otherwise, Linux commonly falls back to a terminal editor such as `nano`.

### Open the current folder in your configured editor

Press:

```text
E
```

This is useful if you configure VS Code as your editor.

### Recommended editor setting for VS Code

If you use VS Code with WSL, add this to your Bash configuration:

```bash
echo 'export EDITOR="code --wait"' >> ~/.bashrc
source ~/.bashrc
```

Then, in Superfile:

- Press `e` to open the selected file in VS Code
- Press `E` to open the current folder in VS Code

`--wait` tells the terminal command to wait until the opened VS Code file is closed before continuing.

### Check your current editor setting

```bash
echo "$EDITOR"
```

---

## Search and sorting

### Search within the current folder

Press:

```text
/
```

Then type part of a file or folder name.

For example, to find a document called:

```text
makepdlf-design-notes.md
```

You could type:

```text
design
```

Press `Esc` to cancel the search.

> Search helps you filter or find items in the current folder. It is not the same as searching every file in the full project.

### Change sorting

Press:

```text
o
```

Use the sort menu to choose how items are ordered, such as by:

- Name
- Size
- Modified date
- File type
- Natural order

### Reverse the current sort order

Press:

```text
R
```

For example, if sorting by date, reverse order can put the newest items first.

---

## Select multiple files

Superfile has two modes:

| Mode           | Purpose                                               |
| -------------- | ----------------------------------------------------- |
| Normal mode    | Browse folders and work with one current item         |
| Selection mode | Mark multiple files or folders for a single operation |

### Enter or leave selection mode

Press:

```text
v
```

Press `v` again to return to normal mode.

### Select additional items

While in selection mode:

| What you want to do                   | Key                         |
| ------------------------------------- | --------------------------- |
| Extend selection down                 | `Shift + Down Arrow` or `J` |
| Extend selection up                   | `Shift + Up Arrow` or `K`   |
| Select all items in the current panel | `A`                         |

### Beginner safety rule

Before copying, moving, or deleting multiple items:

1. Look carefully at what is selected.
2. Confirm you are in the correct folder.
3. Use the lowest-risk operation first.
4. Avoid selecting project source files unless you know why you are changing them.

---

## Copy, move, rename, create, delete

These actions affect your real Windows/WSL files.

### Copy files or folders

1. Highlight the item you want to copy.
2. For multiple items, enter selection mode with `v` and select items.
3. Press:

```text
Ctrl + C
```

4. Navigate to the destination folder.
5. Press:

```text
Ctrl + V
```

This creates a copy and leaves the original in place.

### Move files or folders

1. Highlight the item you want to move.
2. Press:

```text
Ctrl + X
```

3. Navigate to the destination folder.
4. Press:

```text
Ctrl + V
```

This removes the original after moving it to the destination.

### Rename a file or folder

1. Highlight the item.
2. Press:

```text
Ctrl + R
```

3. Type the new name.
4. Press `Enter` to confirm.
5. Press `Esc` to cancel.

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

Type the folder name ending in a forward slash:

```text
research/
```

Then press `Enter`.

Superfile interprets the ending `/` as “create a folder.”

### Delete a file or folder

Highlight the item and press either:

```text
Delete
```

or:

```text
Ctrl + D
```

Superfile should ask for confirmation.

### Permanently delete a file or folder

Press:

```text
Shift + D
```

> **Warning:** Permanent deletion is more dangerous than ordinary deletion. Do not use it unless you are certain the item is no longer required and you understand how recovery works for the underlying filesystem.

### Safe deletion rule

For work in a Git repository:

- Do not casually delete project files from Superfile.
- Check `git status` after any significant file operation.
- Prefer `git mv` for renaming or moving tracked files.
- Prefer Git-aware deletion, such as `git rm`, when intentionally removing a tracked file.
- Use Superfile mainly for browsing, opening, and managing non-code assets unless you are confident about the Git implications.

---

## Work with two folders

One of Superfile’s most useful features is opening multiple file panels.

This is useful when you want to copy or move files between folders.

### Create another file panel

Press:

```text
n
```

A new file panel opens.

### Switch between panels

Press:

```text
Tab
```

You can also use:

```text
Shift + L
```

to move to the next panel, or:

```text
Shift + H
```

to move to the previous panel.

### Close the current panel

Press:

```text
w
```

### Example: copy an image into a project folder

1. Open Superfile in the project library.
2. Press `n` to create a second panel.
3. In the first panel, navigate to the source folder containing the image.
4. Press `Tab` to switch to the second panel.
5. Navigate to the destination folder in the MakePDLF project library.
6. Press `Tab` to return to the source panel.
7. Highlight the image.
8. Press `Ctrl + C`.
9. Press `Tab` to switch to the destination panel.
10. Press `Ctrl + V`.

Before pasting, confirm that the correct destination panel is active.

---

## Preview and file information

### Toggle the preview panel

Press:

```text
f
```

This shows or hides the file-preview panel.

Previews work best for text-based files, such as:

```text
.md
.txt
.json
.toml
.yaml
.yml
.py
.js
.ts
html
css
```

Preview support for images, PDFs, Office documents, and other binary formats depends on your Superfile version, terminal capabilities, and installed helper tools.

### Focus the metadata panel

Press:

```text
m
```

Metadata can include file size, type, modified date, and permissions.

### Focus the sidebar

Press:

```text
s
```

### Focus the process/status panel

Press:

```text
p
```

---

## Useful path commands

### Copy the selected item’s path

Highlight a file or folder and press:

```text
Ctrl + P
```

This copies the path of the selected item.

### Copy the current folder path

Press:

```text
c
```

This copies the current working directory path.

This is useful when you need to paste a path into:

- A Bash command
- VS Code
- A Git command
- Documentation
- A prompt for an AI coding assistant

### Open a shell command prompt in the current folder

Press:

```text
:
```

This opens Superfile’s shell-command prompt.

For example, you could run:

```bash
git status
```

or:

```bash
ls -la
```

Use this feature cautiously as a beginner. It runs commands in the real current folder.

### Open Superfile command mode

Press:

```text
>
```

This opens Superfile’s own command prompt rather than a normal shell prompt.

### Pin the current folder

Press:

```text
P
```

This pins or unpins the current folder in the Superfile sidebar.

Useful folders to pin may include:

```text
/mnt/c/dev/dev-makepdlf/makepdlf-project-library
/mnt/c/dev/dev-makepdlf
/mnt/c/Users/<your-Windows-user>/Downloads
```

---

## Key reference

### Essential beginner keys

| Task                          | Key                         |
| ----------------------------- | --------------------------- |
| Move up                       | `Up Arrow`                  |
| Move down                     | `Down Arrow`                |
| Open selected folder or file  | `Enter`                     |
| Go up one folder              | `Backspace`                 |
| Quit Superfile                | `q` or `Esc`                |
| Help                          | `?`                         |
| Show/hide hidden files        | `.`                         |
| Search in current folder      | `/`                         |
| Show/hide preview             | `f`                         |
| Create a file or folder       | `Ctrl + N`                  |
| Rename                        | `Ctrl + R`                  |
| Copy                          | `Ctrl + C`                  |
| Move                          | `Ctrl + X`, then `Ctrl + V` |
| Paste                         | `Ctrl + V`                  |
| Delete                        | `Delete` or `Ctrl + D`      |
| Open selected file in editor  | `e`                         |
| Open current folder in editor | `E`                         |
| Add a second panel            | `n`                         |
| Switch panels                 | `Tab`                       |
| Show sort options             | `o`                         |

### Full useful key list

| Task                                  | Key                                     |
| ------------------------------------- | --------------------------------------- |
| Confirm/open selected item            | `Enter`, `Right Arrow`, or `l`          |
| Go to parent folder                   | `Backspace`, `Left Arrow`, or `h`       |
| Scroll one item up/down               | `Up Arrow` / `Down Arrow`, or `k` / `j` |
| Scroll one page up/down               | `Page Up` / `Page Down`                 |
| Quit                                  | `q` or `Esc`                            |
| Quit and retain final folder in Bash  | `Q` after optional configuration        |
| Help                                  | `?`                                     |
| Search                                | `/`                                     |
| Toggle hidden dotfiles                | `.`                                     |
| Toggle preview panel                  | `f`                                     |
| Sort options                          | `o`                                     |
| Reverse sort                          | `R`                                     |
| Create file/folder                    | `Ctrl + N`                              |
| Rename                                | `Ctrl + R`                              |
| Copy                                  | `Ctrl + C`                              |
| Cut/move                              | `Ctrl + X`                              |
| Paste                                 | `Ctrl + V` or `Ctrl + W`                |
| Delete                                | `Delete` or `Ctrl + D`                  |
| Permanently delete                    | `Shift + D`                             |
| Zip/compress                          | `Ctrl + A`                              |
| Extract archive                       | `Ctrl + E`                              |
| Open selected file with editor        | `e`                                     |
| Open folder with editor               | `E`                                     |
| Add a file panel                      | `n`                                     |
| Close focused file panel              | `w`                                     |
| Next panel                            | `Tab` or `Shift + L`                    |
| Previous panel                        | `Shift + Left Arrow` or `Shift + H`     |
| Enter/exit selection mode             | `v`                                     |
| Select all in selection mode          | `A`                                     |
| Pin/unpin current folder              | `P`                                     |
| Copy selected item path               | `Ctrl + P`                              |
| Copy current folder path              | `c`                                     |
| Open shell prompt                     | `:`                                     |
| Open Superfile prompt                 | `>`                                     |
| Open zoxide navigation, if configured | `z`                                     |

---

## MakePDLF everyday workflow

This is a safe, simple workflow for ordinary use.

### Open the project library

```bash
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
spf
```

### Browse and inspect

1. Use arrow keys to move through folders.
2. Press `Enter` to open a folder.
3. Press `Backspace` to go back.
4. Press `.` if you need to see hidden files.
5. Press `f` if you want to hide or show previews.
6. Press `/` to find a file by name.

### Open a document or project file

1. Highlight the file.
2. Press `e`.
3. Edit in your configured editor.
4. Return to Superfile when finished.

### Copy an ordinary asset safely

This is suitable for an image, PDF, export, diagram, or other non-source asset.

1. Navigate to the source location.
2. Highlight the asset.
3. Press `Ctrl + C`.
4. Navigate to the destination folder.
5. Press `Ctrl + V`.
6. Confirm the copied file appears in the destination folder.

### Finish work

1. Press `q` to leave Superfile.
2. Run:

```bash
git status
```

3. Read the list of changed, added, moved, or deleted files.

---

## Git safety for project files

Superfile understands files and folders. It is not a Git client.

That means it can move, rename, and delete files, but it does not automatically explain the effect on a Git repository.

### After changing project files

Run:

```bash
git status
```

Useful Git checks:

```bash
git status
git diff
git diff --stat
```

### Rename or move a tracked file

For important source files, templates, configuration files, or documentation that is already tracked by Git, prefer:

```bash
git mv old-name.md new-name.md
```

For a move into another folder:

```bash
git mv old-folder/file.md new-folder/file.md
```

Then check:

```bash
git status
```

### Delete a tracked file intentionally

Prefer:

```bash
git rm path/to/file.md
```

Then check:

```bash
git status
```

### Why this is useful

Git can usually recognise moves and renames after the fact, but using `git mv` and `git rm` makes your intent explicit and avoids confusion when reviewing changes.

---

## Optional shortcut commands

### Add a short `mpl` command

This opens Superfile directly in the MakePDLF project library.

Run once:

```bash
echo "alias mpl='cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library && spf'" >> ~/.bashrc
source ~/.bashrc
```

Then use:

```bash
mpl
```

### Add a generic short `sf` command

Run once:

```bash
echo "alias sf='spf'" >> ~/.bashrc
source ~/.bashrc
```

Then use:

```bash
sf
```

### Remove an alias later

Open your Bash configuration:

```bash
nano ~/.bashrc
```

Find the alias line, delete it, then save and exit.

Reload Bash:

```bash
source ~/.bashrc
```

---

## Optional: stay in the final folder

Normally, when you quit Superfile, Bash remains in the folder where you originally launched Superfile.

For example:

```bash
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
spf
```

If you browse to another folder and quit, Bash normally remains at:

```text
/mnt/c/dev/dev-makepdlf/makepdlf-project-library
```

Superfile can be configured so that, when you quit, Bash changes into the last folder you visited.

This is useful, but it is optional. Leave it alone until you are comfortable using Superfile.

### Enable “stay in the final folder”

First, find Superfile’s configuration paths:

```bash
spf pl
```

Then edit Superfile’s configuration file and set:

```toml
cd_on_quit = true
```

Add this function to `~/.bashrc`:

```bash
spf() {
  export SPF_LAST_DIR="${XDG_STATE_HOME:-$HOME/.local/state}/superfile/lastdir"
  command spf "$@"
  [ ! -f "$SPF_LAST_DIR" ] || {
    . "$SPF_LAST_DIR"
    rm -f -- "$SPF_LAST_DIR" > /dev/null
  }
}
```

Reload Bash:

```bash
source ~/.bashrc
```

After this setup, when you quit Superfile, your Bash terminal will change to the last folder you were browsing.

---

## Configuration locations

To see the locations used by your installed Superfile version, run:

```bash
spf pl
```

This should show paths for items such as:

- Main configuration file
- Hotkey configuration file
- Themes
- Plugins
- Logs
- State files

On Linux/WSL, configuration is commonly stored under:

```text
~/.config/superfile/
```

State data is commonly stored under:

```text
~/.local/state/superfile/
```

Do not edit configuration files until you have a copy or backup.

### Safe configuration backup

Before editing configuration, create a backup:

```bash
cp ~/.config/superfile/config.toml ~/.config/superfile/config.toml.backup
```

If you change hotkeys, make a backup first:

```bash
cp ~/.config/superfile/hotkeys.toml ~/.config/superfile/hotkeys.toml.backup
```

---

## Common problems

### Superfile shows the wrong files

You probably launched it from the wrong directory.

Exit with:

```text
q
```

Then check your current location:

```bash
pwd
ls -la
```

Open that same WSL folder in Windows Explorer:

```bash
explorer.exe .
```

If Explorer and Superfile show the same files, Superfile is working correctly.

For the MakePDLF project library, use:

```bash
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
spf
```

### I cannot see `.gitignore` or `.env`

Press:

```text
.
```

This toggles hidden dotfiles.

### Icons look wrong or appear as boxes

Superfile can use Nerd Font icons. If the terminal font does not support them, icons may look incorrect.

Options:

- Configure Windows Terminal to use a Nerd Font.
- Or disable Nerd Font icons in Superfile’s config:

```toml
nerdfont = false
```

### I accidentally entered a prompt or menu

Press:

```text
Esc
```

or:

```text
Ctrl + C
```

This generally cancels typing or closes a temporary prompt.

### I do not remember a key

Press:

```text
?
```

Use the built-in hotkey list rather than guessing.

### I want to see the current folder in Windows Explorer

While inside Superfile, copy the current folder path with:

```text
c
```

Or quit Superfile and run:

```bash
explorer.exe .
```

### `spf` says “command not found”

Check whether Superfile is available:

```bash
command -v spf
```

If that returns nothing, it is not in your current Bash `PATH`.

Also try:

```bash
which spf
spf --version
```

---

## Safety checklist

Before using copy, move, rename, or delete:

- Confirm that you are in the intended project and folder.
- Check the exact item highlighted or selected.
- Use copy before move when you are uncertain.
- Do not permanently delete unless you are certain.
- Do not casually alter `.git`, `.env`, `.vscode`, or configuration files.
- Use `git status` after significant work in a Git repository.
- Use `explorer.exe .` when you need to visually confirm the matching Windows folder.
- Press `?` instead of guessing unfamiliar keys.

---

## Quick-start card

```text
Start at MakePDLF project library:
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
spf

Move:
Up/Down Arrow

Open:
Enter

Go back:
Backspace

Quit:
q

Help:
?

Hidden files:
.

Search:
/

Open selected file in editor:
e

Copy:
Ctrl+C

Move:
Ctrl+X, then Ctrl+V

Paste:
Ctrl+V

Rename:
Ctrl+R

Create file/folder:
Ctrl+N
Use a trailing / for a folder name

Delete:
Delete or Ctrl+D

Second panel:
n

Switch panels:
Tab

Git check after changes:
git status
```

---

## Official references

- Superfile website: <https://superfile.dev/>
- Superfile hotkey list: <https://superfile.dev/list/hotkey-list/>
- Superfile custom hotkeys: <https://superfile.dev/configure/custom-hotkeys/>
- Superfile source repository: <https://github.com/yorukot/superfile>

Superfile provides default keybindings for navigation, multiple panels, searching, selection mode, copy/move/paste, file and folder creation, rename, deletion, path copying, editor integration, archive handling, previews, and configurable shortcuts. [61][66]
