# Limits

An honest accounting of where
Jujutsu 0.46 stops, drawn from the
lab where the lab reached the edge,
and from the project's documentation
where it did not.

## A young tool

Jujutsu is pre-1.0 and behaves like
it: commands are renamed, flags
change, and output wording shifts
between releases. Two examples live
in this guide's own lab notes.
Colocation became the default for
`jj git init` only recently — older
tutorials send you to a layout the
current tool does not make, and
advice written for an older release
should be checked against the one
you run. Pin a version per team,
upgrade deliberately, and expect to
re-read release notes.

## The working copy has edges

The automatic snapshot is keyed to
commands: it happens when a `jj`
command runs, not the instant a
file changes. A file edited and a
machine crashed before any command
runs can lose the snapshot's copy —
though the file itself is as safe
as it ever was, which is the part
that matters. Very large working
trees also make each snapshot
proportionately slower; snapshotting
is a scan, not a watcher.

## Scale and shape of history

The lab's graph had a dozen changes.
Repositories with tens of thousands
of commits work — Jujutsu is used on
very large Git repositories — but
operations that touch the whole
graph are priced accordingly, and
this guide's "everything is instant"
experience is a small-repository
experience. Equally, the tool's
comfort zone is a graph you can
hold in your head; it draws the
graph beautifully and does not
pretend a five-hundred-change stack
is a plan.

## Where Git shows through

The Git backend is a strength with
a seam. Git features with no
Jujutsu counterpart — signed
commits are the commonly met one —
range from awkward to unavailable
depending on release. Submodules
are not supported in Jujutsu's
working copy. Partial clones and
some exotic ref arrangements can
surprise it. And the export dance
of chapters 4 and 11 is real: Git
tools in a colocated folder only
see what has been exported, which
confuses everyone once.

## Conflicts are calm, not solved

Chapter 9's stored conflicts are a
genuine improvement in machinery.
They do not reduce the *thinking* a
real conflict needs, and their
diff-style presentation, while
informative, is one more format to
learn to read. A team adopting
Jujutsu should expect to teach the
markers just as it once taught
Git's.

## Collaboration has a social layer

Jujutsu makes rewriting — and
therefore sideways pushes of shared
bookmarks — easy. Easy is not the
same as welcome: rewriting a branch
three teammates have based work on
will cost them exactly what a force
push costs in Git, plus their
unfamiliarity with the rescue. The
tool's model is at its best for
history that is still yours; shared
history wants the usual courtesies
and agreements, tool or no tool.

## Not yet, or not this tool

The native (non-Git) backend is
experimental. Some Git hosting
workflows — pull requests and their
review state — live outside any
local tool's reach and still belong
to the services' own interfaces.
Graphical tooling for Jujutsu
itself is young beside Git's
thirty years of accumulated
interfaces. None of this is hidden
by the project, and none of it
stops the daily use this guide
demonstrates; it is the price of
adopting a tool whose best years
are visibly in front of it.
