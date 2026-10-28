# Conflicts Without the Sirens

By the end of this chapter two
versions of the shopping list have
disagreed about line 2, the
disagreement has been stored as an
ordinary part of a change, read in
the file itself, and settled by
editing — with no merge state to be
stuck in, and no command you had to
finish before doing anything else.

## How the argument starts

From the packing-list change, the
lab grew two children. One changed
line 2 of `list.txt` from `cheese`
to `cheddar`; the other, branching
from the same parent, changed it to
`fruit salad`. Each is unremarkable
alone. Then a merge change was made
with both as parents:

```text
$ jj new pnrqtzul @ -m "Merge the two menu ideas"
Working copy  (@) now at: qslkzuwo 03ee13b7 (conflict) (empty) Merge the two menu ideas
Parent commit (@-)      : pnrqtzul bca9c18a Cheddar instead of cheese
Parent commit (@-)      : stppusnt f1c52342 Fruit salad instead
Added 0 files, modified 1 files, removed 0 files
Warning: There are unresolved conflicts at these paths:
list.txt    2-sided conflict
```

In Git, this moment starts a special
*merge state*: the repository is
half-locked until you resolve,
abort, or finish. In Jujutsu,
nothing of the sort has happened.
The merge change simply *contains* a
conflict as part of its content —
the same way another change might
contain a half-written paragraph.
`(conflict)` rides along in
`jj log` and `jj st` as a label on
the change; the warning names the
files involved. You may leave the
change, work elsewhere for a week,
push it, even share it — the
conflict is data, and it waits
patiently instead of blocking the
door.

## Reading a conflict

The file in your folder shows the
conflict in Jujutsu's own format:

```text
bread
<<<<<<< conflict 1 of 1
%%%%%%% diff from: ukqvskxp e81e850b "Packing list for the picnic"
\\\\\\\        to: pnrqtzul bca9c18a "Cheddar instead of cheese"
-cheese
+cheddar
+++++++ stppusnt f1c52342 "Fruit salad instead"
fruit salad
>>>>>>> conflict 1 of 1 ends
apples
grapes
napkins
```

Read it as a small report, not as
damage. The `%%%%%%%` section is a
*diff*: starting from the common
ancestor (named in full — the
packing-list change), one side
removed `cheese` and added
`cheddar`. The `+++++++` section
shows the other side's version
whole: `fruit salad`. Between the
`<<<<<<<` and `>>>>>>>` lines,
Jujutsu has told you exactly what
each side did, with the change IDs
attached, so the report doubles as
a reminder of where each version
came from. (Git's conflict markers,
by contrast, show the two outcomes
but not the path from the ancestor
— one reason Jujutsu's are longer,
and easier to settle correctly.)

## Settling it

Resolving is not a command. Open
the file, replace the whole marked
block with the settled text — the
lab kept both additions — and save:

```text
bread
cheddar
fruit salad
apples
grapes
napkins
```

The next command snapshots, and the
snapshot finds a file with no
conflict in it:

```text
$ jj st
Working copy changes:
M list.txt
Working copy  (@) : qslkzuwo 09a12794 Merge the two menu ideas
Parent commit (@-): pnrqtzul bca9c18a Cheddar instead of cheese
Parent commit (@-): stppusnt f1c52342 Fruit salad instead
```

The warning is gone, `(conflict)`
leaves the change's label, and the
resolution is simply the merge
change's new contents. If the
resolution is wrong, it is wrong the
way any edit is wrong: correctable
by editing again, or by `jj undo`.

One consequence deserves stating
plainly. Because a conflict can sit
inside a change indefinitely,
Jujutsu will also happily build
*further* changes on top of a
conflicted one, and the conflict
will still be there when you come
back. The calm is real, but so is
the responsibility: `jj st`'s warning
is the whole alarm system. Read it.

The conflict, its reading and its
settlement are transcripts
`t08-conflict-a.txt`,
`t09-conflict-b.txt` and
`t10-conflict-resolved.txt`.

## Check

Your merge change exists; while the
conflict was open, `jj st` named the
file and the file showed both
sides' edits with their sources;
after your edit, the warning is
gone and the settled text is what
`jj diff` reports.
