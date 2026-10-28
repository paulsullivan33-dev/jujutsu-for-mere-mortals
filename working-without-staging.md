# Working Without Staging

By the end of this chapter the
repository has its first file, saved
three times as it grows, and you have
not run anything resembling an "add"
or a "commit" — because in Jujutsu
there is nothing to run. This is the
chapter the whole tool is built
around.

## The snapshot

Create `list.txt` in the picnic
folder with any editor, holding three
lines: `bread`, `cheese`, `apples`.
Then simply look:

```text
$ jj st
Working copy changes:
A list.txt
Working copy  (@) : ukqvskxp 9e2e18e4 (no description set)
Parent commit (@-): zzzzzzzz 00000000 (empty) (no description set)
```

Before printing that, Jujutsu
**snapshotted** the working copy: it
scanned the folder, found a file it
had never seen, and recorded it into
the `@` change. `A list.txt` is the
report — A for added. The commit ID
has moved (it was `0e16a74f` when the
change was empty) and the change ID
has not. Hold that observation for
chapter 6.

Every `jj` command snapshots first —
`jj st`, `jj log`, `jj diff`, all of
them — so the recorded state is
never staler than your last command,
whatever tool edited the files in
between.

## Seeing what the change holds

`jj diff` shows the change's contents
as a diff against its parent:

```text
$ jj diff
Added regular file list.txt:
        1: bread
        2: cheese
        3: apples
```

Edit the file again — the lab added
`grapes` — and look once more:

```text
$ jj st
Working copy changes:
A list.txt
Working copy  (@) : ukqvskxp e24b9e0a (no description set)
Parent commit (@-): zzzzzzzz 00000000 (empty) (no description set)
```

Same change, new contents, newer
commit ID, same change ID. There is
no moment at which the grapes version
was "unsaved": the first command to
run after the edit recorded it. In
Git terms you have been committing
continuously since the file existed;
Jujutsu just declines to make a
ceremony of it.

## Describing, when ready

The change is still `(no description
set)`. In Git you would have had to
write the message before the work
existed. Here, write it now that the
work explains itself:

```text
$ jj describe -m "Packing list for the picnic"
Working copy  (@) now at: ukqvskxp 89bc972b Packing list for the picnic
Parent commit (@-)      : zzzzzzzz 00000000 (empty) (no description set)
```

`jj describe` without `-m` opens
your editor on the current
description — empty, this time —
which is the way to write longer
messages. With `-m` it takes the
text from the command line. Either
way the description is a property of
the change, editable at any time,
exactly like its file contents.
Chapter 7 edits this one again,
much later, to make a point.

## What "saved" means now

Three habits from other tools need
retiring:

- *Save early, commit eventually*
  becomes simply *save* — the
  snapshot does the rest.
- *Check status before committing*
  becomes unnecessary worry: status
  is a report, not a gate.
- *Fear of the half-finished state*
  fades: a change with no description
  and rough contents is a normal,
  private, fully recorded state.
  Nobody else can see it until you
  push it (chapter 11), and chapter
  12 has a recipe for keeping scratch
  work un-pushable by accident.

The complete session — file created,
edited, described — is transcript
`jujutsu-lab/transcripts/t02-snapshot.txt`
and the describe step is in
`t03-describe-new.txt`.

## Check

Your `jj st` shows `A list.txt` and,
after `jj describe`, the change
carries your description in both
`jj st` and `jj log`. Nothing was
staged, because there is no stage.
