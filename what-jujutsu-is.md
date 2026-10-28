# What Jujutsu Is

A programmer keeps versions of their
work so that yesterday's good state is
never truly lost. **Version control**
is the discipline of saving snapshots
of a folder — each snapshot called a
**commit** — with a note about why,
so any earlier state can be recovered,
compared, or built on. **Git** is the
version control system nearly everyone
uses, and its storage is excellent:
fast, compact, and understood by every
hosting service and tool on earth.

Git's *interface*, though, asks a lot
of the person driving it. To save
work you first **stage** it — pick
which changed files, or which parts of
files, go into the next commit — then
commit with a message written before
you can see how the story ends.
Changing your mind later ("that fix
belongs in the earlier commit") means
**rewriting history**, which Git makes
possible but treats as an advanced and
slightly suspect operation. And there
is no undo for Git itself: a command
that garbles your branches garbles
them until you unpick it by hand.

**Jujutsu is a version control system
that keeps Git's storage and replaces
the interface.** The command is `jj`.
Underneath, a Jujutsu repository is a
Git repository — the same commits, the
same object store, readable by Git
itself. On top, nearly every friction
above is designed away:

- **There is no staging area.** Your
  working folder is itself a change
  (Jujutsu says **change** where Git
  says commit). Every time Jujutsu
  looks at the folder — any command
  does this — it records the current
  state into that change automatically.
  Saving work is not a step. It has
  already happened.
- **You describe changes when you are
  ready.** A change can live happily
  with no description while you think;
  `jj describe` writes the message
  whenever the thought ripens, before
  or after the work.
- **Rewriting is the normal case.**
  Edit any change, even an old one with
  newer changes stacked on it, and
  Jujutsu rewrites the descendants to
  sit on your new version automatically.
  Nothing needs rebasing by hand
  because there is nothing fragile to
  protect — see the next point.
- **Every change has two names.** A
  **change ID** stays the same across
  rewrites; the **commit ID** (the Git
  hash) changes whenever the content
  does. You point at the change ID,
  and it keeps meaning the same piece
  of work however often it is polished.
- **The tool itself has undo.** Every
  operation Jujutsu performs —
  snapshot, describe, rewrite, push —
  is recorded in an **operation log**.
  `jj undo` restores the repository to
  before the last operation, bookmarks,
  descriptions and all.

## A familiar shape

If you know Git's vocabulary, the
translation is short. A commit is a
change. A branch is a **bookmark** —
a name pointing at a change — with
one behavioural difference chapter 8
explains. `git status` is `jj st`,
`git log` is `jj log`, and the place
Git calls HEAD is written `@`, "the
working-copy change". If you do not
know Git's vocabulary, so much the
better: chapters 1 and 2 build the
model from the files up, and the
glossary collects every term.

A first taste, after installing
(chapter 3). In an empty folder:

```bash
jj git init --colocate
echo "bread" > list.txt
jj st
```

No `add`, no `commit` — yet `jj st`
already reports the new file as part
of your working-copy change, because
looking was enough to record it:

```text
Working copy changes:
A list.txt
Working copy  (@) : ukqvskxp 9e2e18e4 (no description set)
Parent commit (@-): zzzzzzzz 00000000 (empty) (no description set)
```

That is the lab's real first session,
in transcript
`jujutsu-lab/transcripts/t02-snapshot.txt`.
The letters `ukqvskxp` are the change
ID; the hex `9e2e18e4` is the commit
ID. Chapter 6 is about why there are
two, and why the first one is the one
you will actually use.

## Who it is for

The programmer who uses Git daily and
resents the staging dance and the
interactive-rebase ceremony. The
beginner who bounced off Git's
interface and suspects — correctly —
that the ideas are simpler than the
commands make them look. Anyone who
has ever lost work to a Git command
they did not fully understand, and
would like the tool to keep a undo
button for itself.

## What it is not

Jujutsu is not a replacement for Git's
*storage* — with the Git backend, it
literally is Git's storage, and the
repository it makes can be handed to
any Git tool or hosting service.
It is not finished: version 0.46 is a
young tool whose commands still move
between releases, and chapter 14 is
honest about the rough edges. It is
not a hosting service, a code review
system, or an editor. And it does not
make the *ideas* of version control
optional: changes, parents, merges
and conflicts still exist. It just
stops charging you bookkeeping tax
on every one of them.
