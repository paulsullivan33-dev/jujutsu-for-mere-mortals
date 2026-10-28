# Bookmarks

By the end of this chapter the
picnic plan has a name that Git can
see, the name has been moved forward
as the work grew, and you have been
refused — politely, and for a good
reason — when you tried to move it
sideways.

## A bookmark is a name on a change

Git's *branches* are names that move
on their own: commit while a branch
is checked out and the branch
advances with you. Jujutsu's
**bookmarks** are names that stay
where you put them. Nothing moves a
bookmark except a command about
bookmarks. That single difference
removes a whole family of surprises:
you can stack three new changes
above a bookmark and it still points
at the finished work it was naming,
until you deliberately move it.

Create one at the Drinks and cups
change:

```text
$ jj bookmark create picnic-plan -r qnzlmsnr
Created 1 bookmarks pointing to qnzlmsnr 4a29b132 picnic-plan | Drinks and cups
```

`-r` names the change the bookmark
points at; without it, `@` is
assumed. `jj bookmark list` reads
all of them back:

```text
$ jj bookmark list
picnic-plan: qnzlmsnr 4a29b132 Drinks and cups
```

## Moving a bookmark forward

Work continued: a new change,
"Blanket and games", was written on
top of the bookmarked one, and the
plan's name should follow it.
`jj bookmark set` moves an existing
bookmark to another change:

```text
$ jj bookmark set picnic-plan -r @
Moved 1 bookmarks to nvqmkrsm 39f0eac8 picnic-plan* | Blanket and games
```

The asterisk matters: `picnic-plan*`
means the bookmark's local position
and its position in the Git store
disagree — Git has not been told
yet. The next export, fetch or push
(chapter 11) settles it, and the
asterisk disappears.

## The refusal

Earlier in the lab, that same move
failed, and the failure teaches the
one rule bookmarks enforce. `@` had
wandered: a new change was created
from the *packing list* change
rather than from the bookmarked one,
so `@` sat on a side branch of the
graph, not ahead of the bookmark:

```text
$ jj bookmark set picnic-plan -r @
Error: Refusing to move bookmark backwards or sideways: picnic-plan
Hint: Use --allow-backwards to allow it.
```

A bookmark may move *forward* —
along descendants of the change it
names — freely. Moving it backwards,
or sideways onto an unrelated line
of work, would quietly rename which
history the name stands for, so
Jujutsu refuses unless you insist
with `--allow-backwards`. In the
lab's case the sideways move was
simply a mistake — the change had
been created in the wrong place —
and the honest fix was to abandon
that change, return to the
bookmarked change with `jj edit`,
and build on top of it properly
(transcript
`jujutsu-lab/transcripts/t06-bookmark-fixed.txt`).

The lesson generalises: when
Jujutsu refuses a bookmark move, it
has usually noticed that your `@`
is not where you think it is. Check
`jj log` before reaching for the
override flag.

## Bookmarks and Git branches

To Git, your bookmarks are branches
with the same names — chapter 11
pushes `picnic-plan` to a bare Git
repository and reads it back with
`git log`, where it appears as an
ordinary branch. Within Jujutsu you
may also see names like
`picnic-plan@origin` (the position
the remote reported) and
`picnic-plan@git` (the position in
the local Git store). One name,
three vantage points; the bare name
is yours to move.

## Check

`jj bookmark list` shows your
bookmark on the change you intended;
`jj log` prints the bookmark's name
beside that change; and you have
seen the sideways refusal and know
what it is protecting.
