# Rewriting History Safely

By the end of this chapter you have
folded one change into another with
`jj squash`, met the error that
stopped the lab's first attempt, and
can say precisely why rewriting in
Jujutsu is not the dangerous
operation it is in Git.

## Why rewriting is safe here

Rewriting feels dangerous in Git for
three reasons: the old version is
hard to find afterwards, other work
built on the old version is
stranded, and there is no undo if
the rewrite itself was the mistake.
Jujutsu removes all three by
construction. The change ID keeps
meaning the same piece of work
(chapter 6). Descendants are rebased
automatically (chapter 6 again).
And every rewrite is an operation,
recorded in the operation log, so
`jj undo` (chapter 10) restores the
pre-rewrite graph exactly. Rewriting
is not a special risky mode of this
tool; it is Tuesday.

## jj squash: folding a fix into its parent

Late in the lab's story, a small
change — "Add sparkling water" —
existed only because its one line
belonged in the change beneath it,
"Add iced tea". `jj squash` folds
`@` into its parent:

```text
$ jj squash
Error: Failed to edit description
Caused by:
1: Failed to run editor 'nano'
2: No such file or directory (os error 2)
Hint: Edited description is left in /tmp/editor-nnpdfX.jjdescription
```

The lab's first attempt failed in
full, and the failure is worth more
than a success would have been.
Squash normally opens your editor on
the *combined* description of the
two changes, inviting you to edit
the merged message. This machine
had no `nano` — the editor its
configuration named — and no fall
back, so the command stopped. Note
what it did *not* do: it did not
half-apply the fold. The graph after
the error was the graph before it,
verified in the transcript.

Two ways forward. Install or
configure a real editor (chapter 3),
or decline the editing step —
`--use-destination-message` keeps
the parent's description and skips
the editor entirely:

```text
$ jj squash --use-destination-message
Working copy  (@) now at: vllpmktx b513309a (empty) (no description set)
Parent commit (@-)      : kzkzkvtk 46a5cafd picnic-plan* | Add iced tea
```

The change "Add sparkling water"
is gone as a separate entry; its
line is inside the parent, whose
commit ID changed (`46a5cafd`) while
its change ID (`kzkzkvtk`) and its
description did not. Jujutsu leaves
you standing on a fresh empty change
above it, ready for the next piece
of work. (`jj show picnic-plan`
afterwards lists four lines in
`drinks.txt` — lemonade, paper cups,
iced tea, sparkling water — the
folded line home at last.)

The whole episode, error included,
is transcript
`jujutsu-lab/transcripts/t13-squash.txt`.

## The rest of the rewriting shelf

The lab verified the two rewrites
above and chapter 6's ancestor edit.
The neighbouring operations are
described from the documentation and
labelled as such in `criteria.md`:

- **`jj split`** divides one change
  into two along file (or hunk)
  lines — the mirror of squash, for
  the change that grew two thoughts.
  Chapter 12's second recipe uses
  it in spirit.
- **`jj rebase`** moves a change and
  its descendants onto a different
  parent explicitly, for when the
  *shape* of the graph is wrong, not
  its contents.
- **`jj abandon`** deletes a change
  outright, rebasing its children
  onto its parent. The lab did run
  this once, to remove a change that
  had landed in the wrong place
  (transcript `t06-bookmark-fixed.txt`),
  and chapter 12's fourth recipe
  explains recovering from it via
  the operation log when the
  deletion was a mistake.

## Check

After a squash, `jj log` shows one
change where two stood, the parent's
change ID unchanged and its contents
grown; and if any of it was wrong,
`jj undo` (chapter 10)
brings both changes back.
