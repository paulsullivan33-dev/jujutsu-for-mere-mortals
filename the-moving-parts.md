# The Moving Parts

By the end of this chapter you can
name every piece of a Jujutsu
repository and say where it lives on
disk. Four pieces do nearly all the
work: the Git store underneath, the
`.jj` folder on top, the working-copy
change, and the operation log.

## The Git store underneath

With the **Git backend** — the only
backend this guide uses — every
change you make is stored as an
ordinary Git commit in an ordinary
Git repository. Jujutsu writes those
commits and reads them back; it does
not keep a private copy of your
history in a format only it can read.
That is why a repository made by
Jujutsu can be pushed to any Git
host, cloned by any Git user, and
inspected with Git commands whenever
you like (chapter 11 does exactly
that, repeatedly).

Where the Git repository lives is a
choice made once, at `jj git init`:

- **Colocated** (the default in
  current Jujutsu): the Git repository
  is the `.git` folder sitting right
  beside `.jj` in your working folder,
  exactly where a plain `git init`
  would put it. Git tools run in the
  folder see the repository directly.
  This guide's lab is colocated.
- **Stored elsewhere**: `--git-repo`
  points Jujutsu at a Git repository
  somewhere else on disk. Your working
  folder then contains only `.jj`, and
  plain `git` commands in the folder
  find nothing. The store must already
  exist — the lab tried pointing at a
  path that was not there yet and was
  refused (transcript
  `jujutsu-lab/transcripts/t16-git-repo.txt`).

Either way, the link is recorded in a
small file, `.jj/repo/store/git_target`,
whose entire contents are the path to
the Git repository. The lab read both
of its repositories' link files:

```text
$ cat .jj/repo/store/git_target
../../../.git
```

That is the colocated case: from
`.jj/repo/store`, three steps up is
the working folder, and `.git` is in
it. Nothing about the arrangement is
hidden or magical — it is a path in
a file, and chapter 15 returns to it
when something goes wrong.

## The .jj folder

`.jj` holds everything that is
Jujutsu's own: the record of which
change your working copy is editing,
the operation log, and indexes that
make `jj log` fast. You will almost
never open it by hand; the one file
worth knowing is the `git_target`
above, because it explains which Git
repository you are really using.

## The working-copy change

In Git, your folder holds files and a
separate step copies chosen files
into a commit. In Jujutsu, the folder
*is* a change. One change in every
repository is special: the
**working-copy change**, written `@`
in nearly every command's output.
Before running a command, Jujutsu
**snapshots** the folder — compares
what is on disk with what it last
recorded, and updates `@` to match.
Creating, editing and deleting files
with any tool at all — editor,
script, file manager — is therefore
already version control. Chapter 5
lives on this idea.

Every other change is reached through
`@`'s family: `@-` is its parent,
and the log graph (chapter 4) shows
how changes stack.

## Changes, parents, and the graph

A **change** has a description (its
message), an author, a set of file
contents, one or more **parents**
(the changes it builds on), and its
two IDs. A change with two parents is
a **merge**; chapter 9 makes one on
purpose. History in Jujutsu is
therefore a graph of changes, drawn
by `jj log` with `@` marking where
you stand.

## Bookmarks

A **bookmark** is a name — like
`picnic-plan` — pointing at a change.
Git calls these branches; Jujutsu
renamed them because theirs behave
slightly differently (they do not
move just because you commit on top
of them; chapter 8). Bookmarks are
the names Git sees: when Jujutsu
pushes, each bookmark becomes a Git
branch of the same name.

## The operation log

Separate from the history of your
files, Jujutsu keeps a history of
*itself*: every operation — snapshot,
describe, new, rewrite, fetch, push —
is an entry in the **operation log**,
with the arguments that caused it.
`jj op log` reads it; `jj undo`
steps it back. This is the piece Git
has no equivalent of, and chapter 10
is devoted to it. For now, hold the
distinction: `jj log` shows the
history of the *work*; `jj op log`
shows the history of the *tool's
actions on the work*.
