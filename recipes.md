# Recipes

Short answers to recurring
situations. Every ingredient here
was run in the lab except where the
recipe says otherwise; the limits
chapter (next door at 14) keeps the
documentation-only ledger.

## Start over mid-thought

You are three files into a change
and realise the idea is wrong from
the second line. Do not delete
anything: `jj new` above the change
and start fresh there, or simply
`jj abandon @` to discard the change
and return to its parent with the
folder to match. The discarded
change is an operation-log entry
away (recipe 4) if the idea turns
out to have been right.

## Split a muddled change in two

A change that fixes the list *and*
reorganises the drinks wants to be
two changes. `jj split` divides `@`
along the lines you choose,
interactively; give the first half
a description, the second keeps the
rest. (Described from the
documentation in this guide —
split's interactive prompts leave
no transcript; the shape of the
result is the mirror of chapter 7's
verified squash.)

## Keep scratch work nobody pushes

Push happens per bookmark
(chapter 11), and a change with no
bookmark on it or above it stays
home. So the arrangement that keeps
experiments private needs no
setting: keep finished work under
your bookmark, do scratch work in
unnamed changes above or beside it,
and push the bookmark. Nothing
unnamed travels.

## Recover a change you abandoned

`jj abandon` feels final; the
operation log disagrees. Run
`jj op log`, find the operation
before the abandon, and either
`jj undo` (if the abandon was the
last thing you did) or note the
abandoned change's change ID from
the log around that time and return
to it — the change's commits still
exist until garbage collection,
and `jj edit <change-id>` steps
back into it. The lab's own abandon
was deliberate and stayed
abandoned (transcript
`t06-bookmark-fixed.txt`); the
recovery path is the same mechanism
chapter 10 verified for undo,
applied on purpose.

## Find out what a change holds

`jj diff -r <change-id>` shows one
change's contents wherever it sits;
`jj show <change-id>` adds the
metadata — both IDs, author, the
bookmarks pointing at it. The lab's
`jj show picnic-plan` confirmed a
squash by reading the folded file's
four lines out of the parent change
itself.

## See the graph the way Git would

In a colocated repository, any Git
command works once refs have been
exported (a push or fetch does it):
`git log --oneline --graph
--all` inside the picnic folder
draws the same history from the
other tool's chair. Useful when a
teammate asks "but what does it
look like in Git?" — the answer is
"the same, because it is."

## Freeze a moment in the tool itself

Before a big reshaping, note the
current operation ID from
`jj op log`. If the reshaping
disappoints, `jj op restore <id>`
(described from the documentation)
or repeated `jj undo` returns the
repository to that exact state —
not the files' state, the
*repository's* state, bookmarks and
descriptions included.
