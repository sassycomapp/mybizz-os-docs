---
title: "morning-tasks-explainer"
updated: "2026-11-14"
doc-id: "mb-ce374ee8"
---
# Morning Tasks — Explainer

## 1. What it is

Morning Tasks is the one scheduled job that keeps the developer's PC backed up and tidy. It runs each time the developer logs on to Windows, checks which tasks are due, and runs only those. It replaces fixed-time schedules, because the PC is not left on at night and a fixed daytime slot falls in the middle of other work.

## 2. How it works

One minute after logon, the Task Scheduler task `MorningTasks` starts `C:\mybizz\scripts\morning-tasks.ps1` in a window titled "Morning tasks running - do not open WSL yet". The window closes by itself when the tasks are done.

| Task | When it runs | Time | What it does |
|---|---|---|---|
| Daily copy | First logon of each day | About 2 minutes | Copies `C:\dev` and `C:\mybizz` into `C:\backup-daily\<date>`, without Git history. Keeps the three most recent copies |
| OpenCode clean-up | First logon of each day | Seconds | Removes OpenCode session history older than 60 days, after backing it up |
| Weekly backup | First logon 7 or more days after the last successful weekly backup | About 5 minutes | Backs up the PostgreSQL databases (the PDLF ledger, GBrain, the memory governor and both admin databases) to `C:\backup-gbrain` and the WSL home, exports GBrain to `C:\backup-dream-cycle`, then copies the newest database backups to `G:\My Drive\mybizz-backups\database` |
| Ubuntu export | First logon 30 or more days after the last successful export, never on the same morning as the weekly backup | About 15 minutes | Closes WSL and exports the whole of Ubuntu to `G:\My Drive\mybizz-backups\ubuntu-backup.tar`, replacing the previous export only once the new one is complete |

A task that fails is tried again at the next logon. Each task writes one line to `C:\mybizz\logs\morning-tasks\morning-tasks.log`, and the dates of the last successful runs are kept in `state.json` in the same folder.

A pop-up appears only in two cases:

- a task failed: the pop-up says what failed and what to do;
- the weekly backup has just succeeded: the pop-up says that the dream cycle is due.

## 3. Why it works this way

- **Nothing is missed.** A task is due by the number of days since it last succeeded, not by a clock time, so a morning when the PC was off only moves the task to the next logon.
- **Mornings stay short.** The two long tasks never run on the same morning, so a morning takes at most about 15 minutes, and on most days about 2 minutes.
- **A copy lives off the PC.** The databases and the whole of Ubuntu are copied to Google Drive, so a failed disk does not lose them.
- **Disk space stays bounded.** The daily copies keep three days, the database backups keep two weeks, and the Ubuntu export keeps one copy.
- **Nothing is deleted before its replacement exists.** Old daily copies are removed only after the new copy succeeds, and the Ubuntu export replaces the old file only once the new export is complete.
- **The dream cycle stays manual.** It stops for the developer's yes at each stage, so it cannot run unattended; the pop-up says when it is due.

## 4. What has to happen

- **Log on and wait for the window to close** before opening WSL. On an export morning, this takes about 15 minutes.
- **Keep Google Drive for desktop running**, so that `G:\My Drive` exists at logon.
- **When the pop-up says the dream cycle is due:** in OpenCode (build mode), run `/mb-memory-system-diagnostic` and answer yes to the dream cycle at its end.
- **When the pop-up reports a problem:** follow its instruction, then log off and on again. If the problem stays, paste `morning-tasks.log` to Claude.
- **To set up the task on a new PC:** in PowerShell (Admin), run `C:\mybizz\scripts\register-morning-tasks.ps1`.
