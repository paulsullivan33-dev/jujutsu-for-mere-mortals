# Working with Git

By the end of this chapter the
picnic history has been pushed to a
bare Git repository, read back with
plain Git, joined by a commit made
with Git alone, fetched back into
Jujutsu, and pushed again after a
rewrite — the full conversation
between the two tools, with no
hosting service involved.

## The remote is just a Git repository

A **remote** is another copy of the
Git store under a name, usually
`origin`. Hosting services provide
remotes; so can a folder on the same
disk. The lab made one the plain Git
way and told Jujutsu where it was:

```text
$ git init --bare remote.git
...
Initialized empty Git repository in /home/hatch/workspace/build-check/jujutsu-scratch/lab/remote.git/

$ jj git remote add origin ../remote.git
```

A **bare** repository is a Git store
with no working folder — the right
shape for something whose only job
is to receive pushes. Anything Git
can address works as the remote's
location: a local path here, an
`https://` or `ssh://` address for a
hosted service (chapter 14's limits
are honest about which of those this
guide actually ran).

## Pushing a bookmark

Jujutsu pushes bookmarks, and says
exactly what it intends before doing
it:

```text
$ jj git push --bookmark picnic-plan
Changes to push to origin:
  bookmark: picnic-plan [add to 39f0eac81ccd]
```

The bookmark became a Git branch of
the same name. Proof, with nothing
but Git reading the remote:

```text
$ git --git-dir=../remote.git log --oneline picnic-plan
39f0eac Blanket and games
4a29b13 Drinks and cups
e81e850 Packing list for the picnic
```

Those are the lab's three changes,
as commits, with their descriptions
as messages. To a Git user receiving
them, nothing marks them as having
come from Jujutsu — because at the
storage level, nothing is different.

## A collaborator who only has Git

The lab cloned the remote with
plain Git into a second folder,
edited `drinks.txt` (iced tea joins
the list), committed with plain Git
and pushed — the remote's
`picnic-plan` branch advanced. Back
in the Jujutsu repository:

```text
$ jj git fetch
bookmark: picnic-plan@origin [updated] tracked
```

Fetch does two things. It imports
new commits from the remote, and it
reconciles bookmarks: `picnic-plan`
had not moved locally since the
push, so Jujutsu **tracked** it
forward to the fetched position
automatically. The new commit
appears in `jj log` as a proper
change — commit ID `ecf19466`,
change ID `kzkzkvtk`, which Jujutsu
minted for it on arrival, since the
Git commit carried none — sitting
on top of Blanket and games, with
the bookmark on it. And in the
colocated folder, `git log --oneline
picnic-plan` now works, showing all
four commits: the earlier "no
commits yet" puzzle of chapter 4 is
over, because fetch and push have
exported the refs Git follows.

When a bookmark *has* moved locally
*and* on the remote, fetch does not
guess: both positions are reported
(`picnic-plan` and
`picnic-plan@origin` separately) and
the reconciliation — merge, rebase,
or choose — is yours. This guide
describes that case from the
documentation; the lab's verified
ground is the clean track-forward
above, which is the everyday case
for a solo worker.

## Pushing a rewrite

Finally, the promise of chapter 7
kept. After the squash folded
sparkling water into "Add iced tea",
that change's commit ID had changed:
the pushed history no longer matched
the local one. The push:

```text
$ jj git push --bookmark picnic-plan
Changes to push to origin:
  bookmark: picnic-plan [move sideways from ecf1946694b4 to 46a5cafd598a]
```

"Move sideways" is Jujutsu being
precise: the bookmark's new commit
is not a descendant of the old one —
it is a *rewrite* of it — and Git
would call the update a force push.
Jujutsu performs it as a matter of
course, because rewriting is normal
here; the remote's log afterwards
shows the new commit in place of
the old, contents folded. On a
shared branch other people build
on, that same everyday move deserves
a conversation first — the tool
makes it easy, not consequence-free,
and chapter 14 says so again.

The whole chapter is transcripts
`t11-push.txt`, `t12-fetch.txt` and
`t14-push-again.txt`.

## Check

Your remote's `git log` shows your
changes as commits; a commit pushed
by plain Git appears in your
`jj log` after `jj git fetch`; and
a rewritten change can be pushed
again, with Jujutsu naming the move
honestly before it makes it.
