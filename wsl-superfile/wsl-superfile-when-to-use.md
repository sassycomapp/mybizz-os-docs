---
title: "wsl-superfile-when-to-use"
doc-id: "mb-8ab80d1e"
updated: "2026-10-10"
---
# WSL superfile when to use

Superfile is not a replacement for Windows Explorer, and you should not force yourself to use it just because it is installed.

Superfile is optional. Its value is narrow: it keeps file navigation and simple file operations **inside the same WSL terminal workflow** as Bash, Git, and OpenCode. If that does not save you time, use Windows Explorer instead.

## The practical answer

For your PDLF workflow, use each tool for what it does best:

| Task                                                                                   | Best tool                |
| -------------------------------------------------------------------------------------- | ------------------------ |
| Browse folders visually; drag/drop; preview PDFs/images; manage ordinary Windows files | **Windows Explorer**     |
| Work with an AI agent on project files                                                 | **OpenCode**             |
| Check Git, run scripts, search content, generate files, run development tools          | **Bash / WSL**           |
| Quickly inspect or transfer files without leaving a terminal session                   | **Superfile** — optional |

You have already found Superfile’s core limitation: it is a keyboard-driven terminal browser, not a richer visual environment. Explorer has a mouse, drag-and-drop, thumbnails, familiar previews, Windows shell integration, context menus, file associations, and generally a lower learning burden for normal file management.

So: **do not use Superfile for normal day-to-day browsing when Explorer is easier.**

## When Superfile is useful

Superfile earns its place only when you are already working in WSL and want to avoid breaking your terminal flow.

### 1. Fast terminal-side orientation

Suppose you are in your project root and OpenCode says:

> “The relevant documentation is under `stepwise/` and `specifications-local/`.”

Rather than switching to Explorer, locating the same directory manually, and returning to the terminal, you can run:

```
cd /mnt/c/pdlf
spf
```

You can then inspect the directory tree, look at adjacent files, and quit with `q` back to Bash.

This is useful only if you are already in a terminal-first work session.

### 2. Two-folder copy or move without opening Explorer

Superfile’s strongest feature is its multiple file panels.

Example: you have generated files under:

```
output-opencode/
```

and need to compare or copy selected output into:

```
docs-local/
```

You can open two panels, one on each folder, and use:

```
Ctrl + C
Tab
Ctrl + V
```

