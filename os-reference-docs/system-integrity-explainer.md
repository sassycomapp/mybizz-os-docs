---
title: "system-integrity-explainer"
doc-id: "mb-91e45ce5"
updated: "2026-10-14"
---
# System Integrity — Explainer

*What System Integrity is, why it exists, which systems make it up, how each one works, why it works that way, and what has to happen for it to keep working. Each system has its own section and its own document suite, which holds the depth. This document holds the whole picture and points to them.*

---

## How to read this document

Section 1 says what System Integrity is. Section 2 says why it exists. Section 3 lists the systems. Section 4 shows how they work together. Sections 5 to 10 describe each system on its own, in the same order every time:

- **What it is**
- **Intent:** the target state (**Target**) and what must change to reach it (**Remediation**)
- **How it works**
- **Why it works this way**
- **What has to happen**
- **Where it stops** (what it does not do, and which system does that instead)
- **Its documents**

Section 11 states the rules and intents that every one of these systems shares.

"How it works" and the sections after it describe each system as it runs today. "Intent" describes what it must become. Where the two differ, Intent governs, and the Remediation list names each change still to be made. When a remediation is done, it is removed and the description is updated.

In this document the word **system** means any installed skill, any agent, or any script-based process. It is a loose collective term for all three.

---

## 1. What System Integrity is

**System Integrity is the group of systems that keeps every other system in MyBizz-OS trustworthy.** None of these systems builds anything. They exist so that the systems that do build, and the documents that describe them, can be relied on without the developer checking everything by hand.

System Integrity has four jobs:

1. **Documented truly.** Every system has documents that say what it is, how it is set, and how to work with it, and those documents are true.
2. **Configured correctly.** Every system is set the way its issuer and the developer intend, and stays that way.
3. **Governed safely.** No agent can take a forbidden action, whatever it believes or has agreed to.
4. **Recorded reliably.** Work, decisions and lessons are saved where they will be found again, and every document enters history in a correct form.

The six systems are:

- **mb-sysdoc-creator:** writes the documents for each system.
- **Cupcake**, with its rule-building skill **mb-compile-rules:** enforces the rules on every agent action.
- **mb-config-manager:** checks each system against its documents and proposes corrections.
- **mb-doc-check:** keeps every document and link correct at the moment it is committed.
- **mb-project-docs:** keeps each project's own documents current.
- **mb-close-out**, with the script **commit-all:** saves a session's work, facts and changes.

---

## 2. Why it exists

The developer's working time goes into building PDLF and the applications PDLF produces. There is no time left to keep watch over the systems underneath: whether a policy engine still enforces, whether a document still matches reality, whether a skill still works, or whether a session's decisions were saved.

Those systems degrade quietly. A setting changes and the document describing it does not. A rule file loads but never fires. A link points to a document that was renamed. A decision made in a session is lost when the conversation ends. The common failure is always the same: **something looks present but is not working**, and nobody notices until it matters.

AI agents make this worse in a specific way. An agent forgets everything between sessions. It follows a written rule only when it remembers to. It reports what it believes it did, not what it did. A developer who cannot audit code cannot catch these failures by reading.

System Integrity replaces watchfulness with structure:

- Documents are produced by research, not by copying claims (mb-sysdoc-creator).
- Rules are enforced by a machine, not by the agent's good intentions (Cupcake).
- Systems are checked against their records, and corrections are proposed (mb-config-manager).
- Documents are corrected automatically as they are committed (mb-doc-check).
- Project documents are brought up to date at the end of each session (mb-project-docs).
- Sessions end with everything saved and committed (mb-close-out and commit-all).

The governing sentence of MyBizz-OS applies here in full: **the developer decides, the agent executes, the records remember, and the machine enforces.**

---

## 3. The systems at a glance

| System | Kind | Its one job | When it runs | Who starts it |
|---|---|---|---|---|
| mb-sysdoc-creator | Skill | Write a system's config, reference and explainer, researched and verified | When a system needs documenting or re-documenting | The developer, by name |
| Cupcake | Installed product (plugin + policy engine) | Allow or block every agent tool call against compiled rules | On every agent tool call | Always on |
| mb-compile-rules | Skill | Turn a plain-language rule into a tested, live Cupcake rule | When a rule is added or changed | The developer, by name |
| mb-config-manager | Agent (draft) with per-system exam skills | Check each system against its record and propose corrections | On demand | The developer, by name |
| mb-doc-check | Python program (commit hook) | Make every committed document and link correct | On every commit made in WSL | Git, automatically |
| mb-project-docs | Skill | Keep a project's config, explainer, README and `AGENTS.md` current | At close-out, or on demand | The developer, or mb-close-out |
| mb-close-out | Skill | End a session with its summary, facts, project documents and commit saved | At the end of a section of work | The developer, by name |
| commit-all | Script | Commit, sync GBrain and push every documentation repository | After any work, with or without close-out | The developer, in the WSL terminal |

---

## 4. How the systems work together

The systems form one cycle around every other system in MyBizz-OS.

