# The Neighbours

Jujutsu lives in a crowded street.
These are the tools a reader is most
likely to be choosing between, or
using alongside. The characterisations
here are judgement, not benchmark;
`criteria.md` says which parts of
the comparison the lab itself
demonstrated.

## Git itself

Git is the store, the network, and
the lingua franca. It is also the
interface Jujutsu replaces: staging,
the commit-message-before-the-work,
interactive rebase as a specialist
skill, no undo for the tool. You do
not choose between Git and Jujutsu
the way you choose between editors —
a Jujutsu repository *is* a Git
repository, your teammates can stay
on Git forever, and chapter 11's
collaborator did exactly that without
knowing Jujutsu existed. The honest
choice is per-person: which front
end do *you* want to drive the store
with? *Git for Mere Mortals* teaches
the store through its own commands.

## lazygit and the Git interfaces

lazygit, GitKraken, the GitHub
desktop app and editor integrations
keep Git's model and give it a
friendlier face. If your pain is
"Git's commands are hard to
remember", a visual interface may
be the whole cure. If your pain is
the model itself — staging, stranded
rebases, no undo — the face is not
where the cure lives. In a colocated
repository the two compose: drive
with `jj`, and open lazygit in the
same folder when you want its view
of the graph. *lazygit for Mere
Mortals* is the companion guide.

## Mercurial

Jujutsu's design owes Mercurial a
visible debt: changesets with stable
identities, a willingness to rewrite
history, an operation log's cousin
in Mercurial's own undo. Mercurial
is the mature, finished version of
several of these ideas, with its own
storage and a smaller present-day
ecosystem. A reader choosing fresh
today will usually pick Jujutsu for
the Git storage and the hosting
world that comes with it; a reader
on Mercurial already has much of
what this guide celebrates.

## The hosting services' tools

`gh`, the GitHub CLI, and its
counterparts are complements, not
rivals: they operate on pull
requests, issues and releases, which
live on the service, not in your
repository. Jujutsu changes how the
commits are made; what the service
does with them afterwards is
unchanged, and your bookmark named
`picnic-plan` arrives there as a
branch named `picnic-plan`, ready
for the usual pull-request ceremony.

## When the neighbour is the answer

Stay with plain Git where a team's
shared scripts and habits are built
on its exact behaviour, or where
Jujutsu's youth (chapter 14) is a
real cost — an air-gapped network
with no route to updated binaries,
for instance. Jujutsu's pitch was
never that Git is broken. It is that
the daily bookkeeping tax is
optional, and chapter by chapter,
this guide has shown the receipts.
