# Changes and Their Two IDs

By the end of this chapter you can
start a fresh change on top of
finished work, step back into an
older change to extend it, and
explain the two IDs every change
carries — including the party trick
that defines Jujutsu: rewrite a
change, and everything built on it
follows.

## jj new: a fresh page

The packing list is described and
done. Start the next piece of work:

```text
$ jj new -m "Drinks and cups"
Working copy  (@) now at: qnzlmsnr 1e9beeb8 (empty) Drinks and cups
Parent commit (@-)      : ukqvskxp 89bc972b Packing list for the picnic
```

`jj new` creates a new empty change
whose parent is the current one, and
moves `@` onto it. The packing-list
change is now closed beneath you —
snapshot still applies, but to the
*new* change. Add `drinks.txt`
(`lemonade`, `paper cups`) and the
graph has grown a storey:

```text
$ jj log
@  qnzlmsnr lab@example.com 2026-10-28 17:55:20 998c4386
│  Drinks and cups
○  ukqvskxp lab@example.com 2026-10-28 17:55:19 89bc972b
│  Packing list for the picnic
◆  zzzzzzzz root() 00000000
```

The ○ marks changes that exist but
are not the one your folder is
editing. `@` is the only change whose
contents match your folder.

## The two IDs, side by side

Ask for both IDs in full, for every
change, with a **template** — a small
format string Jujutsu fills in per
change:

```text
$ jj log --no-graph -T 'change_id.short() ++ "  " ++ commit_id.short() ++ "  " ++ description.first_line() ++ "\n"'
qnzlmsnrqyzo  998c4386b195  Drinks and cups
ukqvskxpmknl  89bc972b934e  Packing list for the picnic
zzzzzzzzzzzz  000000000000
```

- The **commit ID** (right column) is
  Git's hash of the change's contents:
  files, description, author, parents.
  Change *anything* — one letter of
  one file — and it is a different
  number. You have already watched
  `ukqvskxp`'s commit ID change three
  times in the chapters so far while the change
  itself stayed the same piece of
  work.
- The **change ID** (left column) is
  Jujutsu's own name for the piece of
  work, minted when the change is
  created and carried through every
  rewrite. It is a string of letters
  from the back half of the alphabet
  (k–z), which is deliberate: it can
  never be confused with a hex hash.

In daily use, short prefixes are
enough, and every command that takes
a change accepts the change ID.
`jj log` prints the shortest unique
prefix of each; when two IDs share
letters, the display lengthens the
ambiguous ones (the full string above
shows the first twelve characters;
`jj log` showed eight, because eight
were unique in that small graph).

## jj edit: stepping into an older change

Now the payoff. The packing list
needs `napkins`, and it belongs in
the packing-list change — not in a
new "add napkins" change on top.
In Git this is where the ceremony
starts. Here:

```text
$ jj edit ukqvskxp
Working copy  (@) now at: ukqvskxp 89bc972b Packing list for the picnic
Parent commit (@-)      : zzzzzzzz 00000000 (empty) (no description set)
Added 0 files, modified 0 files, removed 1 files
```

Your folder now holds the packing
list change's files: `drinks.txt`
disappears (it belongs to the child),
because the folder always mirrors
`@`. Add `napkins` to `list.txt` and
look at the graph:

```text
$ jj st
Rebased 1 descendant commits onto updated working copy.
Working copy changes:
A list.txt
Working copy  (@) : ukqvskxp e81e850b Packing list for the picnic

$ jj log
○  qnzlmsnr lab@example.com 2026-10-28 17:55:24 4a29b132
│  Drinks and cups
@  ukqvskxp lab@example.com 2026-10-28 17:55:24 e81e850b
│  Packing list for the picnic
◆  zzzzzzzz root() 00000000
```

Read what happened, because it is the
heart of the tool. Editing the parent
gave it a new commit ID (`e81e850b`).
The child change — Drinks and cups —
was **automatically rebased** onto
the new parent: same change ID
(`qnzlmsnr`), new commit ID
(`4a29b132`), contents unchanged, now
containing the napkins version of the
list beneath it. No command asked
for this and none was needed: in
Jujutsu, descendants always sit on
the current version of their
ancestors.

Chapter 7 uses this machinery on
purpose, harder. The sessions in
this chapter are transcripts
`t03-describe-new.txt` and
`t04-ids-and-edit.txt`.

## Check

Your graph has two described changes;
the change IDs in your `jj log`
today are the ones it will show
tomorrow, however much you polish
the contents, and `jj edit` moves
your folder — and its snapshot —
between them.
