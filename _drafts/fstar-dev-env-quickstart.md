---
layout: post
title:  "How to get started with F* development - a Quick Start Guide"
categories: fstar
author: Mikko Rajakangas
---
After my latest post delving into proof-oriented
programming, I'm aware there are probably some
readers who were left wondering how to get started
with F* development. More specifically, how to set
up your development environment so you can start
diving into the interesting world of program
verification and proof-oriented programming.
<!--excerpt-->

## Installing F* ##

There are a few options given at the present time
for some of the steps in setting up the development
environment for F* development. First of all, it
is possible to install F* from a binary package
if you're developing on Linux or Windows. That
approach has some limitations, however: You won't
be able to compile generated OCaml code, unless
of course you already have Ocaml isntalled as well.
There are a few other options as well: running F*
from a docker image or using a Chocolatey package
if you are running Windows. Since I'm using both
Linux (Ubuntu Studio) and Mac OS, and I also want
to be able to compile the generated OCaml code,
I opted for the default option: using an OPAM
package.

### F* dependencies for Mac OS ###

First things first: you'll need to install OCaml
and OPAM, which is the OCaml package manager. It
is advisable to make your life simple and install
them via *Homebrew*, with the following commands:

    $ brew install ocaml
    $ brew install gpatch  // a dependecy needed by OPAM //
    $ brew install opam
    $ brew install coreutils // needed for F* installation to complete succesfully //

It is a good practice to also verify that both OCaml
and OPAM are correctly installed. Then you should be
able to proceed into installing F* itself with OPAM.

### F* dependencies for Linux (Ubuntu) ###

With Linux it is best to also use the distribution's
package manager. For your flavor the directions
can be found [here](https://ocaml.org/docs/install.html)
for OCaml and [here](https://opam.ocaml.org/doc/Install.html#Using-your-distribution-39-s-package-system)
for OPAM. If you are using a Debian based distribution,
here are the CLI commands needed:

    $ apt install ocaml
    
    // The following are Ubuntu specific and can be skipped with other Debian distributions //

    $ add-apt-repository ppa:avsm/ppa
    $ apt update

    // End of Ubuntu specific commands //

    $ apt install opam

After you've verified that both OCaml and OPAM
did indeed install correctly you can now install
F* itself with OPAM.

### Configuring OPAM ###

Once you have OCaml and OPAM installed on your
system you still need to initialize and configure
OPAM. Initializing OPAM happens simply by running
`opam init` and updating the `PATH` to `ocamlfind`
and the OCaml libraries. Unless you have a good
reason not to, I recommend allowing `opam init`
to edit your `~/bashrc.` or `~/profile.` so that
these steps get done automatically. Otherwise, use:

    $ eval $(opam config env)

Next, you need to ensure that OPAM is using an OCaml
version supported by F* (at the time of writing versions
from 4.04.0 to 4.12.X should work). You do this by typing:

    $ opam switch list

The current OCaml version can be identified by the letter C.
If it's **not** within the version range above,
do the following:

    $ opam switch list-available
    $ opam switch <version-number>

### Installing F* with OPAM ###

This one is really simple, provided all the above
steps were succesful. There are a few options, but
I do recommend installing F* via the following command
to install the latest development version of F* alongside
all of the required dependencies:

    $ opam pin add fstar --dev-repo

This way you'll get an up-to-date version of F* and don't
have to manually install *z3*.

## Editor Setup ##

Now that we have the language installed, it's time
to look at the editor options. In theory, you could
use any text editor, whatever you prefer. But if,
like me, you want syntax highlighting and all that
language level special support jazz, the options
are a bit more limited: Atom, Vim, VS Code and
Emacs. The plugins for Atom and Vim are discontinued,
so I can't recommend them, whereas the VS Code plugin
is relatively new, and lacks some of the features
the recommended Emacs has. So I decided to go with
Emacs, and as such the following sections guide you
through setting up your Emacs editor for F* development
for GNU Emacs on Ubuntu ***and*** Aquamacs on Mac.
For other Linux distributions see the [GNU Emacs website](https://www.gnu.org/software/emacs/download.html#gnu-linux) for installing instructions.

For Ubuntu/Debian, installing Emacs is simple:

    $ sudo apt-get install emacs

For F* development on Mac OS we want to install
Aquamacs (or at least that's the Emacs version
I prefer for Mac). You can download it from [here](http://aquamacs.org/download.html).

After you've got your Emacs installed you'll need the
[fstar-mode plugin](https://github.com/FStarLang/fstar-mode.el)
for F* language support.

Add the following to your init file (usually `.emacs`)
if it is not already there:

```elisp
(require 'package)
(add-to-list 'package-archives '("melpa" . "https://melpa.org/packages/") t)
(package-initialize)
```

Restart Emacs, then run `M-x package-refresh-contents`
and `M-x package-install RET fstar-mode RET`.
 
If `fstar.exe` and `z3` are not already in your
path, set the `fstar-executable` and `fstar-smt-executable`
variables:

```elisp
(setq-default fstar-executable "PATH-TO-FSTAR.EXE")
(setq-default fstar-smt-executable "PATH-TO-Z3(.EXE)")
```

Et voilà! You should have your F* development
environment now up and running! Have fun with
proof-oriented programming!