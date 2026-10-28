# Your First Repository

By the end of this chapter you have a
repository, you can read its two
everyday views — `jj log` and `jj st`
— and you have met the strangest
sight in the guide: plain Git
insisting, in a folder Jujutsu is
actively managing, that nothing has
ever happened there.

Make a folder for the picnic project
and stand in it. Then:

## jj git init

```text
$ jj git init --colocate
Initialized repo in "."
```

One line, and two folders appear:
`.jj` (Jujutsu's own record, chapter
2) and `.git` (the Git store beside
it). The `--colocate` flag asks for
exactly this side-by-side layout. In
Jujutsu 0.46 colocated is already the
default, so plain `jj git init` makes
the same arrangement — the lab ran
both ways and compared the result
(transcripts t01 and t15). Spelling
the flag out costs nothing and keeps
the intention visible.

(To start Jujutsu on an *existing*
Git project instead, the command is
the same, run inside that project's
folder: `jj git init --colocate`
adopts the Git repository that is
already there. That route is
described from the documentation in
this guide; the lab started from
empty folders throughout.)

## Reading jj log before anything exists

```text
$ jj log
@  ukqvskxp lab@example.com 2026-10-28 17:54:57 0e16a74f
│  (empty) (no description set)
◆  zzzzzzzz root() 00000000
```

Two entries, read bottom-up. The
◆ marks a change other tools may
not move: `zzzzzzzz` is the **root**,
a permanent empty change that anchors
every repository's graph. Above it,
marked `@`, is your working-copy
change — empty, undescribed, and
already real. The words after the
email are the date; the hex at the
end is the commit ID. The letters
before the email, `ukqvskxp`, are
the change ID, and they are the ID
this guide will follow around: yours
will differ, because change IDs are
minted fresh for every repository.

## Reading jj st

```text
$ jj st
The working copy has no changes.
Working copy  (@) : ukqvskxp 0e16a74f (empty) (no description set)
Parent commit (@-): zzzzzzzz 00000000 (empty) (no description set)
```

`jj st` (short for `jj status`)
answers three questions: what has
changed in the folder (nothing, yet),
which change the folder is editing
(`@`, identified by both IDs), and
what sits underneath it (`@-`, the
root). Every later chapter leans on
these two lines.

## The strange sight

Because the repository is colocated,
plain Git can be asked about it:

```text
$ git log --oneline
fatal: your current branch 'main' does not have any commits yet
```

Git is not wrong, and nothing is
broken. Your working-copy change
exists as a Git *commit object*, but
no Git *branch* points at it yet, and
`git log` follows branches. Jujutsu
exports names to Git when there is a
reason to — when you create a
bookmark that Git should know about,
or when you push and fetch
(chapter 11) — not before. Until
then, Git sees an empty repository
with good manners. Chapter 15 lists
this among the traps, because
everyone meeting Jujutsu for the
first time walks into it once.

## Check

`ls -a` shows `.git` and `.jj` side
by side; `jj log` shows your `@`
above the root; `jj st` names the
same change. The full session is
transcript
`jujutsu-lab/transcripts/t01-init.txt`.
Next, the folder gets contents — and
you do not have to tell Jujutsu.
