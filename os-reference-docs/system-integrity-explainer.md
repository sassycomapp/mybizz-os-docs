---
title: "system-integrity-explainer"
doc-id: "mb-91e45ce5"
updated: "2026-10-15"
---
# System Integrity — Explainer

*What System Integrity is, why it exists, which systems make it up, how each one works, why it works that way, and what has to happen for it to keep working. Each system has its own section and its own document suite, which holds the depth. This document holds the whole picture and points to them.*

---

## How to read this document

Section 1 says what System Integrity is. Section 2 says why it exists. Section 3 lists the systems. Section 4 shows how they work together. Sections 5 to 10 describe each system on its own, in the same order every time:

- **What it is**
- **Intent:** what must still change (**Remediation**), present only while something is open
- **How it works**
- **Why it works this way**
- **What has to happen**
- **Where it stops** (what it does not do, and which system does that instead)
- **Its documents**

Section 11 states the rules and intents that every one of these systems shares, and the remediations that apply to all of them and to this document.

Each section describes the system as it is built and runs. Where something must still change, the system's Intent lists it as a Remediation. A remediation is removed when it is done. A system with nothing open has no Intent part.

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
| mb-sysdoc-creator | Skill | Research a system against its issuer and write or refresh its four documents | When a system needs documenting or re-documenting | The developer, or mb-config-manager, by name |
| Cupcake | Installed product (plugin + policy engine) | Allow or block every agent tool call against compiled rules | On every agent tool call | Always on |
| mb-compile-rules | Skill | Turn a plain-language rule into a tested, live Cupcake rule | When a rule is added or changed | The developer, by name |
| mb-config-manager | Agent | Examine each system against its standard and apply approved corrections | On demand | The developer, by name |
| mb-doc-check | Python program (commit hook) | Make every committed document and link correct | On every commit made in WSL | Git, automatically |
| mb-project-docs | Skill | Keep a project's config, explainer, README and `AGENTS.md` current | At close-out, or on demand | The developer, or mb-close-out |
| mb-close-out | Skill | End a session with its summary, facts, project documents and commit saved | At the end of a section of work | The developer, by name |
| commit-all | Script | Commit, sync GBrain and push every documentation repository | After any work, with or without close-out | The developer, in the WSL terminal |

---

## 4. How the systems work together

The systems form one cycle around every other system in MyBizz-OS.

1. **A system is documented.** mb-sysdoc-creator researches the system against its issuer and writes its four documents. The config document becomes the record of how the system is set, measured against the issuer's standard.
2. **Agents are governed while they work.** Cupcake checks every tool call an agent makes. The rules it enforces are built and changed only through mb-compile-rules.
3. **The system is examined over time.** mb-config-manager compares the issuer standard, the record and the live system, and applies the corrections the developer approves.
4. **Work ends with a close-out.** mb-close-out writes the session page, saves the session's facts, runs mb-project-docs for the project worked in, and lists every uncommitted change.
5. **Everything is committed.** commit-all commits every documentation repository, refreshes GBrain and pushes to GitHub.
6. **Every document is corrected as it enters history.** mb-doc-check runs inside each commit and fixes frontmatter and links, or stops the commit when a new link points nowhere.

Each step leaves the next one something trustworthy to work from. A document written by mb-sysdoc-creator is the record mb-config-manager checks against. A rule built by mb-compile-rules is what Cupcake enforces. A change made in a session is what mb-close-out sweeps up. A commit made by commit-all is what mb-doc-check corrects.

**In one picture:** mb-sysdoc-creator → system record → mb-config-manager ↔ live system; agent → Cupcake → tools; session → mb-close-out (→ mb-project-docs) → commit-all → mb-doc-check → Git and GBrain.

---

## 5. mb-sysdoc-creator — writes the documents for each system

### What it is

mb-sysdoc-creator is the skill that researches a skill or system against its issuer's authoritative specification, and writes or refreshes the system's four documents. It is the only system that researches issuers and writes these documents.

- **`-config.md`:** the settings sheet. What exists, what it is set to, and the issuer's standard for each setting, with a source link per value.
- **`-reference.md`:** what an agent consults while working with the system, including its Authoritative sources and its Checks.
- **`-explainer.md`:** what the system is, what it can do, whether to use it, why it works the way it does, and what has to happen.
- **`-learnings.md`:** what differs from, or adds to, the standard in the MyBizz setup, and why.