1. **A system is documented.** mb-sysdoc-creator researches the system and writes its three documents. The config document becomes the record of how the system is set.
2. **Agents are governed while they work.** Cupcake checks every tool call an agent makes. The rules it enforces are built and changed only through mb-compile-rules.
3. **The system is checked over time.** mb-config-manager compares the live system with its record and proposes corrections for the developer to approve.
4. **Work ends with a close-out.** mb-close-out writes the session page, saves the session's facts, runs mb-project-docs for the project worked in, and lists every uncommitted change.
5. **Everything is committed.** commit-all commits every documentation repository, refreshes GBrain and pushes to GitHub.
6. **Every document is corrected as it enters history.** mb-doc-check runs inside each commit and fixes frontmatter and links, or stops the commit when a new link points nowhere.

Each step leaves the next one something trustworthy to work from. A document written by mb-sysdoc-creator is the record mb-config-manager checks against. A rule built by mb-compile-rules is what Cupcake enforces. A change made in a session is what mb-close-out sweeps up. A commit made by commit-all is what mb-doc-check corrects.

**In one picture:** mb-sysdoc-creator → system record → mb-config-manager ↔ live system; agent → Cupcake → tools; session → mb-close-out (→ mb-project-docs) → commit-all → mb-doc-check → Git and GBrain.

---

## 5. mb-sysdoc-creator — writes the documents for each system

### What it is

mb-sysdoc-creator is a skill that turns "document this system" into three verified documents. The developer names the system and supplies whatever they have: a description, files, links. The skill treats all of it as leads to investigate, never as text to copy. It produces the same three documents every time:

- **`{system}-config.md`:** the settings sheet. What exists and what it is set to.
- **`{system}-reference.md`:** what an agent consults while working with the system. Exact commands, rules and procedures.
- **`{system}-explainer.md`:** what the system is, when to use it, and why it works the way it does.

What may go into each document is fixed by one permanent specification, `doc-suite-specification.md`.

### Intent

**Target**
- **Function.** mb-sysdoc-creator is the only system that researches a system's issuer and writes the system's document suite.
- **The standard.**
  - The issuer's authoritative specification for the installed version is the standard.
  - Its sources are the official documentation, source repository, release notes and configuration schema.
  - For a system we built ourselves, we are the issuer, and the system's own source files are the standard.
- **The suite.** It writes four files in `mybizz-config-docs/{system}/`, and nothing else:
  - `{system}-config.md`:
    - One table per settings area, with the columns **Setting · Issuer standard · Our value · Source · Learning**.
    - Source links to the exact issuer page for that value.
    - Learning names the learnings entry that justifies a difference. It is blank when our value equals the standard.
    - The installed version, and the issuer version the standard was taken from, are stated once at the top.
  - `{system}-reference.md`: everything an agent needs to work with the system, with two required sections:
    - **Authoritative sources:** each source's title, link, issuer version and date checked.
    - **Checks:** the exact command or method to read each setting's live value, and to read the installed version.
  - `{system}-explainer.md`: what the system is, why it exists and how it works. It holds no citations and no values.
  - `{system}-learnings.md`: created empty if missing.
- **Two modes.**
  - **Create:** for a system with no suite. It writes all four files.
  - **Refresh:** for a system whose standard may have changed. It re-researches the authoritative sources and proposes changes row by row. It never rewrites the suite.
  - The developer, or mb-config-manager, starts either mode.
- **Approval.** The developer approves the findings, then the drafted changes, before anything is written.
- **Learnings.** When its research finds a workaround, preference or unique setting, it proposes a learnings entry for approval.
- **Learnings first.** In Refresh mode, it reads the system's learnings before researching. It proposes no change that contradicts an entry without citing the entry and stating why.
- **It never installs.** It records what the issuer has released. It never upgrades or installs anything.
- **Grants.** It requests `net-grant` to reach issuer sites, and `mg-grant` to write in `mybizz-config-docs`, as section 11 sets out.

