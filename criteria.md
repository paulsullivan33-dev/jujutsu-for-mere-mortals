# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's picnic exists only in the lab.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 14 names what the tool
   cannot do: its pre-1.0 churn, snapshot edges, the places
   Git shows through (signed commits, submodules), conflicts
   that still need thinking, and the social cost of easy
   rewriting on shared history.
8. **Safety nets named before sharp tools.** The operation
   log and undo are introduced as concepts in chapter 2 and
   given their own chapter (10) before the guide leans on
   them; every destructive-feeling operation in the text —
   abandon, squash, the sideways push — is paired in its own
   chapter with the way back.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, 7.7 GB Ubuntu 24.04 sandbox.
Jujutsu **0.46.0** was the official linux-x86_64 musl release
binary from the project's GitHub releases, run unmodified
alongside Git 2.43.0. The lab worked under a scratch account
("Lab User <lab@example.com>") so no real identity appears
anywhere. All transcripts live in `jujutsu-lab/transcripts/`,
numbered in the order the chapters use them.

Run for real, with transcripts as evidence:

- Install, version, and user configuration read back with
  `jj config list` (t00).
- `jj git init --colocate`, the `.git`/`.jj` layout, `jj log`
  and `jj st` on the empty repository, and plain Git's
  "no commits yet" answer in the same folder (t01). Plain
  `jj git init` produced the identical colocated layout,
  confirming colocation is the current default (t15).
- The automatic snapshot: a file's creation and later edit
  recorded by `jj st` with no add step, the commit ID moving
  while the change ID held still (t02).
- `jj describe`, `jj new`, and a two-storey graph (t03);
  both IDs read in full with a template, `jj edit` into an
  ancestor, and the descendant's automatic rebase (t04).
- A bookmark created, a move refused as sideways when `@`
  had been built on the wrong parent, the sideways change
  abandoned, the bookmark moved forward properly, and the
  `*` marking an unexported position (t05, t06).
- `jj undo` reversing a description, with `jj op log`
  showing the operation, its arguments, and even the
  snapshot operations around it (t07).
- A real two-sided merge conflict: two children editing the
  same line differently, the merge change storing the
  conflict, the conflict read in the file with Jujutsu's
  diff-style markers, and resolution by ordinary editing,
  confirmed by the warning disappearing (t08, t09, t10).
- The full Git conversation against a bare repository on
  the same disk: `jj git remote add`, `jj git push` of the
  bookmark, the remote's history read back with plain
  `git log` (t11); a second clone where a "collaborator"
  used plain Git alone to commit and push, then
  `jj git fetch` importing it and tracking the bookmark
  forward, with `git log` now working in the colocated
  folder (t12); a squash, and the rewritten change pushed
  again as a sideways move (t13, t14).
- `--git-repo` storing the Git repository elsewhere: the
  refusal when the target did not exist, and success after
  creating it with `git init --bare` first (t16).
- Odds and ends the troubleshooting chapter relies on:
  `jj root`, `jj git export` answering "No export needed in
  colocated workspaces", and a `.gitignore` rule hiding a
  file from `jj st` and then, removed, revealing it (t17,
  t18).

Documented only — described from the project's documentation,
not run, and the text does not claim otherwise:

- Installing through package managers, and the macOS and
  Windows routes.
- Adopting Jujutsu into a pre-existing Git project with
  `jj git init --colocate` run inside it (the lab always
  started from empty folders).
- `jj split` and `jj rebase` (squash, abandon and the
  automatic descendant rebase were run); `jj op restore`
  and redo (undo itself was run).
- The diverged-bookmark case, where both sides moved a
  bookmark: the lab's fetch met only the clean
  track-forward case.
- Hosted remotes over HTTPS/SSH, the experimental native
  backend, signed commits, and any repository larger than
  the lab's picnic.
- The comparison tools in chapter 13 are characterised,
  not benchmarked.

Fixtures are this guide's own and deliberately generic: a
picnic packing list, a drinks list, a blanket, and a
cheddar-versus-fruit-salad disagreement. The lab's one
"collaborator" was the same scratch account driving plain
Git in a second clone. No example is modelled on any real
person, project or organisation.
