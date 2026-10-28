# Troubleshooting

The traps below are ordered by how
the lab met them, which is the order
most readers will too. Each names
the symptom as the tool reports it,
the cause, and the way out.

## git log says there are no commits yet

**Symptom:** in a fresh colocated
repository, `git log` answers
`fatal: your current branch 'main'
does not have any commits yet`,
while `jj log` shows your work
plainly (chapter 4).

**Cause:** your changes exist as Git
commit objects, but no Git branch
points at them yet, and `git log`
follows branches.

**Way out:** nothing is broken. Give
Git a name to follow — create a
bookmark (chapter 8) and Jujutsu
exports it to the Git store as part
of its normal work; in a colocated
repository that export is automatic
(the lab asked for it by hand and
was told `No export needed in
colocated workspaces`), while a
stored-elsewhere repository exports
on push, fetch, or an explicit
`jj git export`. After that,
`git log` reads the same history.

## The bookmark will not move

**Symptom:** `Refusing to move
bookmark backwards or sideways`,
with a hint about `--allow-backwards`
(chapter 8).

**Cause:** almost always that `@`
is not a descendant of the change
the bookmark names — most often
because `jj new` was run while
standing on an older change, and
the new work grew on a side branch.

**Way out:** read `jj log`, stand
on the right change with `jj edit`,
and build from there. Keep
`--allow-backwards` for the rare
case where renaming history is
genuinely what you mean.

## A command fails about the editor

**Symptom:** `Error: Failed to edit
description ... Failed to run editor
'nano' ... No such file or directory`
(chapter 7).

**Cause:** the command needed to
open a text editor — `jj describe`
without `-m`, `jj squash`, `jj split`
— and the configured editor is not
installed.

**Way out:** install the editor, or
point Jujutsu at one that exists
with `jj config set --user ui.editor`,
or pass the text on the command line
(`-m`) or a no-editor flag
(`--use-destination-message`) where
one is offered. The failed command
applied nothing; run it again.

## --git-repo refuses a path

**Symptom:** `jj git init --git-repo
somewhere` answers `Cannot access
... No such file or directory`
(chapter 2).

**Cause:** `--git-repo` names an
existing Git repository; Jujutsu
does not create it for you.

**Way out:** create the store first
(`git init --bare somewhere`), then
run the `jj git init` again.

## Your edits are not in the change

**Symptom:** `jj st` shows no
changes, but you definitely edited
a file.

**Cause:** usually one of two quiet
facts. You are standing in a
different folder than the
repository's root (a sibling
project, a second clone), so this
repository's snapshot cannot see
the file. Or the file matches an
ignore rule — `.gitignore` applies
in Jujutsu too — and ignored files
are never snapshotted.

**Way out:** `jj root` prints the
repository you are actually standing
in — the lab's answered with its
picnic folder's full path. For the
ignore case, read the rules: the lab
watched a `build.log` stay invisible
while `.gitignore` matched it, and
appear in `jj st` — recorded, with
no add step — the first time it
looked after the rule was removed
(transcript `t18-ignore.txt`).

## jj log shows a change twice, or oddly

**Symptom:** the same description
appears on two lines, or a bookmark
seems to be in two places.

**Cause:** history was rewritten,
and what you are seeing are two
*vintages* of one change, or a
bookmark's local position beside
its remote one (`name@origin`).
Both are the graph showing its
working.

**Way out:** compare the IDs, not
the descriptions (chapter 6): same
change ID means the same piece of
work at different moments; different
change IDs mean genuinely different
work that happens to share a title.
Bookmarks settle on the next fetch
or push, or by your deliberate move.

## Everything is wrong and it was fine at lunch

**Symptom:** the graph, bookmarks
or descriptions are in a state you
do not recognise.

**Way out:** do not unpick by hand.
`jj op log` (chapter 10) shows every
operation with its command line;
find the moment it was last right
and step back to it with `jj undo`,
repeatedly if needed. This is the
troubleshooting step Jujutsu owns
outright: most "I have ruined the
repository" afternoons in Git are,
here, a two-command inconvenience.
