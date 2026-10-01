---
title: "documentation-linking-explainer"
doc-id: "mb-15cd9959"
updated: "2026-10-01"
---
# Documentation linking — Explainer

*Companion note: the rules a document must meet are in [[[mybizz-config-docs:gbrain/gbrain-doc-contract]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-doc-contract.md) and
[[[mybizz-os-docs:os-reference-docs/frontmatter-conventions]]](C:/mybizz/mybizz-os-docs/os-reference-docs/frontmatter%20conventions.md); the procedure to follow every time you finish work is
[[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md); GBrain's side of the story is in [[[mybizz-config-docs:gbrain/gbrain-explainer]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-explainer.md); the
repositories and their GBrain sources are listed in [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md).*

## 1. What this actually is

Every link from one document to another, in all eight documentation repositories, is stored in
one fixed form:

`[[[source-id:page-name]]](C:/full/windows/path.md)`

When the link has display text, the text sits after a pipe inside the square brackets:
`[[[mybizz-config-docs:gbrain/gbrain-config|Configuration]]](C:/mybizz/mybizz-config-docs/gbrain/gbrain-config.md)`.

That one line is read by two different programs, and each reads the part it needs. **GBrain**, the
memory system, reads the part inside the square brackets: which repository (`source-id`) and which
page in it (`page-name`). That is enough to connect pages, even across repositories. **MarkText**,
the editor you read documents in, reads the part in round brackets: a real file path that it opens
when you hold Ctrl and click.

Nobody writes this form. AI agents, and you, write the short form `[[name]]` or
`[[name|shown text]]`. A small program, `mb-doc-check`, runs inside every commit (the "commit
hook"), rewrites the link into the full form, and keeps it correct from then on. The same program
gives every document its frontmatter, repairs links when documents are renamed, moved or deleted,
and stops a commit that contains a link pointing to nothing.

## 2. The system in plain terms

**The eight repositories.** `mybizz-os-docs`, `mybizz-config-docs`, `data-mybizz-mgt`, `pdlf`,
`dev-root`, `makepdlf-project-library`, `mb-3-cs-project-library` and `project-library-global`.
Each is also a GBrain source with its own id; the full list is in [[[mybizz-os-docs:os-reference-docs/repo-organization]]](C:/mybizz/mybizz-os-docs/os-reference-docs/repo-organization.md). A small
file, `mb-doc-check.sources`, tells the hook which folder is which source id. A second file,
`mb-doc-check.roots`, lists the folders a link may point into.

**The page name.** GBrain names a page after its file: the path from the root of the repository,
in lower case, with spaces turned into hyphens and without `.md`. `Tools and apps/Strix.md` in a
repository becomes `tools-and-apps/strix`. The hook uses GBrain's own rule, so the name it writes
is the name GBrain expects.

**Three frontmatter fields.** Every document starts with `title`, `doc-id` and `updated`, except the files and folders listed in [[[mybizz-config-docs:mb-doc-check/doc-check-config]]](C:/mybizz/mybizz-config-docs/mb-doc-check/doc-check-config.md).
- `title` is the file name without `.md`.
- `doc-id` is a permanent random code such as `mb-3fa91c2e`. It is created once and never changes,
  whatever happens to the file.
- `updated` is the date of the last real change to the document, for example `"2026-10-01"`.

**Two ways a link can be wrong.** It can point to nothing (the document does not exist), or it can
name something that exists in several places (`README` exists in many repositories). The hook
catches both at the moment you commit.

## 3. What the commit hook does

At every commit, for every document in that commit:

1. **Completes the frontmatter.** Adds a missing `title`, a missing `doc-id` and the `updated`
   date. It never changes an existing `title` or `doc-id`.
2. **Writes every link in the full form.** It converts `[[name]]`, `[[folder/name]]`,
   `[[source-id:name]]`, ordinary markdown links to documents, and document paths pasted into the
   text.
3. **Repairs links after a rename or a move.** It recognises the document by its `doc-id`, not by
   its name or its content, so a document that was renamed and completely rewritten is still found.
   Links to it are corrected in every repository, not only the one being committed.
4. **Turns links to a deleted document into plain words.** The sentence stays; the dead link goes.
5. **Follows a document into `obsolete`.** When you retire a document by moving it into an
   `obsolete` folder, links to it follow it there.
6. **Stops the commit** when a link you just typed points to nothing, or to a name several
   documents share, or when the frontmatter is damaged. The message names the line and, for a
   shared name, the exact links you can write instead.
7. **Stores every document with LF line endings.**
8. **Moves the `updated` date only for a real change.** A document that is new, moved, or changed
   in anything other than its date line gets today's date. A document whose only difference is the
   date line, or its line endings, keeps its old date.

What it never does: move, rename or delete a file; touch `SKILL.md` files (installed skills) or
anything inside an `obsolete` folder; or commit for you.

## 4. Why it works the way it does

**One form, and the person writing does not choose.** The documents are written by AI agents. A
rule with two options is applied inconsistently, because every agent has to decide, and the
decision is made differently each time. So there is exactly one stored form, one short form to
write, and a program that converts the second into the first. No one has to remember anything.

**The source id inside the link.** GBrain connects a link to a page in another repository only when
the link names that repository. A link with only a name reaches only its own repository. The hook
knows which folder is which source from `mb-doc-check.sources`, so the writer never has to. A link
is followed across repositories when the page that holds it sits in a repository that is
federated, which all eight are. The code repositories are deliberately not.

**The full path outside the link.** MarkText opens the file named in the round brackets. A full
Windows path depends only on the target document: it does not depend on where the linking document
sits or on how MarkText was opened, and nothing has to be calculated per document. The cost is that
the paths describe this computer's drive layout.

**Both parts in one place.** The page name and the path describe the same target. Writing them
together, and recomputing the path from the page name at every commit, means they cannot drift
apart.

**A permanent `doc-id`.** Names and folders change; content gets rewritten. Only a code that never
changes lets a program say with certainty that this is the same document as before, and that is
what makes automatic repair of links possible.

**`updated`, and never `date`.** The date shows when a document last really changed, and it is set
by the program so that it can be trusted. It is not called `date` because GBrain reads a field with
that name as the page's own date and may use it to rank search results.

**The date moves only on a real change.** A date that moves for any reason, such as a line-ending
conversion, stops meaning anything. So a commit whose only effect on a document would be the date
is refused with "NOTHING TO COMMIT", and the document is put back exactly as it was.

**Pipes in tables.** In a markdown table the pipe character separates cells. A link with display
text inside a table cell would cut the cell in two, so inside a table row the pipe in front of the
display text is written `\|`. MarkText and GBrain both read that correctly.

**The same repository first.** Names such as `README` or `AGENTS` exist in several repositories.
A link written in a document means the one in its own repository. Only when that is not unique, or
the name exists only elsewhere in several places, does the hook ask you to name the repository.

**Names ignore upper and lower case.** GBrain page names are lower case and Windows file names do
not care about case, so `[[agents]]` finds `AGENTS.md`.

**A link to a deleted document becomes plain words.** A link that points to nothing misleads an
agent that follows it and adds noise to GBrain. The words keep the sentence readable and honest.

**A pasted path is a link; a path in backticks is text.** Pasting the path of a document is the
natural way to refer to it, so the hook turns it into a link. But a path can also be a statement of
where something lives. Putting it in `backticks` is the one switch that says "this is text", and
the hook never touches anything inside backticks or code blocks. A document that states its own path
is not linked to itself.

**The hook runs at commit, not on a schedule.** Every document is correct at the moment it enters
Git history, so nothing accumulates and no cleanup run is ever needed.

**One line ending.** Git, WSL tools and AI agents all write LF. A mix of LF and CRLF produces noise
in comparisons and warnings in editors, so all documents are stored with LF and MarkText is set to
LF.

**Commits are made from WSL.** The hook lives in the Git that runs in WSL. Commits made from a
Windows program bypass it.

## 5. What has to happen

- **Write links as `[[name]]` or `[[name|text]]`.** Never write the full form by hand. When the hook
  reports a shared name, write `[[source-id:name]]` as the message shows. Put a path in backticks
  when it only states where something is.
- **Commit with one command.** `commit-all.sh` commits the eight repositories, updates GBrain and
  pushes. Never skip the hook with `--no-verify`. The procedure is [[[mybizz-os-docs:operating-procedures/commit-all-repositories]]](C:/mybizz/mybizz-os-docs/operating-procedures/commit-all-repositories.md).
- **Let GBrain read links across repositories.** `sync-gbrain.sh` ends with a full read of the
  database (`gbrain extract links --source db`). That step is what writes links between
  repositories. The ordinary sync marks the pages it imports as finished and cannot resolve a link
  into another repository, so a lighter second pass finds nothing to do.
- **Keep the source map true.** `mb-doc-check.sources` must match GBrain's list of sources.
  `mb-doc-check.py sources-check` compares them and must say "everything matches".
- **Adding a repository** means adding it to GBrain and federating it, adding a line to
  `mb-doc-check.sources` and a folder to `mb-doc-check.roots`, installing the hook, and bringing its
  existing documents under the rules with `rollout-repo.sh`.

## 6. Suitability

- **Best for:** documentation written mostly by AI agents across several repositories, where the
  documents must refer to one another, be searchable as one body of knowledge, and be reorganised
  without breaking.
- **Good for:** renaming and moving documents freely; retiring documents into `obsolete`; reading
  and following links in MarkText; letting GBrain answer questions that cross repository
  boundaries.
- **Not good for:** reading the documents on GitHub, where the local paths do not open; moving a
  repository to another drive or folder, because every path to it would be wrong; links into the
  code repositories, the two dormant libraries and the memory governor's folder, which are outside
  the eight and have no clickable path.

## 7. S.W.O.T.

**Strengths.** One form everywhere. Links are repaired automatically after any rename or move.
Links across repositories reach GBrain. Documents are checked as they enter history.

**Weaknesses.** The long form is noisy to read in raw text, and in MarkText it shows its brackets.
The paths tie the documents to this computer's folder layout. Only commits made in WSL are covered.

**Opportunities.** Bringing the two dormant libraries and, if wanted, other repositories under the
same rules. Using the stable `doc-id` for further automatic checks.

**Threats.** A commit with `--no-verify` leaves a document in the wrong form. A mismatch between
`mb-doc-check.sources` and GBrain's source list makes links point to a source that does not exist.
A GBrain upgrade that changes how page names or links are read would need the hook's page-name rule
to be checked again; `mb-doc-check.py slug-selftest` does that check.