**Remediation**
- Add issuer research, per-value citations, and the Authoritative sources and Checks sections.
- Change `doc-suite-specification.md` to require the four files, the config columns and the learnings format in section 11.
- Add Refresh mode.
- Remove its run records.
- Move its own learnings from `C:\mybizz\logs\mb-sysdoc-creator\` into its own folder in `mybizz-config-docs`.
- Delete a run's resume state when the run completes.
- Bring every existing suite into the new shape through Refresh, one system at a time.

### How it works

One run moves through fixed phases:

1. **Resume check.** If an unfinished run exists for the same system, the skill offers to resume it.
2. **Learnings.** It reads its own file of lessons from past runs and applies them.
3. **Intake.** It confirms the specification exists, reads every supplied file, opens every supplied link, and confirms where the documents will be written. Without a destination it stops and asks.
4. **Research.** It searches GBrain several times, reads files and runs commands to check every fact it can. It always checks four related areas and reports "none found" when there is nothing: Cupcake rules, memory governor treatment, the security library, and related skills and sources. It tags each fact by where it came from: confirmed live, confirmed through GBrain, unverified from a source, or stated by the developer.
5. **Report and first approval.** It presents its findings, sorted by the document they belong in. Nothing is drafted until the developer approves.
6. **Draft and second approval.** It drafts all three documents, tests each line against the specification, and presents them. Nothing is written to disk until the developer approves again.
7. **Write, log and learn.** It writes the documents, records the run, and adds any lasting lesson to its learnings file.

Its run records and learnings live outside the documentation repositories, under `C:\mybizz\logs\mb-sysdoc-creator\`.

### Why it works this way

- **Research before drafting.** Supplied material is often stale or wrong. A document written by summarising it would make the errors official. Checking live first is the whole value of the skill.
- **Two approvals, not one.** Research and drafts fail in different ways. Findings can be wrong. Drafts can be right but put facts in the wrong document. Two approvals let the developer correct each separately.
- **The four related areas are always checked.** Skipping an area because it "seemed irrelevant" is how silent gaps appeared before. An explicit "none found" turns an omission into a checked claim.
- **One fact, one home.** A fact written in two documents becomes two things that can go out of step. The specification's tests for each document keep every fact in exactly one place.

### What has to happen

- The developer invokes it by name, `/mb-sysdoc-creator (system-name)`, and gives a destination folder.
- The specification `doc-suite-specification.md` must stay in place. Without it the skill stops.
- The developer approves twice in every run.
- Any Cupcake block stops the run until the developer decides.

### Where it stops

- It does not check a system again later. That is mb-config-manager's job.
- It does not correct links or frontmatter. mb-doc-check does that at commit.
- It does not document projects. mb-project-docs does that.
- It does not save facts to governed memory. mb-submit-memory does that.

### Its documents

[[[mybizz-config-docs:mb-sysdoc-creator/sysdoc-creator-explainer]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/sysdoc-creator-explainer.md) · [[[mybizz-config-docs:mb-sysdoc-creator/sysdoc-creator-reference]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/sysdoc-creator-reference.md) · [[[mybizz-config-docs:mb-sysdoc-creator/sysdoc-creator-config]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/sysdoc-creator-config.md) · [[[mybizz-config-docs:mb-sysdoc-creator/doc-suite-specification]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/doc-suite-specification.md). The skill's source is in its own repository, `C:\mybizz\mb-sysdoc-creator\`.

---

## 6. Cupcake and mb-compile-rules — enforce the rules on every agent action

### What it is

**Cupcake** is an open-source policy enforcement layer for AI coding agents, issued by EQTYLab (`github.com/eqtylab/cupcake`). It is built on OPA, the Open Policy Agent, and its policy language, Rego. It is installed in OpenCode as a plugin. It sits between the agent and every tool the agent calls, and it decides, mechanically, whether each call is allowed.

**mb-compile-rules** is the skill that writes Cupcake's rules. A rule starts as a plain-language document. The skill turns it into a compiled, tested rule file in Cupcake's live folder.

The distinction that matters: a rule written in `AGENTS.md` is a request the agent may not follow. A rule enforced by Cupcake is checked every time, whatever the agent believes or has agreed to.

### Intent

**Target**
- **Functions.**
  - Cupcake enforces the rules on every agent tool call.
  - mb-compile-rules is the only route by which a rule becomes live.
  - Each keeps its own function. Neither checks configuration or writes documentation.
- **Issuers.**
  - Cupcake's issuer is EQTYLab. Its standard is the Cupcake repository and documentation at the installed version.
  - mb-compile-rules's issuer is us. Its standard is its own `SKILL.md`.
- **Rules belong to Cupcake.** The plain-language rules in `rules-cupcake-global` and the compiled rule files are Cupcake's own configuration. They are recorded in Cupcake's config, not in mb-compile-rules's.
- **Testing.** The developer runs every real test. A rule stays live only if its real tests pass.
- **Grants.** Cupcake enforces the grants that the integrity systems request, as section 11 sets out. It never issues one.

**Remediation**
- Remove `exam-plan`, `exam-report`, `log` and the exam `skill` from the `cupcake` folder.
- Remove the `skill` stub from the `mb-compile-rules` folder.
- Bring both suites into the new shape through mb-sysdoc-creator's Refresh.

### How it works

**Every tool call is checked.** Before a tool call runs, the plugin turns it into a structured event and asks the Cupcake checker for a decision. If any rule denies it, the call is blocked with the rule's own message. If nothing denies it, the call proceeds.

**Two kinds of rule.**

- **Builtins** ship with Cupcake: system protection, sensitive-data protection, and protection of the Cupcake binary from agent execution.
- **Custom rules** are written for this environment, one per concern. They cover test tampering, lockfiles, unverified packages, destructive database operations, history rewriting, autonomous commits, writes outside the current project, secret files, outbound network access, new tools or skills, the agent's own governing files, and Cupcake's own control plane.

**Rules are global.** Every custom rule lives in `~/.config/cupcake/policies/opencode/` and applies to every project. A project may also carry its own `.cupcake` folder with local rules. A folder without one falls back to the global rules only.

**Grants open a door for a short time.** Some rules can be relaxed by a grant the developer issues in the WSL terminal:

- `mg-grant` opens a folder outside the project for a set number of writes.
- `net-grant` opens a website for a set number of fetches.
- `git-grant` allows a set number of commits, pushes or merges in one repository.

A grant expires after a fixed time (30 minutes by default) or when its count runs out. The agent can never issue a grant itself.

**Failure does not stop work.** If Cupcake itself fails, it is set to "fail open": the call is allowed and the failure is logged, rather than every tool call being blocked.

**How a rule is built (mb-compile-rules).**

1. The developer writes the rule in plain language in `C:\dev\project-library-global\rules-cupcake-global\`.
2. The skill reads it in full and drafts the rule file in one complete write.
3. It checks the file's syntax with `opa check`.
4. It places the file in the live folder and runs an immediate health check.
5. The developer runs the real block and allow tests in the WSL terminal.
6. The rule is kept only if the real tests pass. Otherwise it is removed at once.

### Why it works this way

- **A machine, not a request.** The agent cannot reason around a rule it cannot reach.
- **The agent cannot run Cupcake.** Cupcake blocks its own binary from the agent, so the agent can never weaken or test its own guard. Every real test is run by the developer.
- **Placement is the start of testing.** Cupcake only ever evaluates what is actually installed, so a rule cannot be tested on its own first. The health check right after placement catches a bad file in seconds.
- **One complete write per rule.** Partial edits that land out of order can leave a rule broken and stop all evaluation.
- **Only real tests count.** Some rules pass offline checks and still fail live. Only the developer's real output decides.
- **Time is handled outside the rules.** Some Rego functions fail silently inside Cupcake, including time functions. So grant expiry and counting are done by the plugin, and the rules only check whether a grant exists.
- **Fail open.** A guard that blocks all work when it breaks gets switched off. Failing open keeps it trusted and in place.

### What has to happen

- Every new or changed rule goes through mb-compile-rules. No rule file reaches the live folder any other way.
- The developer runs every real test.
- The developer issues every grant, with the count the work needs.
- The plugin exists at exactly one path. A second copy, even a link to the same file, makes every check run twice.
- Plugin or global configuration changes take effect only after OpenCode is fully restarted.
- Every project `.cupcake` folder keeps at least one real rule file, or the engine fails to load.

### Where it stops

- It cannot see conversational approval. A rule that protects a file blocks every write, including one the developer just approved in chat.
- It cannot see inside a script file. It checks the command text only.
- It does not decide what should be forbidden. The developer decides, in the plain-language rule.
- It is not a security review of the code being built. That is mb-sentinel-review's job.

### Its documents

[[[mybizz-config-docs:cupcake/cupcake-explainer]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-explainer.md) · [[[mybizz-config-docs:cupcake/cupcake-reference]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-reference.md) · [[[mybizz-config-docs:cupcake/cupcake-config]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-config.md) · [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-explainer]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-explainer.md) · [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-reference]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-reference.md) · [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-config]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-config.md). The skill's source is in its own repository, `C:\mybizz\mb-compile-rules\`. The plain-language rules are in `C:\dev\project-library-global\rules-cupcake-global\`.

---

## 7. mb-config-manager — checks each system against its record

### What it is

mb-config-manager is the agent responsible for keeping every system correctly configured. It compares a system's actual state with the system's config document, finds what has silently stopped working, and proposes corrections. Nothing changes without the developer's approval, item by item.

It is designed as an agent, not a skill. Its instructions exist as a draft (`config-manager-agent.md`) and are not yet installed. Runs are started by the developer with the draft as the instruction.

### Intent

**Target**
- **Function.** mb-config-manager keeps every skill and system configured to its standard. It examines, proposes and, after approval, corrects. It does not research issuers or write suites. It has mb-sysdoc-creator do both.
- **Installed as an agent.**
  - It is an OpenCode agent, invoked through the `/mb-config-manager` command.
  - It never runs by itself.
- **Invocation.**
  - `/mb-config-manager (system-name)` examines one system.
  - `/mb-config-manager` with no name reads the register. It lists each system whose Last updated is later than its Last checked, or that has never been checked. Then it asks which to examine.
- **It holds nothing about its subjects.**
  - It holds one examination procedure, the same for every system.
  - It reads every value, source and check fresh from the subject's folder on each run.
- **One run, for one system:**
  1. **Locate.** Read the system's register row and folder. Read the learnings file in full before anything else.
     - If the system has no register row, add one.
     - If the system has no suite, run mb-sysdoc-creator in Create mode, then continue.
     - Any missing file in the folder is a GAP.
  2. **Issuer.**
     - Read the installed version, using the reference's Checks.
     - Check the Authoritative sources for a newer issuer release or a changed specification.
     - If the installed version differs from the recorded one, run mb-sysdoc-creator in Refresh mode first.
  3. **Compare** each config row three ways: issuer standard, our value and live value. Weigh every finding against the learnings. A difference that a learnings entry justifies is not a finding.
     - Read the live value with the reference's Checks.
     - Then show that the system actually works, using the reference's Checks.
  4. **Report** the findings in chat only. Each finding gives:
     - its kind;
     - what differs;
     - the proposed correction;
     - the justification, citing the source or the learnings entry.
  5. **Decide.** The developer approves or rejects each item.
  6. **Apply** each approved change itself, one at a time, checking each before the next.
     - Where Cupcake reserves an action for the developer, it gives the exact terminal lines instead.
     - A correction that needs a Cupcake rule goes through mb-compile-rules.
  7. **Record** only the approved results:
     - changed config rows;
     - new, changed or removed learnings entries;
     - the system's register row, with today's Last checked date.
- **Finding kinds.**
  - **DOWN:** the system is unavailable.
  - **UNHEALTHY:** it runs but behaves wrongly.
  - **DRIFT:** the live value differs from our recorded value.
  - **DEVIATION:** our value differs from the standard, and no learnings entry justifies it.
  - **BEHIND:** the issuer has released a newer version than the one installed.
  - **GAP:** something required is missing or undocumented.
- **Upgrades are never automatic.**
  - An upgrade can break the systems downstream of it. OpenCode and GBrain both break other systems when upgraded without exact care.
  - So a BEHIND finding is a notice and a request, never an action. It tells the developer:
    - what the new version is;
    - what the issuer's release notes say changes or breaks;
    - which learnings entries it affects;
    - which other systems are known to depend on it.
  - The developer decides: upgrade now, or stay.
  - **If the developer chooses to upgrade:** it gives the exact upgrade steps, including any the learnings require. The developer runs them. Then it runs Refresh and examines the system again.
  - **If the developer chooses to stay:** it proposes a learnings entry stating the installed version kept and why.
  - **An upgrade made outside a run:** when the installed version has changed since the last check, it reports this before anything else, then runs Refresh.
- **The register** (`config-manager-register.md`):
  - It is the only ongoing record in System Integrity.
  - It has one row per skill and system, the integrity systems and mb-config-manager included.
  - Its columns are **System · Kind · Docs folder · Installed version · Last updated · Last checked · State**.
  - **Kind** is skill, agent, script or plugin.
  - **Last updated** is the date the system itself last changed. For a system with an issuer, that is its installed version's date. For a system we built, it is the last commit to its source.
  - **State** is Active, Deferred or Retired.
  - Rows are overwritten, never appended. The register holds no settings and no notes.
- **Grants.** It requests `net-grant` to reach issuer sources, `mg-grant` to write in `mybizz-config-docs`, and any grant a correction needs, as section 11 sets out.

**Remediation**
- Install the agent, plus the `/mb-config-manager` command that routes to it.
- Rewrite its examination procedure to the run above.
- Retire the per-system exam skills (`mb-config-{system}`).
  - Their checks move into each subject's reference, under Checks.
  - Their expected values move into each subject's config.
- Remove exam plans, exam reports, logs, the body-hash fingerprints and the self-check that uses them.
- Reduce the register to the columns above.
- Retire `config-setup-prompt` once the agent is installed.

### How it works

**The agent and its exam skills.** The agent does the coordinating. For each managed system there is a separate exam skill, `mb-config-{system}`, installed in `~/.config/opencode/skills/`. The exam skill holds that system's checks.

**The register.** One document, `config-manager-register.md`, lists every managed system, its exam skill, its config document, and its state (for example Installed, Deferred or Retired).

**One run.**

1. **Self-check.** The agent checks its own consistency: each installed exam skill matches its source, each system folder holds its files, and each config document exists.
2. **Plan.** It writes an exam plan for the system from the config document, the last report, the log and the learnings.
3. **Exam.** It runs the exam skill in four tiers:
   - Tier 0: is the system there and can it be invoked?
   - Tier 1: does each setting match the config document?
   - Tier 2: does the system actually work, not just exist?
   - Tier 3: deeper checks, run only if the developer allows.
4. **Classify.** Every finding is one of four kinds: DOWN (unavailable), UNHEALTHY (runs but behaves wrongly), DRIFT (live state differs from the record) or GAP (something missing or undocumented).
5. **Approval.** The developer decides each finding: approve, reject or defer.
6. **Apply.** Approved repairs are applied one at a time, each checked before the next.
7. **Record.** The run writes an exam report, adds a row to the system's log, and files any lessons.

Where Cupcake blocks a check, the check becomes a command the developer runs in the WSL terminal.

### Why it works this way

- **Present is not the same as working.** A file can exist while the system it configures does nothing. So the exam proves function, not just presence.
- **Detection runs without questions; repair waits for approval.** The exam must finish to be useful, and no change to a working system is safe without the developer's decision.
- **One repair at a time, with a way back.** A repair that fails can be undone before it compounds.
- **The agent is bound by Cupcake like any other agent.** It requests grants and honours every denial.

### What has to happen

- The developer starts each run, for one system or for all.
- The developer decides every finding.
- The developer runs every check that Cupcake reserves for the terminal.
- The developer commits the results with commit-all.

### Where it stops

- It does not write a system's documents. mb-sysdoc-creator does that.
- It does not correct links or frontmatter. mb-doc-check does that.
- It does not change Cupcake's rules. mb-compile-rules does that.

### Its documents

[[[mybizz-config-docs:mb-config-manager/config-manager-explainer]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-explainer.md) · [[[mybizz-config-docs:mb-config-manager/config-manager-reference]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-reference.md) · [[[mybizz-config-docs:mb-config-manager/config-manager-config]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-config.md) · [[[mybizz-config-docs:mb-config-manager/config-manager-agent]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-agent.md) · [[[mybizz-config-docs:mb-config-manager/config-manager-register]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-register.md) · [[[mybizz-config-docs:mb-config-manager/config-setup-prompt]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-setup-prompt.md).

---

## 8. mb-doc-check — keeps every document and link correct at commit

### What it is

mb-doc-check is a small Python program, not an AI. It runs automatically as a Git hook every time documents are committed in WSL. It makes sure every document entering the history of a documentation repository is correct: it has complete frontmatter, every link is in one standard form, and every link points to a document that exists.

### Intent

**Target**
- **Function.** Its function is unchanged: it checks form and links at commit. It never checks configuration or meaning.
- **Issuers.**
  - Its issuer is us. Its standard is `mb-doc-check.py` and `mb-doc-check.sources`.
  - Its link form depends on GBrain's naming rules, so GBrain is the issuer for those rules.

**Remediation**
- Add `doc-check-learnings.md` to its folder.
- Bring its suite into the new shape through mb-sysdoc-creator's Refresh.

### How it works

At each commit it compares the committed state of the repository with the state about to be committed. From that it knows what is new, what moved and what was deleted. Then, for every document in the commit, it:

1. completes the frontmatter: a `title`, a permanent `doc-id`, and an `updated` date that moves only when the document really changed;
2. reads every link, in any accepted form, and decides which document it means;
3. rewrites each link in the standard form, which both GBrain and MarkText can follow;
4. repairs links to documents that were renamed or moved, in every covered repository;
5. turns links to deleted documents into plain words;
6. stores the file with one kind of line ending.

It stops the commit only when a link typed in that commit points to nothing or to a name shared by several documents. Problems that already existed are reported but never block.

A document keeps its identity through its `doc-id`, not its name. A document can be renamed, moved or rewritten and every link to it still follows it.

A settings file, `mb-doc-check.sources`, says which folder is which GBrain source.

### Why it works this way

- **At commit, not on a schedule.** A document is correct at the moment it enters history, so errors never pile up.
- **Text only.** It never moves, renames or deletes a file. Where documents live is the developer's decision.
- **It blocks only new mistakes.** One old broken link must not stop all work. A broken link just typed costs one line to fix now.
- **It fails open.** If the program itself breaks, it says so and lets the commit through. A helper's bug must never stop work.
- **Same result every time.** The documents are written by AI agents, so the checker must not add variation of its own.

### What has to happen

- Commits are made in WSL, with commit-all, never with `--no-verify` and never from a Windows program.
- After any change to GBrain's sources, `sources-check` is run.
- After any GBrain upgrade, `slug-selftest` is run.
- When a commit is stopped, the named line is fixed and the commit is run again.
- A new documentation repository is brought under the hook before it is used.

### Where it stops

- It checks form and links, never meaning. It cannot tell whether a document is true.
- It does not cover code repositories, the Anvil clone or the memory governor's folder.
- It never edits an installed `SKILL.md` or anything in an `obsolete` folder.

### Its documents

[[[mybizz-config-docs:mb-doc-check/doc-check-explainer]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-explainer.md) · [[[mybizz-config-docs:mb-doc-check/doc-check-reference]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-reference.md) · [[[mybizz-config-docs:mb-doc-check/doc-check-config]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-config.md) · [[[mybizz-os-docs:os-reference-docs/documentation-linking-explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/documentation-linking-explainer.md). The program is `C:\mybizz\scripts\mb-doc-check.py`.

---

## 9. mb-project-docs — keeps each project's documents current

### What it is

mb-project-docs is a skill that keeps each project's own documents current. A project is one of `mb-3-cs`, `mb-4-ecom`, `mb-5-pdlf` and `pdlf`. In each, it maintains:

- a **config** file: the project's state;
- an **explainer**: what the project is and why it is built the way it is;
- the project's **`AGENTS.md`**: how agents work in it, which doubles as the project's reference;
- a **README** that points to all three as required reading.

It works on one project at a time, the one the session is in.

### Intent

**Target**
- **Function.** Its function is unchanged. It documents projects, never systems.
- **Where project documents live.** They live in their own project repositories, not in `mybizz-config-docs`.
- **Projects are not subjects.** Projects are not subjects of mb-config-manager. mb-project-docs itself is a subject.
- **Issuer.** Its issuer is us. Its standard is its own `SKILL.md`.

**Remediation**
- Add `project-docs-learnings.md` to its folder.
- Bring its suite into the new shape through mb-sysdoc-creator's Refresh.

### How it works

1. It reads the project's real state and what the session changed.
2. It compares that with what the files say and lists what is no longer true.
3. It creates any file that is missing.
4. It proposes small edits: corrections, plus what the session added, each to the file that owns it.
5. It prepares the `AGENTS.md` change as a draft. The developer applies it with one terminal line.

For a code repository it writes into `theme\docs`, and only if the Anvil app already exists. Otherwise it stops and asks the developer to create the app.

### Why it works this way

- **Three files, not one.** State, rules and reasons change at different speeds, so an update touches only the file that is wrong.
- **The reference is the `AGENTS.md`.** An agent's rules and a project's working reference are the same content. One file prevents two copies drifting apart.
- **The agent does not write `AGENTS.md`.** The file governs the agent. The developer applies its changes.
- **Small edits, never regeneration.** State can be found again. Reasons and decisions cannot, and a rewrite would lose them.
- **It runs at close-out.** That is when the session's changes are known and the developer is already approving.

### What has to happen

- It runs inside mb-close-out, or when the developer invokes `/mb-project-docs`.
- The developer approves its changes at close-out's single approval step.
- The developer applies the `AGENTS.md` change with the terminal line it provides.

### Where it stops

- It does not document systems. mb-sysdoc-creator does that.
- It does not correct links. mb-doc-check does that, and only in documentation repositories.
- It does not work across several projects at once.

### Its documents

[[[mybizz-config-docs:mb-project-docs/project-docs-explainer]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-explainer.md) · [[[mybizz-config-docs:mb-project-docs/project-docs-reference]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-reference.md) · [[[mybizz-config-docs:mb-project-docs/project-docs-config]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-config.md). The skill's source is in its own repository, `C:\mybizz\mb-project-docs\`.

---

## 10. mb-close-out and commit-all — save a session's work

### What it is

**mb-close-out** is a skill that ends a section of work without losing anything that matters: what was decided, what changed on disk, and which facts are worth trusting later. It does no new work of its own. It runs the existing pieces in order and gathers every decision into one approval.

**commit-all** is a script, `C:\mybizz\scripts\commit-all.sh`. It commits every documentation repository, refreshes GBrain and pushes everything to GitHub with one command. It is close-out's last step, and it can also be run on its own whenever work needs committing without a close-out.

### Intent

**Target**
- **Functions.** Both functions are unchanged, and the two stay separate.
  - mb-close-out ends a section of work.
  - commit-all commits, syncs and pushes, alone or as close-out's last step.
- **commit-all is a system in its own right**, with its own folder: `mybizz-config-docs/commit-all/`.
- **Session pages stay.** They are working memory for GBrain recall, not history.
- **commit-all's log holds the last run only.** It is used only to diagnose that run.
- **Issuers.** The issuer of both is us. Their standards are mb-close-out's `SKILL.md` and `commit-all.sh`.

**Remediation**
- Add `mb-close-out-learnings.md` to its folder.
- Bring mb-close-out's suite into the new shape through mb-sysdoc-creator's Refresh.
- Create the commit-all folder and its four files through mb-sysdoc-creator's Create.
- Move commit-all's content out of mb-close-out's documents into its own suite. The operating procedure links to that suite.
- Make commit-all overwrite its log on each run.

### How it works

**mb-close-out, step by step:**

1. **Session page.** It writes a summary of the session as a file under `sessions/`. commit-all brings it into GBrain.
2. **Facts.** It lists the session's lasting rules, decisions and pitfalls, one claim each, and sorts them:
   - **advisory** facts, which only need to be found again, are saved straight to GBrain;
   - **governed** facts, which must be trusted later without re-checking, are written as fact files and sent through `mb-submit-memory` and the memory governor's checks.
3. **Project documents.** If the session worked in a project, it runs mb-project-docs.
4. **Changed files.** It lists every uncommitted change in every repository and flags anything that was not this session's work.
5. **One approval.** It presents everything at once. The developer may cancel, change or re-route any item.
6. **Save.** After approval it saves the facts and applies the project documents.
7. **Terminal lines.** It gives the developer the exact lines to paste: any `AGENTS.md` copy, any grant, and the commit-all command.
8. **Promotions.** If a governed fact is waiting for review, it gives the developer the line to promote it.
9. **Check.** It confirms every repository committed and pushed, every governed fact landed, and the memory governor's review queue is empty.

**commit-all, in order:**

1. It commits every repository with changes, using one message. mb-doc-check runs inside each commit.
2. It refreshes GBrain with `sync-gbrain.sh`.
3. It pushes every repository to GitHub.
4. It prints a summary, ending in `All done.` when nothing failed, and saves a full log.

With `--status` it only shows what would be committed and changes nothing.

### Why it works this way

- **One orchestrator over existing pieces.** Every piece already had its own procedure. Close-out sequences them rather than duplicating them.
- **One approval, not three.** mb-submit-memory and mb-project-docs each need approval. Close-out's single approval satisfies both.
- **Terminal lines, not agent authority.** The agent holds no Git authority, no credentials and no power to grant access. Every such action is a line the developer pastes.
- **Lines contain only known values.** A command built from remembered output is where a wrong value enters a privileged action.
- **An empty review queue ends the close-out.** An open review item blocks work everywhere, so close-out does not end with one pending.
- **Close-out and commit-all are separate.** Work often needs committing before a section is finished. commit-all serves that alone; close-out uses it as its final step.

### What has to happen

- The developer invokes `/mb-close-out` when a section of work is finished. It never starts by itself.
- The developer approves once, then pastes the terminal lines.
- commit-all is run in the WSL terminal, never from Windows.
- If commit-all reports a BLOCKED repository, the developer fixes the named problem and runs it again.

### Where it stops

- The agent never commits. The developer runs commit-all.
- Close-out does not keep notes during a session. It acts at the end.
- commit-all covers the documentation repositories and the scripts repository only. Code repositories are committed separately.

### Its documents

[[[mybizz-config-docs:mb-close-out/mb-close-out-explainer]]](C:/mybizz/mybizz-config-docs/mb-close-out/mb-close-out-explainer.md) · [[[mybizz-config-docs:mb-close-out/mb-close-out-reference]]](C:/mybizz/mybizz-config-docs/mb-close-out/mb-close-out-reference.md) · [[[mybizz-config-docs:mb-close-out/mb-close-out-config]]](C:/mybizz/mybizz-config-docs/mb-close-out/mb-close-out-config.md) · [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md). The skill's source is in its own repository, `C:\mybizz\mb-close-out\`.

---

## 11. Rules and intents every System Integrity system shares

### Shared intent

- **One function each.** Every integrity system has one function that no other integrity system performs:
  - mb-sysdoc-creator researches and writes suites;
  - mb-config-manager examines and corrects;
  - Cupcake enforces;
  - mb-compile-rules makes rules live;
  - mb-doc-check corrects form and links at commit;
  - mb-project-docs documents projects;
  - mb-close-out ends sessions;
  - commit-all commits.
- **Integrity systems hold nothing about their subjects.**
  - Their own config, reference and explainer describe themselves only.
  - The one exception is mb-config-manager's register. It is an index of names, versions, dates and states, and holds no settings.
- **Every subject has its own folder.**
  - Each skill or system being kept has a folder in `mybizz-config-docs/`.
  - The folder holds exactly four files: config, reference, explainer and learnings.
  - The integrity systems are subjects too, and keep the same four files in their own folders.
- **The issuer's specification is the standard.**
  - mb-sysdoc-creator sets it.
  - mb-config-manager keeps it current, through mb-sysdoc-creator's Refresh mode.
- **Learnings hold only what is true now.**
  - Each learnings file records updates, preferences, workarounds and unique settings found to be relevant.
  - Each entry has an ID (`L1`, `L2` and so on), what it is, why it holds, and the source or finding it came from.
  - A config row that differs from the standard points to its entry.
  - An entry that stops being true is removed, and any row pointing to it is corrected in the same change.
- **Learnings are read first.**
  - Learnings hold the nuances and unique settings that make our setup work.
  - Every integrity system reads a subject's learnings in full before it researches, examines or proposes anything for that subject.
  - No proposal contradicts a learnings entry without citing it and stating why.
- **No upgrade or update is installed autonomously.**
  - Any integrity system that finds a newer version notifies the developer and asks.
  - The developer decides whether and when to upgrade, and runs the upgrade.
- **Grants are requested, in the standard form.**
  - When an integrity system needs to write outside its project, reach a website or act on a repository, it stops. It gives the developer the exact grant line to paste in the WSL terminal, with the narrowest folder, domain or repository and the smallest count the work needs.
  - The line formats are fixed in [[[mybizz-os-docs:operating-procedures/grant-paths]]](C:/mybizz/mybizz-os-docs/operating-procedures/grant-paths.md):
    - `mg-grant --path <folder> --count N` to write in a folder;
    - `net-grant --domain <domain> --count N` to reach a website;
    - `git-grant`, in the form given in Cupcake's reference, to act on a repository.
  - When the work finishes before the grant expires, it gives the matching revoke line: `mg-revoke --path <folder>` or `net-revoke --domain <domain>`.
- **Every change is approved.** No integrity system writes a subject's suite, learnings or register row without the developer's approval of that item.
- **No record is kept for history or ceremony.**
  - The only ongoing record is mb-config-manager's register.
  - Findings are given in chat.
  - Git keeps history.

### Shared rules

- **The developer decides.** No system applies a change to a working system, a governing file or a record without the developer's approval.
- **The agent never commits.** Every commit, push and grant is a line the developer runs.
- **Denials are final.** When Cupcake blocks an action, the system stops and asks. It never retries in another form or works around the block.
- **Grants are requested, never issued by the agent.** The agent states the folder, website or repository and the count it needs.
- **Credentials are named, never shown.** A credential appears only as its variable name.
- **One fact, one home.** Every fact lives in one document. A fact written twice drifts.
- **Present is not working.** A system is trusted only when it is shown to work, not when its files exist.
- **Fail open where failure would stop work.** Cupcake and mb-doc-check both let work continue when they themselves break, and say so.
- **Plain records, current state.** Documents state what is true now and why. They do not keep history for its own sake; Git keeps history.

