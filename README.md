# Jujutsu for Mere Mortals

**Git** keeps the history of your files:
every version you have ever saved, who
saved it, and why. Working with Git,
though, means working as its bookkeeper
first — choose which changes to *stage*,
word a message before you know whether
the change is any good, and treat
rewriting history as a job for experts
with steady hands. **Jujutsu** — the
command is `jj` — is a newer **version
control system** that keeps Git's storage
and throws away the bookkeeping. There
is no staging area. Your working folder
*is* a change, recorded automatically
every time you look. Rewriting history
is the ordinary way of working, not a
dangerous exception, and the tool itself
has an undo button.

This guide explains how that works. It
builds one small repository — the plans
for a picnic — and does everything to
it: writes files without ever staging
them, renames and reshapes changes,
creates a bookmark and moves it, starts
an argument between two versions of a
shopping list and settles it, pushes the
result to a plain Git repository, takes
in a collaborator's Git commit, and
undoes a mistake with `jj undo`. Every
command and every output in these pages
was run against the real tool in the
lab, and the transcripts are kept in
this repository as the evidence.

The lab behind this guide ran
**Jujutsu 0.46.0**, the official release
binary, on a small Linux machine. The
repository was *colocated* — Jujutsu
and Git looking at the same `.git`
folder, so plain Git commands worked
in the same directory throughout. The
"remote" the lab pushed to was a bare
Git repository on the same disk; no
account, no hosting service, and no
network were needed to prove the whole
Git conversation, including a second
clone in which a "collaborator" worked
with plain Git alone.

What the lab could not do, it did not
fake. Talking to real hosting services
(GitHub, GitLab and friends), working
in repositories far larger than a
picnic list, the non-Git storage
backends Jujutsu is experimenting with,
and large-team workflows are taught
from the documentation and labelled as
such wherever they appear. `criteria.md`
keeps the full ledger of what was run
and what was only read.

## What this guide covers

- [**What Jujutsu is**](what-jujutsu-is.md) —
  the mental model: Git's storage, a
  calmer front end, and the working
  copy that is always a change
- [**The moving parts**](the-moving-parts.md) —
  changes, bookmarks, the operation
  log, and where the Git repository
  actually lives
- [**Installing Jujutsu**](installing-jujutsu.md) —
  the release binary, your name and
  email, and the version check
- [**Your first repository**](your-first-repository.md) —
  `jj git init` a colocated repo, and
  reading `jj log` and `jj st` before
  anything is written
- [**Working without staging**](working-without-staging.md) —
  the automatic snapshot: edit a file
  and it is already recorded; `jj diff`
  and describing a change after the fact
- [**Changes and their two IDs**](changes-and-their-two-ids.md) —
  change IDs that survive rewriting
  and commit IDs that don't, `jj new`,
  and `jj edit` stepping into an
  older change
- [**Rewriting history safely**](rewriting-history-safely.md) —
  editing an ancestor and watching
  descendants follow, and folding a
  fix into its parent with `jj squash`
- [**Bookmarks**](bookmarks.md) —
  Jujutsu's named pointers, how they
  differ from Git branches, and the
  sideways move Jujutsu refuses
- [**Conflicts without the sirens**](conflicts.md) —
  a conflict stored as ordinary data,
  read in the file itself, and settled
  by editing like any other text
- [**Undo and the operation log**](undo-and-the-operation-log.md) —
  every operation recorded, and
  `jj undo` walking the repository
  back to before a change of heart
- [**Working with Git**](working-with-git.md) —
  remotes, `jj git push` and
  `jj git fetch` against a bare local
  repository, and a collaborator who
  only has Git
- [**Recipes**](recipes.md) — start
  over mid-thought, split a muddled
  change in two, keep a private
  scratch change nobody pushes,
  recover a change you abandoned
- [**The neighbours**](the-neighbours.md) —
  Git itself, lazygit and the other
  Git interfaces, Mercurial: what each
  does that Jujutsu doesn't, and
  vice versa
- [**Limits**](limits.md) — a young
  tool with sharp edges: large
  repositories, partial tooling, and
  the places Git still shows through
- [**Troubleshooting**](troubleshooting.md) —
  the empty `git log`, the bookmark
  that would not move sideways, the
  missing text editor, and the traps
  this guide's lab fell into
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Replacing Git on a whole team, or
migrating an organisation's
repositories: this guide stays with
one person learning the tool on their
own work, where Jujutsu shines.
Jujutsu's experimental native backend
(a store that is not Git at all) is
pointed at, not taught — everything
here uses the Git backend, which is
the supported road today. Scripting
Jujutsu with its template language is
shown once, for reading IDs, and not
developed into a chapter; the
documentation carries that further.

## How to use this guide

Read chapter 1 for the model and
chapter 2 before installing anything —
knowing which part holds what turns
later confusion into a named problem.
Chapters 4 to 6 are one sitting:
create the repository, write in it,
and watch Jujutsu record your work
without being asked. Chapter 7 is
where the tool earns its keep —
rewrite something and notice that
nothing breaks. Chapter 9 repays
slow reading: a conflict in Jujutsu
looks alarming and is not, and
understanding why changes how you
feel about conflicts everywhere.
The lab's transcripts live in
`jujutsu-lab/transcripts/`, numbered
in the order the chapters use them.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Git for Mere Mortals](https://paulsullivan33-dev.github.io/git-for-mere-mortals/)
- [Git Rescue for Mere Mortals](https://paulsullivan33-dev.github.io/git-rescue-for-mere-mortals/)
- [lazygit for Mere Mortals](https://paulsullivan33-dev.github.io/lazygit-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/jujutsu-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/jujutsu-for-mere-mortals/blob/main/LICENSE-CODE).
