---

title: "wsl-superfile-ui-reference"
doc-id: "mb-8680c2bd"
updated: "2026-10-06"

---

# wsl-superfile-ui-reference

![](C:/Users/dev-p/AppData/Roaming/marktext/images/2026-10-04-12-21-57-image.png)

Your screenshot is clear enough to identify the layout. You are viewing the **MakePDLF project library**, and Superfile is divided into a sidebar, a main file browser, a preview area, metadata, processes, and clipboard panels.

## Reading your screen

### 1. Sidebar: left side

The far-left column contains navigation shortcuts:

```
superfile
Home
Trash
Pinned
Disks
/
distro
```

Think of this as Superfile’s navigation menu.

- **Home** — your Linux/WSL home directory.
- **Trash** — items moved to the trash, if Superfile and the filesystem support it.
- **Pinned** — folders you have deliberately saved for quick access.
- **Disks** — available filesystem locations.
- `/` — the Linux filesystem root.
- **distro** — likely your WSL distribution or a mounted filesystem entry.

You can move focus to the sidebar using:

```
s
```

Then use the arrow keys and `Enter` to select a location.

For now, you can mostly ignore the sidebar because your project path is already open in the main browser.

## 2. Main browser: centre

The large central area is your primary file browser.

At the top, you can see:

```
/mnt/c/dev/dev-makepdlf/makepdlf-project-library
```

That confirms you are in the correct MakePDLF project-library directory.

The visible folders include items such as:

```
agents
checklists-local
docs-local
obsolete
output-gstack
output-opencode
specifications-local
stepwise
templates-local
workspace
```

This is the area where you will do most of your work.

### Navigate in the main browser

Use:

| Action                    | Key          |
| ------------------------- | ------------ |
| Move selection up         | `Up Arrow`   |
| Move selection down       | `Down Arrow` |
| Open selected folder      | `Enter`      |
| Go up one folder          | `Backspace`  |
| Show or hide dotfiles     | `.`          |
| Search the current folder | `/`          |

The highlighted item is the item currently selected. In your screenshot, `stepwise` appears to be selected.

### What the `17/25` means

Near the bottom of the main browser, you can see:

```
17/25
```

This means the selected item is number 17 out of 25 items in that panel. It is not an error.

## 3. Right side: preview/browser panel

The right-hand panel is showing the contents of the selected `stepwise` folder.

It includes files such as:

```
INDEX.md
run-plan.md
step-01-orchestrator.md
step-05-undefined.md
step-11-decision-tree-v2.html
step-15-plan-eng-review.md
```

This is useful because Superfile can show the contents of a selected folder or preview a selected file without you having to open it fully.

For example:

1. Highlight `stepwise` in the central browser.
2. The right panel displays its contents.
3. Move the selection to a file such as `INDEX.md`.
4. Superfile may show a text preview or file details, depending on your current focus and preview settings.
5. Press `Enter` only when you actually want to open the folder or file.

The selected item is not changed merely by highlighting it. You must deliberately press an operation key.

## 4. Metadata panel: bottom centre

The panel labelled **Metadata** shows information about the currently selected item.

Your screenshot shows:

```
Name       stepwise
Size       512 B
Date Modified ...
Permissions drwxrwxrwx
Owner      dev-p
Group      dev-p
```

Here is what those values mean:

| Field         | Meaning                                                         |
| ------------- | --------------------------------------------------------------- |
| Name          | The selected file or folder                                     |
| Size          | Its size; directory sizes may not represent all contained files |
| Date Modified | When it was last changed                                        |
| Permissions   | Linux permission flags                                          |
| Owner         | The Linux/WSL owner                                             |
| Group         | The Linux/WSL group                                             |

The permission value begins with:

```
d
```

That means `stepwise` is a directory.

For a regular file, the first character would usually be:

```
-
```

The remaining characters describe read, write, and execute permissions.

Because this folder is on the Windows-mounted `/mnt/c` filesystem, WSL may display broad or synthetic permissions such as:

```
drwxrwxrwx
```

That is normal for many Windows-mounted WSL paths and does not necessarily mean that the Windows folder is unrestricted.

## 5. Processes panel: bottom left

The panel labelled **Processes** currently says:

```
No processes running
```

This is normal.

Superfile uses this area to show operations that are running or waiting, such as:

- Copying a large folder
- Moving many files
- Compressing files
- Extracting an archive
- Deleting a large amount of data

For ordinary navigation, it will usually remain empty.

If you start a large copy and see a process appear, allow it to complete before quitting Superfile or closing the terminal.

## 6. Clipboard panel: bottom right

The panel labelled **Clipboard** currently says:

```
No content in clipboard
```

This is Superfile’s internal file-operation clipboard.

It is separate from the normal Windows text clipboard.

When you select a file and press:

```
Ctrl + C
```

Superfile places that file into its file-operation clipboard.

Then navigate to a destination folder and press:

```
Ctrl + V
```

The file is copied there.

For a move:

```
Ctrl + X
```

Then navigate to the destination and press:

```
Ctrl + V
```

The Clipboard panel should then show the item waiting to be pasted.

### Important distinction

Do not assume `Ctrl + C` copies text into Windows clipboard. In this context it is a **file copy operation**.

The Clipboard panel is telling you which files Superfile has marked for a copy or move operation.