The four documents live in `C:\mybizz\mybizz-config-docs\{system}\` and are found by their endings. What may go into each is fixed by one specification, `doc-suite-specification.md`, a working file in the skill's own folder.

Its issuer is MyBizz. Its source is `C:\mybizz\mb-sysdoc-creator\mb-sysdoc-creator.md`, in its own repository, and its installed version is that repository's latest commit. The installed copy, `~/.config/opencode/skills/mb-sysdoc-creator/SKILL.md`, carries the same body below its own frontmatter.

### How it works

**The standard.** The issuer's authoritative specification for the installed version is the standard: official documentation, source repository, release notes and configuration schema. For a system MyBizz built, MyBizz is the issuer, and the system's own source files are the standard. Every standard value is recorded with a link to its exact source.

**Mode, decided from the folder:**

- **Create:** the folder does not exist, or holds none of the four documents. It writes all four.
- **Refresh:** the folder holds at least one of the four. It refreshes each existing document row by row and creates any missing one. It rewrites a document whole only when the document must be restructured to the specification, every fact in it has been marked carried, corrected or retired, and the developer has approved the full draft.

**One run:**

1. **Locate.** It confirms the specification exists, reads its own learnings, finds the system's folder, sets the mode, and in Refresh reads every existing document, the learnings first.
2. **Intake.** It reads everything the developer supplied, as leads to verify, never as text to copy.
3. **Research.**
   - It names the issuer and reads the installed version.
   - It finds the authoritative sources for that version, and notes any newer issuer release.
   - For every setting it records the issuer standard with its source, verifies the MyBizz value live, and checks any difference against the learnings.
   - It writes the method to check each setting, the installed version, and whether the system works.
   - It runs the cross-system check: Cupcake rules, memory governor treatment, the security library, and related skills and sources, reporting "none found" for any with no result.
   - It tags every fact: confirmed live, confirmed through GBrain, unverified from a source, or stated by the developer.
4. **Report, first approval.** Create presents findings by document. Refresh presents a numbered change list. Any newer issuer release and proposed learnings entries are included.
5. **Draft.** Create drafts all four documents. Refresh drafts only the approved changes, any approved restructure, and any missing document. Every line passes its document's placement test.
6. **Present, second approval.**
7. **Apply.** It writes inside the system's folder only. A new document takes the same name start as the documents already in the folder. In a new or empty folder it takes the folder name. No document is renamed.
8. **Close.** It proposes any lesson for its own learnings, reports what was written, gives the revoke line for any open grant, and reminds the developer to commit.

It keeps no record of its runs. Findings are given in chat.

### Why it works this way

- **The issuer is the measure.** A record checked only against itself drifts unnoticed. A cited issuer value gives every setting something to be measured against.
- **Research before drafting.** Supplied material is often stale or wrong. Checking live first is the whole value of the skill.
- **Learnings first.** The learnings hold what makes the MyBizz setup work. Research that ignores them proposes changes that break it.
- **Row by row, unless the shape is wrong.** A rewrite loses reasoning and corrections that cannot be found again, so a document in the right shape is changed only row by row. A document in the wrong shape cannot be patched into the right one, so it is rewritten whole, with every fact accounted for and the full draft approved.
- **Two approvals.** Findings and drafts fail in different ways. Two approvals let the developer correct each separately.
- **The cross-system check runs every time.** An explicit "none found" turns an omission into a checked claim.
- **One fact, one home.** A fact written twice drifts. The placement tests keep every fact in one document.
- **It never installs.** An upgrade can break systems downstream. The developer decides every upgrade.

### What has to happen

- The developer, or mb-config-manager, invokes it with `/mb-sysdoc-creator (system-name)`.
- `doc-suite-specification.md` stays in place. Without it the skill stops.
- The developer approves twice in every run, and issues any grant it requests.
- After any change to the source, the developer copies its body into the installed copy, and the two bodies are checked identical.
- The developer commits with commit-all.

### Where it stops

- It does not examine a system or apply corrections to it. mb-config-manager does that.
- It does not correct links or frontmatter. mb-doc-check does that at commit.
- It does not document projects. mb-project-docs does that.
- It does not save facts to governed memory. mb-submit-memory does that.

### Its documents

[[[mybizz-config-docs:mb-sysdoc-creator/sysdoc-creator-explainer]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/sysdoc-creator-explainer.md) · [[[mybizz-config-docs:mb-sysdoc-creator/sysdoc-creator-reference]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/sysdoc-creator-reference.md) · [[[mybizz-config-docs:mb-sysdoc-creator/sysdoc-creator-config]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/sysdoc-creator-config.md) · [[[mybizz-config-docs:mb-sysdoc-creator/sysdoc-creator-learnings]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/sysdoc-creator-learnings.md) · [[[mybizz-config-docs:mb-sysdoc-creator/doc-suite-specification]]](C:/mybizz/mybizz-config-docs/mb-sysdoc-creator/doc-suite-specification.md). The skill's source is in its own repository, `C:\mybizz\mb-sysdoc-creator\`.

---

## 6. Cupcake and mb-compile-rules — enforce the rules on every agent action

### What it is

**Cupcake** is an open-source policy enforcement layer for AI coding agents, issued by EQTYLab (`github.com/eqtylab/cupcake`). It is built on OPA, the Open Policy Agent, and its policy language, Rego. It is installed in OpenCode as a plugin. It sits between the agent and every tool the agent calls, and it decides, mechanically, whether each call is allowed. EQTYLab issues the engine. MyBizz issues the plugin amendments, the custom rules and the grant tools.

**mb-compile-rules** is the skill that writes Cupcake's rules. A rule starts as a plain-language document. The skill turns it into a compiled, tested rule file in Cupcake's live folder. Its issuer is MyBizz, and its standard is its source, `C:\mybizz\mb-compile-rules\mb-compile-rules.md`.

The distinction that matters: a rule written in `AGENTS.md` is a request the agent may not follow. A rule enforced by Cupcake is checked every time, whatever the agent believes or has agreed to.

### Intent

**Remediation**
- **Unverified and out-of-date row (DRIFT).** cupcake-config's "Skeleton initialization" row names the folders `mb-3-cs`, `mb4ecom`, `mb4ecom-project-library`, `mb5pdlf` and `mb5pdlf-project-library`. Those folders no longer exist under those names, and the row marks every folder except `mybizz-config-docs` as carried from record, unverified. Fix: the developer lists the folders that hold a `.cupcake` skeleton in the WSL terminal, and the row is corrected from that output through `/mb-config-manager cupcake`.
- **Open item points to a missing file.** cupcake-config open item 2 places the exact commands of the skills-dir ancestor check in an exam report under `C:\mybizz\logs\`, which does not exist. Fix: write the two-step check's exact commands into the reference's Checks table, run by the developer, and remove the pointer from the open item.
- **Learning states an unproven cause.** Learning L4 states as fact the cause that open item 2 records as unverified. Fix: remove L4. It is written again only when the developer's check proves it.
- **Learning holds an out-of-date condition.** Learning L5 ties its revisit to the move to OpenCode V2. OpenCode runs version 1, because the Cupcake plugin does not run on V2. Fix: remove the V2 clause and keep the decision to stay on 0.3.0.
- **Grant-decrement anomaly, unproven.** cupcake-config open item 3 records a grant unit consumed without a successful write. The mb-doc-check examination produced a second occurrence on one path. Fix: the developer designs and runs one live test that separates the two recorded hypotheses. The result becomes a config row and, where it differs from the standard, a learning, and the open item closes.
- **Security library contradicts the setup.** `sec-fail-closed.md` in `project-library-global` records no known exceptions. Cupcake's fail-open setting is one. Fix: add a Known Exceptions entry to `sec-fail-closed.md` naming Cupcake's fail-open setting and pointing to cupcake learning L6.
- **Heading format (DEVIATION).** The cupcake and mb-compile-rules explainers number their section headings. The specification's titles carry no numbers, and other suites follow them. Fix: remove the numbers in both explainers.

### How it works

**Every tool call is checked.** Before a tool call runs, the plugin turns it into a structured event and asks the Cupcake checker for a decision. If any rule denies it, the call is blocked with the rule's own message. If nothing denies it, the call proceeds.

**Two kinds of rule.**

- **Builtins** ship with Cupcake: system protection, sensitive-data protection, and protection of the Cupcake binary from agent execution.
- **Custom rules** are written for this environment, one per concern. They cover test tampering, lockfiles, unverified packages, destructive database operations, history rewriting, autonomous commits, writes outside the current project, secret files, outbound network access, new tools or skills, the agent's own governing files, and Cupcake's own control plane.

**Rules are global.** Every custom rule lives in `~/.config/cupcake/policies/opencode/` and applies to every project. A project may also carry its own `.cupcake` folder with local rules. A folder without one falls back to the global rules only.

**Rules belong to Cupcake.** The plain-language rules in `rules-cupcake-global` and the compiled rule files are Cupcake's own configuration, recorded in Cupcake's config. mb-compile-rules's documents point there and hold no record of the rules.

**Grants open a door for a short time.** Some rules can be relaxed by a grant the developer issues in the WSL terminal:

- `mg-grant` opens a folder outside the project for a set number of writes.
- `net-grant` opens a website for a set number of fetches.
- `git-grant` allows a set number of commits, pushes or merges in one repository.

A grant expires after a fixed time (30 minutes by default) or when its count runs out. The agent can never issue a grant itself.

**Failure does not stop work.** If Cupcake itself fails, it is set to "fail open": the call is allowed and the failure is logged, rather than every tool call being blocked.

**How a rule is built (mb-compile-rules).**

1. The developer writes the rule in plain language in `C:\dev\project-library-global\rules-cupcake-global\`. The source keeps its own name.
2. The skill reads it in full and drafts the rule file in one complete write, named `cup-(rule-name).rego`, with one `cup-` prefix only.
3. It checks the file's syntax with `opa check`.
4. It places the file in the live folder and runs an immediate health check.
5. The developer runs the real block and allow tests in the WSL terminal.
6. The rule is kept only if the real tests pass. Otherwise it is removed at once.

### Why it works this way

- **A machine, not a request.** The agent cannot reason around a rule it cannot reach.
- **The agent cannot run Cupcake.** Cupcake blocks its own binary from the agent, so the agent can never weaken or test its own guard. Every real test is run by the developer.
- **Guards read the command text.** A command that names a protected path or tool is denied on its text alone, whatever it would do. Such checks are run by the developer.
- **Placement is the start of testing.** Cupcake only ever evaluates what is actually installed, so a rule cannot be tested on its own first. The health check right after placement catches a bad file in seconds.
- **One complete write per rule.** Partial edits that land out of order can leave a rule broken and stop all evaluation.
- **Only real tests count.** Some rules pass offline checks and still fail live. Only the developer's real output decides.
- **Time is handled outside the rules.** Some Rego functions fail silently inside Cupcake, including time functions. So grant expiry and counting are done by the plugin, and the rules only check whether a grant exists.
- **One record of the rules.** A second record of the rules in mb-compile-rules's documents would drift from Cupcake's.
- **Fail open.** A guard that blocks all work when it breaks gets switched off. Failing open keeps it trusted and in place.

### What has to happen

- Every new or changed rule goes through mb-compile-rules. No rule file reaches the live folder any other way.
- The developer runs every real test.
- The developer issues every grant, with the count the work needs.
- The plugin exists at exactly one path. A second copy, even a link to the same file, makes every check run twice.
- Plugin or global configuration changes take effect only after OpenCode is fully restarted.
- Every project `.cupcake` folder keeps at least one real rule file, or the engine fails to load.
- The installed copy of mb-compile-rules stays identical in body to its source after every source change.

### Where it stops

- It cannot see conversational approval. A rule that protects a file blocks every write, including one the developer just approved in chat.
- It cannot see inside a script file. It checks the command text only.
- It does not decide what should be forbidden. The developer decides, in the plain-language rule.
- It does not check configuration or write documentation. mb-config-manager and mb-sysdoc-creator do that.
- It is not a security review of the code being built. That is mb-sentinel-review's job.

### Its documents

[[[mybizz-config-docs:cupcake/cupcake-explainer]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-explainer.md) · [[[mybizz-config-docs:cupcake/cupcake-reference]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-reference.md) · [[[mybizz-config-docs:cupcake/cupcake-config]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-config.md) · [[[mybizz-config-docs:cupcake/cupcake-learnings]]](C:/mybizz/mybizz-config-docs/cupcake/cupcake-learnings.md) · [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-explainer]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-explainer.md) · [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-reference]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-reference.md) · [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-config]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-config.md) · [[[mybizz-config-docs:mb-compile-rules/mb-compile-rules-learnings]]](C:/mybizz/mybizz-config-docs/mb-compile-rules/mb-compile-rules-learnings.md). The skill's source is in its own repository, `C:\mybizz\mb-compile-rules\`. The plain-language rules are in `C:\dev\project-library-global\rules-cupcake-global\`.

---

## 7. mb-config-manager — examines each system against its standard

### What it is

mb-config-manager is an OpenCode agent that keeps every MyBizz skill and system configured to its standard. It examines a system, proposes corrections with their justification, and applies only what the developer approves. It never researches issuers and never writes document suites. It has mb-sysdoc-creator do both.

It is installed as `~/.config/opencode/agents/mb-config-manager.md` (mode: primary), with the command `~/.config/opencode/command/mb-config-manager.md`, which routes to it. Its source is `config-manager-agent.md`, and its one ongoing record is `config-manager-register.md`. Both are working files in its own folder, listed in its config. Its issuer is MyBizz. Its installed version is the latest commit of `config-manager-agent.md`, and the installed agent's body matches that source.

### Intent

**Remediation**
- Give commit-all its first examination, filling its register row, once its suite exists.

### How it works

**Invocation.**

- `/mb-config-manager (system-name)` examines one system.
- `/mb-config-manager` with no name reads the register, lists every Active system whose Last checked is empty or earlier than its Last updated, and asks which to examine.

**It holds nothing about any system.** One examination procedure serves every system. Every value, source and check is read fresh from the system's four documents on each run.

**One run, for one system:**

1. **Locate.** It reads its own learnings, the system's register row and its folder. A system with no register row gets one proposed. A system with no documents is sent to mb-sysdoc-creator first. A missing document, or a file the config does not list as a working file, is a GAP. Documents that are not in the specification's shape are one DEVIATION. Its correction is a full mb-sysdoc-creator Refresh, and every other correction to the documents folds into it. It then reads the learnings in full, then the config, then the reference.
2. **Issuer.** It reads the installed version. A version different from the config's means the system was changed outside a run: it reports this first and has mb-sysdoc-creator refresh the standard. A newer issuer release is BEHIND.
3. **Compare.** For every config row it compares the issuer standard, the recorded MyBizz value and the live value, weighs each difference against the learnings, and runs the `Works` check.
4. **Report.** Findings are given in chat only, numbered, each with its kind, the three values, the proposed correction and its justification.
5. **Decide.** The developer approves or rejects each finding. A BEHIND finding is a choice: upgrade, with exact steps the developer runs, or stay, with a learnings entry giving the reason.
6. **Apply.** Approved corrections are applied one at a time, each checked before the next, with the replaced value kept as the way back.
7. **Record.** It writes the approved config rows and learnings entries, updates the register row, gives revoke lines for open grants, and reminds the developer to commit.

**Finding kinds:** DOWN, UNHEALTHY, BEHIND, DRIFT, DEVIATION and GAP.

**The register** has one row per skill and system that exists, with the columns System · Kind · Docs folder · Installed version · Last updated · Last checked · State. State is Active or Deferred. A removed system has its row removed. The register holds no settings and no notes.

### Why it works this way

- **Present is not working.** A system can exist and do nothing. The `Works` check proves function.
- **Three-way comparison.** Comparing live state only with the record misses a record that is itself wrong. The issuer standard is the independent measure.
- **Nothing held about subjects.** Values held in two places go out of step. Reading the system's own documents on every run keeps one home for every value.
- **Learnings first.** The learnings hold what makes the MyBizz setup work. A difference they justify is a choice, not a fault.
- **Shape before rows.** A document in the wrong shape cannot be corrected row by row. One full Refresh corrects the shape and every row at once.
- **Upgrades are a decision.** An upgrade can break the systems downstream of it. The developer decides every one.
- **Findings in chat, history in Git.** Records kept for posterity contradict each other over time. The register row is the only record.
- **An agent, not a skill.** Every finding waits for the developer's decision, so the work is a conversation.

### What has to happen

- The developer selects the mb-config-manager agent with `Tab` before running the command, because the command runs in whichever agent is selected.
- After any change to `config-manager-agent.md`, the installed agent is updated to the same body, and the config and register record the new commit.
- The developer invokes it and decides every finding.
- The developer runs every action Cupcake reserves for the terminal, and issues every grant it requests.
- The developer runs every upgrade.
- The developer commits with commit-all.

### Where it stops

- It does not research issuers or write documents. mb-sysdoc-creator does that.
- It does not correct links or frontmatter. mb-doc-check does that.
- It does not change Cupcake's rules. mb-compile-rules does that.

### Its documents

[[[mybizz-config-docs:mb-config-manager/config-manager-explainer]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-explainer.md) · [[[mybizz-config-docs:mb-config-manager/config-manager-reference]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-reference.md) · [[[mybizz-config-docs:mb-config-manager/config-manager-config]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-config.md) · [[[mybizz-config-docs:mb-config-manager/config-manager-learnings]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-learnings.md) · [[[mybizz-config-docs:mb-config-manager/config-manager-agent]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-agent.md) · [[[mybizz-config-docs:mb-config-manager/config-manager-register]]](C:/mybizz/mybizz-config-docs/mb-config-manager/config-manager-register.md).

---

## 8. mb-doc-check — keeps every document and link correct at commit

### What it is

mb-doc-check is a small Python program, not an AI. It runs automatically as a Git hook every time documents are committed in WSL. It makes sure every document entering the history of a documentation repository is correct: it has complete frontmatter, every link is in one standard form, and every link points to a document that exists.

Its issuer is MyBizz. Its standard is `mb-doc-check.py` and `mb-doc-check.sources`, in the `mybizz-scripts` repository at `C:\mybizz\scripts\`. Its link form depends on GBrain's page-name rule and source list, so GBrain is the issuer for those.

### Intent

**Remediation**
- **Help text contradicts the stored form.** `mb-doc-check.py` lines 6 and 83 show the standard link with a relative path. The stored form uses absolute paths (`@paths = absolute`). This is doc-check-config open item 1. Fix: correct both lines in `mb-doc-check.py` to the absolute-path form, run `run-tests.sh`, and close the open item through `/mb-config-manager mb-doc-check`.
- **Heading format (DEVIATION).** The explainer's first section is titled "What this actually is" and its headings are numbered. The specification's title is "What it is", without numbers. Fix: correct the headings in the same run.
- **Open items format (DEVIATION).** The config writes its open items as a table. Other suites write them as a numbered list. Fix: write them as a numbered list in the same run.

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

A settings file, `mb-doc-check.sources`, says which folder is which GBrain source. Its source ids are copied exactly from GBrain's own list.

### Why it works this way

- **At commit, not on a schedule.** A document is correct at the moment it enters history, so errors never pile up.
- **Text only.** It never moves, renames or deletes a file. Where documents live is the developer's decision.
- **It blocks only new mistakes.** One old broken link must not stop all work. A broken link just typed costs one line to fix now.
- **It fails open.** If the program itself breaks, it says so and lets the commit through. A helper's bug must never stop work.
- **Same result every time.** The documents are written by AI agents, so the checker must not add variation of its own.
- **Source ids are GBrain's data.** A hand-written short form of a source id drifts from GBrain's list.

### What has to happen

- Commits are made in WSL, with commit-all, never with `--no-verify` and never from a Windows program.
- After any change to GBrain's sources, `sources-check` is run.
- After any GBrain upgrade, `slug-selftest` is run.
- When a commit is stopped, the named line is fixed and the commit is run again.
- A new documentation repository is brought under the hook before it is used.

### Where it stops

- It checks form and links, never meaning. It cannot tell whether a document is true.
- It never checks configuration. mb-config-manager does that.
- It does not cover code repositories, the Anvil clone or the memory governor's folder.
- It never edits an installed `SKILL.md` or anything in an `obsolete` folder.

### Its documents

[[[mybizz-config-docs:mb-doc-check/doc-check-explainer]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-explainer.md) · [[[mybizz-config-docs:mb-doc-check/doc-check-reference]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-reference.md) · [[[mybizz-config-docs:mb-doc-check/doc-check-config]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-config.md) · [[[mybizz-config-docs:mb-doc-check/doc-check-learnings]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-learnings.md) · [[[mybizz-os-docs:os-reference-docs/documentation-linking-explainer]]](C:/mybizz/mybizz-os-docs/os-reference-docs/documentation-linking-explainer.md). The program is `C:\mybizz\scripts\mb-doc-check.py`.

---

## 9. mb-project-docs — keeps each project's documents current

### What it is

mb-project-docs is a skill that keeps each project's own documents current. A project is one of `mb-3-cs`, `mb-4-ecom`, `mb-5-pdlf` and `pdlf`, as named in `repo-organization.md`. In each, it maintains:

- a **config** file: the project's state;
- an **explainer**: what the project is and why it is built the way it is;
- the project's **`AGENTS.md`**: how agents work in it, which doubles as the project's reference;
- a **README** that points to all three as required reading.

It works on one project at a time, the one the session is in. It documents projects, never systems. Project documents live in their own project repositories, not in `mybizz-config-docs`, and projects are not subjects of mb-config-manager. mb-project-docs itself is a subject, with its own suite.

Its issuer is MyBizz. Its standard is its source, `C:\mybizz\mb-project-docs\mb-project-docs.md`.

### Intent

**Remediation**
- **Unverified setting.** project-docs-config open item 1: whether Anvil keeps the `theme\docs` folder through its cloud round-trip. The folder exists locally in `mb-3-cs-app1`. Fix: the developer pushes `mb-3-cs-app1` to Anvil, pulls it back, and confirms `theme\docs` and its files survive. The result becomes a config row and the open item closes through `/mb-config-manager mb-project-docs`.
- **Open items format (DEVIATION).** The config writes its open items as a table. Other suites write them as a numbered list. Fix: write them as a numbered list in the same run.
- **Project paths out of date elsewhere.** `mybizz-scope-list.md` lines 78 to 83 name `dev-mb4ecom`, `mb4ecom`, `mb5pdlf` and `dev-mb-3-cs\mb-3-cs`. The projects are `mb-3-cs`, `mb-4-ecom` and `mb-5-pdlf`, with their libraries and `{slug}-app1` repositories. Fix: correct those lines to the paths in `repo-organization.md`.

### How it works

1. It reads the project's real state and what the session changed.
2. It compares that with what the files say and lists what is no longer true.
3. It creates any file that is missing.
4. It proposes small edits: corrections, plus what the session added, each to the file that owns it.
5. It prepares the `AGENTS.md` change as a draft. The developer applies it with one terminal line.

For each project it writes into the project library. Where the project has a code repository, that is App 1, `{slug}-app1`, the development workspace, and it writes there into `theme\docs`, but only if the Anvil app already exists. Otherwise it stops and asks the developer to create the app.

### Why it works this way

- **Three files, not one.** State, rules and reasons change at different speeds, so an update touches only the file that is wrong.
- **The reference is the `AGENTS.md`.** An agent's rules and a project's working reference are the same content. One file prevents two copies drifting apart.
- **The agent does not write `AGENTS.md`.** The file governs the agent. The developer applies its changes.
- **Small edits, never regeneration.** State can be found again. Reasons and decisions cannot, and a rewrite would lose them.
- **It runs at close-out.** That is when the session's changes are known and the developer is already approving.
- **Its project table follows `repo-organization.md`.** A table that names a folder that no longer exists makes the skill stop in that project.

### What has to happen

- It runs inside mb-close-out, or when the developer invokes `/mb-project-docs`.
- The developer approves its changes at close-out's single approval step.
- The developer applies the `AGENTS.md` change with the terminal line it provides.
- When a project folder or repository is renamed, the skill's project table is updated in its source and the skill reinstalled.

### Where it stops

- It does not document systems. mb-sysdoc-creator does that.
- It does not correct links. mb-doc-check does that, and only in documentation repositories.
- It does not work across several projects at once.

### Its documents

[[[mybizz-config-docs:mb-project-docs/project-docs-explainer]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-explainer.md) · [[[mybizz-config-docs:mb-project-docs/project-docs-reference]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-reference.md) · [[[mybizz-config-docs:mb-project-docs/project-docs-config]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-config.md) · [[[mybizz-config-docs:mb-project-docs/project-docs-learnings]]](C:/mybizz/mybizz-config-docs/mb-project-docs/project-docs-learnings.md). The skill's source is in its own repository, `C:\mybizz\mb-project-docs\`.

---

## 10. mb-close-out and commit-all — save a session's work

### What it is

**mb-close-out** is a skill that ends a section of work without losing anything that matters: what was decided, what changed on disk, and which facts are worth trusting later. It does no new work of its own. It runs the existing pieces in order and gathers every decision into one approval. Its issuer is MyBizz. Its standard is its source, `C:\mybizz\mb-close-out\mb-close-out.md`.

**commit-all** is a script, `C:\mybizz\scripts\commit-all.sh`. It commits every documentation repository, refreshes GBrain and pushes everything to GitHub with one command. It is close-out's last step, and it can also be run on its own whenever work needs committing without a close-out. Its issuer is MyBizz. Its standard is `commit-all.sh`. It is a system in its own right, with its own folder, `mybizz-config-docs/commit-all/`.

The two stay separate. mb-close-out ends a section of work. commit-all commits, syncs and pushes, alone or as close-out's last step.

### Intent

**Remediation**
- Create the commit-all folder and its four documents through mb-sysdoc-creator's Create.
- Point the commit-all operating procedure, and mb-close-out's reference, to the commit-all suite once it exists, and close mb-close-out's config open item.
- Make commit-all keep the last run's log only, overwriting it on each run. `commit-all.sh` line 41 writes a new dated log file to `~/backups/` on every run.
- **Procedure lists the wrong repositories.** `commit-all-repositories.md` lists `mb4ecom-project-library` and `mb5pdlf-project-library` and omits `scripts`. `commit-all.sh` commits `mb-4-ecom-project-library`, `mb-5-pdlf-project-library` and `scripts`, eighteen repositories in all. Fix: make the procedure point to the list in `repo-organization.md` and `commit-all.sh` instead of copying it.
- **Learnings header missing (DEVIATION).** mb-close-out-learnings lacks the purpose sentence every other learnings file opens with. Fix: add it through `/mb-config-manager mb-close-out`.

### How it works

**mb-close-out, step by step:**

1. **Session page.** It writes a summary of the session as a file under `sessions/`, named by date and topic. commit-all brings it into GBrain.
2. **Facts.** It lists the session's lasting rules, decisions and pitfalls, one claim each, and sorts them:
   - **advisory** facts, which only need to be found again, are saved straight to GBrain;
   - **governed** facts, which must be trusted later without re-checking, are written as fact files and sent through `mb-submit-memory` and the memory governor's checks.
3. **Project documents.** If the session worked in one of the four projects, it runs mb-project-docs.
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

**Session pages stay.** They are working memory for GBrain recall, not history.

### Why it works this way

- **One orchestrator over existing pieces.** Every piece already had its own procedure. Close-out sequences them rather than duplicating them.
- **One approval, not three.** mb-submit-memory and mb-project-docs each need approval. Close-out's single approval satisfies both.
- **Terminal lines, not agent authority.** The agent holds no Git authority, no credentials and no power to grant access. Every such action is a line the developer pastes.
- **Lines contain only known values.** A command built from remembered output is where a wrong value enters a privileged action.
- **An empty review queue ends the close-out.** An open review item blocks work everywhere, so close-out does not end with one pending.
- **Close-out and commit-all are separate.** Work often needs committing before a section is finished. commit-all serves that alone; close-out uses it as its final step.
- **One log, the last run.** The log exists only to diagnose the run just made. Older logs serve no purpose.

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

[[[mybizz-config-docs:mb-close-out/mb-close-out-explainer]]](C:/mybizz/mybizz-config-docs/mb-close-out/mb-close-out-explainer.md) · [[[mybizz-config-docs:mb-close-out/mb-close-out-reference]]](C:/mybizz/mybizz-config-docs/mb-close-out/mb-close-out-reference.md) · [[[mybizz-config-docs:mb-close-out/mb-close-out-config]]](C:/mybizz/mybizz-config-docs/mb-close-out/mb-close-out-config.md) · [[[mybizz-config-docs:mb-close-out/mb-close-out-learnings]]](C:/mybizz/mybizz-config-docs/mb-close-out/mb-close-out-learnings.md) · [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md). The skill's source is in its own repository, `C:\mybizz\mb-close-out\`.

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
  - The folder holds exactly four documents, found by their endings: `-config.md`, `-reference.md`, `-explainer.md` and `-learnings.md`.
  - An integrity system's folder may also hold the working files that system needs to run, each listed in its own config. mb-sysdoc-creator's holds the specification. mb-config-manager's holds its agent source and the register. Nothing else is kept in any folder.
  - The folder name is the system's name. A system MyBizz built starts with `mb-`. An external system keeps its issuer's name.
  - A new document takes the same name start as the documents already in its folder. In a new or empty folder it takes the folder name. No document is ever renamed, because links to it exist in places mb-doc-check does not repair.
  - The integrity systems are subjects too, and keep the same four documents in their own folders.
- **The issuer's specification is the standard.**
  - mb-sysdoc-creator sets it.
  - mb-config-manager keeps it current, through mb-sysdoc-creator's Refresh mode.
  - For a system MyBizz built, the installed version is the latest commit of its source. Its source is the standard.
- **Learnings hold only what is true now.**
  - Each learnings file records updates, preferences, workarounds and unique settings found to be relevant.
  - Each entry has an ID (`L1`, `L2` and so on), what it is, why it holds, and the source or finding it came from. No entry carries a date or history.
  - A config row that differs from the standard points to its entry.
  - An entry that stops being true is removed, and any row pointing to it is corrected in the same change.
- **Learnings are read first.**
  - Learnings hold the nuances and unique settings that make the MyBizz setup work.
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

### Shared remediation

- **This document has a duplicate.** `os-reference-docs\system-integrity-explainer-v5.md` holds an earlier text of this document under a second doc-id. Fix: delete it with `git rm`.
- **Working reports.** The six config-management reports in `os-reference-docs\reports\` record work this document now holds. Fix: move them to `mybizz-os-docs\obsolete\`.
- **One format in every suite.** Explainer headings carry the specification's titles, without numbers. Open items are a numbered list, or the word "None". Every learnings file opens with the same purpose sentence. Each suite's remediation above applies this rule to its own documents.

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
