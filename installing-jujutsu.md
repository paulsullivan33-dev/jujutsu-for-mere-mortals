# Installing Jujutsu

By the end of this chapter `jj` runs
on your machine, knows your name, and
can prove both. Jujutsu is a single
self-contained program — one file, no
runtime to install first — published
for Linux, macOS and Windows by the
project on its GitHub releases page,
and also carried by most package
managers. This chapter's lab used the
release file directly, which is the
route shown here; package-manager
routes are described from the
project's install page and labelled
as documented-only in `criteria.md`.

## Get the program

Download the archive for your system
from the Jujutsu releases page —
on this lab's 64-bit Linux machine,
`jj-v0.46.0-x86_64-unknown-linux-musl.tar.gz`
— unpack it, and put the `jj` file
somewhere on your **PATH** (the list
of folders your shell searches for
commands): `~/bin` or `/usr/local/bin`
are the usual choices. There is
nothing else inside the archive but a
license, a readme, and the program.

Check it answers:

```text
$ jj --version
jj 0.46.0-7d382314f9420b4b8d932245b49c99492c812b9f
```

The long tail after the version is
the exact build; for this guide,
**0.46.0** is the number that matters.
Jujutsu is young and moves quickly,
so if your version is much newer than
the lab's, expect small differences
in output wording — the ideas in this
guide have been stable across many
releases, but exact strings have not.

## Tell it your name

Version control attaches a name and
an email address to every change.
Set yours once:

```bash
jj config set --user user.name "Your Name"
jj config set --user user.email "you@example.com"
```

`--user` writes to your personal
config file (`~/.config/jj/config.toml`
on Linux), so every repository you
touch picks it up. If you already use
Git, note that Jujutsu does *not*
simply borrow Git's settings for
this: set them here too, or your
changes will be authored by a
placeholder and the tool will warn
you about it.

Read the settings back to prove they
landed — the lab's own check, with
its stand-in identity:

```text
$ jj config list
user.name = "Lab User"
user.email = "lab@example.com"
operation.hostname = "lab-machine"
operation.username = "lab"
```

(The last two lines record which
machine and account each operation
came from; they are written into the
operation log of chapter 10 and are
worth setting on machines you share.)

## The editor question, now

Several commands — writing a
description without `-m`, the
interactive half of `jj squash` —
open a text editor. Jujutsu chooses
one from your configuration, then the
standard `EDITOR` and `VISUAL`
environment variables, then Git's
settings. If none of those names a
program that exists, the command
fails *at the moment it needs the
editor*, which is how this guide's
lab met its first real error
(chapter 7 tells the story). Avoid
the surprise: check now which editor
the machine offers, and if you have a
preference, record it:

```bash
jj config set --user ui.editor "nano"
```

Put your editor's command in place of
`nano` — and make sure it is one that
is actually installed.

## Check

`jj --version` prints 0.46.x or a
later version, `jj config list`
shows your real name and email, and
you know which editor Jujutsu will
open. Chapter 4 makes a repository.