## 7. Bottom footer

The lower centre of the interface shows the current panel's controls and status.

Your screenshot contains labels resembling:

```
Name
Browser
```

This is related to the currently focused area and the available panel or sorting mode.

The exact footer content can vary depending on:

- Your Superfile version
- Which panel currently has focus
- The selected item
- Whether you are in normal or selection mode
- The active sort order

Press `?` to see the official key reference for your installed version.

## Your normal working area

For your use, think of the screen like this:

```
+----------------+---------------------------+-------------------+
|                |                           |                   |
| Navigation     | Main project browser     | Preview/browser   |
| sidebar        |                           | panel             |
|                |                           |                   |
+----------------+---------------------------+-------------------+
| Processes       | Metadata                  | Clipboard         |
+----------------+---------------------------+-------------------+
```

Your main workflow is:

1. Use the **central browser** to move through the project.
2. Use the **right panel** to inspect the selected folder or file.
3. Use the **Metadata** panel to confirm what is selected.
4. Use the **Clipboard** panel to check pending copy or move operations.
5. Use the **Processes** panel to monitor longer operations.

## A safe first exercise

You can practise without changing anything.

### Exercise 1: inspect a folder

1. Confirm the top path is:
   
   ```
   /mnt/c/dev/dev-makepdlf/makepdlf-project-library
   ```

2. In the central browser, highlight:
   
   ```
   stepwise
   ```

3. Look at the right-hand panel.

4. Confirm that it contains files such as:
   
   ```
   INDEX.md
   run-plan.md
   step-01-orchestrator.md
   ```

5. Press `Enter` to open `stepwise`.

6. Press `Backspace` to return to the project-library root.

Nothing is changed by merely browsing.

### Exercise 2: inspect a Markdown file

1. Open the `stepwise` folder.
2. Highlight `INDEX.md`.
3. Look at the metadata and preview areas.
4. Do not press a copy, move, rename, or delete key.
5. Press `Backspace` when finished.

## How to use it with OpenCode

The most useful relationship for you is:

```
Superfile = find and inspect
OpenCode  = understand, plan, and modify
Bash      = verify and run explicit commands
```

### Example workflow

Suppose you find this file in Superfile:

```
stepwise/INDEX.md
```

Use the following process:

1. In Superfile, inspect the file and note its path.

2. Press `q` to leave Superfile, or use another Windows Terminal pane.

3. Start OpenCode from the project root:
   
   ```
   cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
   opencode
   ```

4. Ask OpenCode:
   
   ```
   Review @stepwise/INDEX.md. Explain its purpose and identify any inconsistencies. Do not change the file yet.
   ```

OpenCode can search for the file after you type `@` and begin entering its project-relative path. OpenCode is designed to be started in a project directory and provides a terminal interface for working with that project.[[opencode](https://opencode.ai/docs/cli/)][[opencode](https://opencode.ai/docs/tui/)]

### Recommended two-pane arrangement

Use separate Windows Terminal panes:

```
Pane 1:
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
spf
```

```
Pane 2:
cd /mnt/c/dev/dev-makepdlf/makepdlf-project-library
opencode
```

This lets you browse with Superfile while keeping OpenCode available for analysis and changes.

## The keys you will use most

| Purpose                       | Key                         |
| ----------------------------- | --------------------------- |
| Move through the main browser | `Up Arrow`, `Down Arrow`    |
| Open a folder                 | `Enter`                     |
| Return to parent folder       | `Backspace`                 |
| Show or hide hidden files     | `.`                         |
| Search                        | `/`                         |
| Help                          | `?`                         |
| Copy a file                   | `Ctrl + C`                  |
| Move a file                   | `Ctrl + X`, then `Ctrl + V` |
| Paste a file                  | `Ctrl + V`                  |
| Rename                        | `Ctrl + R`                  |
| Create                        | `Ctrl + N`                  |
| Delete                        | `Delete` or `Ctrl + D`      |
| Add another file panel        | `n`                         |
| Switch panels                 | `Tab`                       |
| Quit                          | `q`                         |

## What to avoid for now

Until the interface feels familiar, avoid these operations on important project files:

- `Shift + D` for permanent deletion
- `Ctrl + D` on a selected folder
- Bulk selection with `A`
- Moving large directory trees
- Editing `.git`, `.env`, or configuration files
- Using `Ctrl + X` and `Ctrl + V` without checking the destination panel
- Using shell commands from the `:` prompt unless you understand the command

For now, use Superfile mainly to navigate, inspect, and locate files. Use OpenCode for changes, and check the result with:

```
git status
git diff
```

Superfile is specifically designed to provide a visual terminal file manager with multiple panels, copy/move operations, previews, metadata, and a clipboard area, so the division you see in the screenshot is expected rather than a sign that anything is wrong.[[superfile](https://superfile.dev/zh-tw/notice)][[superfile](https://superfile.dev/overview/)]

## One important observation

Your screenshot shows that Superfile is already working correctly:

- The top path is your intended MakePDLF project library.
- The central list contains your project folders.
- The right panel displays the selected folder’s contents.
- The metadata panel identifies the selected folder.
- The process and clipboard panels are simply idle because no operation is currently running.

You can treat the **central browser** as your main working area and the other panels as information and status panels.
