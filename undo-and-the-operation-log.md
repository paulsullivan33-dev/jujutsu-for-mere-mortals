# Undo and the Operation Log

By the end of this chapter a
description you regretted has been
un-happened — not edited back, but
removed from the repository's past
as if the command had never run —
and you understand the ledger that
makes that possible.

## Two histories

Chapter 2 promised a distinction;
here it pays off. `jj log` shows the
history of the *work* — changes and
their parents. The **operation log**
shows the history of the *tool*:
every operation performed on this
repository, newest first, each with
an ID, the user and machine, how
long ago, and the literal arguments
of the command that caused it:

```text
$ jj op log --limit 3
@  31a5bf25b872 lab@lab-machine default@ now, lasted 53 milliseconds
│  describe commit 39f0eac81ccda100d86b28c7b6d412da77894074
│  args: jj describe -m 'A description we will regret'
○  0a75cbbf387d lab@lab-machine default@ 6 seconds ago, lasted 25 milliseconds
│  point bookmark picnic-plan to commit 39f0eac81ccda100d86b28c7b6d412da77894074
│  args: jj bookmark set picnic-plan -r @
○  2537543b56ee lab@lab-machine default@ 6 seconds ago, lasted 47 milliseconds
│  snapshot working copy
│  args: jj bookmark set picnic-plan -r @
```

Three details are worth seeing.
Even the *snapshot* is an operation
(the third entry — taken on behalf
of the bookmark command). Operations
are small and numerous; `--limit`
and the plain `jj op log` default
keep them readable. And the log is
an audit trail as much as a safety
net: the `args:` lines say exactly
what was typed, which is how you
reconstruct a confusing afternoon.

## jj undo

The lab had just set a description
it did not mean: "A description we
will regret". One command:

```text
$ jj undo
Undid operation: 31a5bf25b872 (2026-10-28 17:55:48) describe commit 39f0eac81ccda100d86b28c7b6d412da77894074
Restored to operation: 0a75cbbf387d (2026-10-28 17:55:42) point bookmark picnic-plan to commit 39f0eac81ccda100d86b28c7b6d412da77894074
Working copy  (@) now at: nvqmkrsm 39f0eac8 picnic-plan | Blanket and games
```

The repository returns to the state
recorded by the previous operation:
the description is "Blanket and
games" again, the bookmark is where
the bookmark operation left it, and
the working copy is re-snapshotted
to match. Undo is itself an
operation — it appears in the log —
and running `jj undo` a second time
would step back one operation
further, not redo. (Redo exists as
part of the operation commands;
like `jj op restore`, which jumps
the repository to any chosen
operation, it is described from the
documentation — the lab's verified
ground is `jj undo` and
`jj op log`.)

## What undo cannot reach

Undo governs the repository's
record, not the world. It does not
unsend a push (chapter 11's remote
keeps what it received until you
push a correction). It does not
restore a file you edited *and never
let Jujutsu see* — though in
practice the next command's snapshot
makes that window vanishingly
small. And it does not replace
backups: the operation log lives in
`.jj`, on the same disk as
everything else it protects.

Within those bounds it changes the
emotional arithmetic of the whole
tool. The reason rewriting history
(chapter 7) and stepping into old
changes (chapter 6) feel calm is
standing in this chapter: every one
of those operations can be walked
back, singly, exactly.

The session is transcript
`jujutsu-lab/transcripts/t07-undo.txt`.

## Check

`jj op log` lists your recent
operations with the commands that
caused them; after a change you
regret, `jj undo` restores the prior
state and says which operation it
restored to.