It is a keyboard-centric equivalent of having two Explorer windows side by side. Superfile’s design emphasizes terminal-based navigation, panels, preview, file operations, and a visual layout for that kind of workflow.[[superfile](https://superfile.dev/overview/)]

But if Explorer’s two windows are easier for you, **use Explorer**. There is no prize for doing this in a terminal.

### 3. Working on a remote Linux machine later

This is where terminal file managers become substantially more valuable.

If you SSH into a Linux VPS, cloud server, Raspberry Pi, or another remote machine, you often do **not** have Windows Explorer connected to that machine’s filesystem. A terminal file manager lets you browse, inspect, and transfer files without needing a graphical remote desktop or SFTP client.

For your current local WSL + Windows setup, this benefit is limited. For future self-hosting or cloud work, it is relevant.

### 4. Staying in a keyboard-only terminal workspace

Some developers prefer a split terminal:

```
Left pane:  Superfile
Right pane: OpenCode
Bottom:      Bash / Git
```

That can be efficient after the keybindings become automatic. But it is a preference—not a requirement—and it is not automatically more productive than Explorer plus Windows Terminal.

### 5. Browsing Linux-only directories

If a project, tool cache, Docker/Podman data, configuration file, or script lives in your Linux home directory, Superfile can help you browse it without having to navigate via:

```
\\wsl.localhost\<distro>\
```

in Explorer.

However, Explorer can access WSL Linux files too. So this remains a convenience, not a necessity.

## What Superfile is not for

You should **not** expect Superfile to replace any of these:

| Need                                                              | Better choice                                |
| ----------------------------------------------------------------- | -------------------------------------------- |
| Familiar file management and visual previews                      | Windows Explorer                             |
| Coding/editing                                                    | OpenCode plus a proper editor where required |
| Understanding PDLF                                | OpenCode                                     |
| Searching text throughout the project                             | `rg` in Bash or OpenCode                     |
| Git status, diffs, history, commits                               | Git commands and OpenCode                    |
| File drag/drop, thumbnails, PDFs, Office files, shell integration | Windows Explorer                             |
| Bulk scripted transformations                                     | Bash or Python scripts                       |
| Detailed file comparison                                          | `git diff`, `diff`, or a dedicated diff tool |

In particular, Superfile does **not** make OpenCode better. It does not automatically pass file context to OpenCode. OpenCode already supports fuzzy file references using `@` from the project directory, and can run shell commands prefixed by `!`.[[opencode](https://opencode.ai/docs/ja/tui/)][[opencode](https://opencode.ai/docs/bs/tui/)]

For example, inside OpenCode you can simply write:

```
Review @stepwise/INDEX.md and @specifications-local/requirements.md.
Explain how they relate. Do not modify either file.
```

That is usually more valuable than browsing the same documents manually in Superfile.

## My recommendation for you

For PDLF, I recommend this:

### Default: use Explorer

Use Windows Explorer for:

- Looking around the project
- Opening and reviewing ordinary documents
- Moving images, PDFs, exports, and other assets
- Comparing folders visually
- Deleting or renaming non-code items when you are certain
- Seeing familiar previews and file associations

To jump from WSL to the currently active project folder in Explorer, use:

```
explorer.exe .
```

That is likely the most useful “file manager command” for your actual workflow.

### Development: use OpenCode

Start it at PDLF root:

```
cd /mnt/c/pdlf
opencode
```

Then use OpenCode for the work that matters:

```
Summarise this project structure. Do not alter files.
```

```
Review @stepwise/INDEX.md and identify missing or inconsistent documentation.
```

```
Create a plan for reorganising the documentation. Do not make changes until I approve the plan.
```

OpenCode starts in the current project directory and lets you attach files using `@`, which performs fuzzy file search within that project.[[opencode](https://opencode.ai/docs/ja/tui/)][[dev.opencode](https://dev.opencode.ai/v2/docs/)]

### Keep Superfile, but treat it as optional

Keep Superfile installed if you enjoy experimenting with terminal tools or anticipate server/SSH work. Use it occasionally when you want a terminal-only two-panel file browser.

But do not make it part of your core process yet.

A reasonable rule for now is:

> **Use Explorer by default. Use Superfile only when you are already in WSL and a terminal-side browse/copy operation is plainly quicker.**

If you never encounter that situation, you can safely stop using Superfile. It is not essential to WSL, OpenCode, Git, or PDLF.

## One issue with your current layout

Your project lives under the Windows-mounted path:

```
/mnt/c/pdlf
```

That is perfectly valid, especially if you deliberately want Windows Explorer access. But Microsoft notes that WSL 2’s filesystem performance is best when your project files live on the same filesystem as the tools using them; Linux-side WSL tools are generally faster with projects stored in the Linux filesystem, while cross-filesystem access has more overhead.[[learn.microsoft](https://learn.microsoft.com/en-us/windows/wsl/compare-versions)]

This does **not** mean you should move PDLF now. Because you use Windows Explorer heavily, keeping it on `C:` is a sensible trade-off. Only reconsider it if you later experience noticeably slow Git operations, dependency installs, builds, or large file-tree searches from WSL.

## A simpler working model

Use this as your mental model:

```
Windows Explorer
  = “Show me my project files and let me manage them visually.”

OpenCode
  = “Help me understand, plan, review, and change this project.”

Bash / Git
  = “Run exact commands and verify what changed.”

Superfile
  = “Optional terminal file browser when I do not want to leave WSL.”
```

That is all Superfile needs to be. You do not need to learn every panel, hotkey, or workflow unless it starts delivering a clear benefit.
