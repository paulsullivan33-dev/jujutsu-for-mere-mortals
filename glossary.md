# Glossary

**@** — the working-copy change: the
one change whose contents are your
folder on disk. `@-` is its parent.

**abandon** — delete a change,
rebasing its children onto its
parent. Recoverable via the
operation log.

**backend** — the storage Jujutsu
keeps changes in. This guide uses
the Git backend everywhere.

**bare repository** — a Git store
with no working folder, used as a
remote or a destination.

**bookmark** — a named pointer to a
change. Git's branches are the
nearest equivalent; bookmarks differ
by never moving on their own.

**change** — Jujutsu's unit of work:
a set of file contents, a
description, an author, and parents.
Stored underneath as a Git commit.

**change ID** — the change's
permanent Jujutsu name (letters
k–z), stable across rewrites.

**colocated** — a repository whose
Git store is the `.git` folder
beside `.jj`, so both tools work in
the same folder directly.

**commit ID** — the Git hash of a
change's exact contents; it changes
whenever anything about the change
does.

**conflict** — a change's stored
disagreement between versions of a
file, kept as data inside the change
until someone edits it away.

**describe** — set or edit a
change's description, at any time.

**export** — writing Jujutsu's
bookmarks into the Git store as
branches, so Git can see them.
Automatic in colocated repositories.

**operation log** — Jujutsu's record
of its own actions on a repository;
the basis of undo.

**parent** — a change another change
is built on. Two parents make a
merge.

**rebase** — moving a change to sit
on a different parent. Jujutsu does
it automatically whenever an
ancestor is rewritten.

**remote** — a named second copy of
the Git store, reached with
`jj git fetch` and `jj git push`.

**revset** — Jujutsu's query language
for naming sets of changes, of which
this guide used only simple names
and IDs.

**root** — the permanent empty
change, ID `zzzzzzzz`, anchoring
every repository's graph.

**snapshot** — Jujutsu's automatic
recording of the working folder into
the `@` change, done before every
command runs.

**squash** — folding one change's
contents into another, usually its
parent.

**staging** — Git's intermediate
step of choosing what enters the
next commit. Jujutsu has no staging
area; the word appears in this guide
mostly to say so.

**template** — a small format string
that tells commands like `jj log`
what to print per change.

**tracked** — a remote bookmark
Jujutsu keeps in step with its local
namesake when only the remote side
has moved.

**undo** — step the whole repository
back to before the last operation.
