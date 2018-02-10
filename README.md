# bash-ctx - Make Bash Context-aware

Do you want to set up project- or client-specific bash functions, aliases or
variables? You can do that easily with `bash-ctx`! Just follow the instruction
here.

## Installation

Download and extract https://github.com/grn/bash-ctx/archive/master.zip and run
`install`:

```
$ wget https://github.com/grn/bash-ctx/archive/master.zip
$ unzip master.zip
$ cd bash-ctx-master
$ ./install
```

Remember to load `bash-ctx` from `.bashrc` by adding:

```bash
if [ -r "${HOME}/.bash-ctx/bash-ctx" ]; then
  source "${HOME}/.bash-ctx/bash-ctx"
fi
```

## Usage

Create a new context for your project:

```
ctx new my-project
```

Edit the newly created context file with:

```
ctx edit my-project
```

You can add project specific aliases (e.g. `alias .deploy='git push heroku'`),
set environment variables, change directories, and so on. In order to start
working in a given context run

```
ctx enter my-project
```

This will launch a new `bash` shell and run the context script you created
above. When you're done you can just quit this shell and return to the parent
shell (where you called `ctx enter`).

You can also list all contexts with `ctx list`:

```
ctx list
my-context
```

## What You Can Do

Upon entering a context you can:

* set project-specific environment variables (e.g. `PATH` or the prompt)
* set project- or client-specific aliases and functions
* activate a Python virtual environment
* use an rbenv gem set
* go to the project directory
* start some services in the background (e.g. Redis, Guard)
* start a `screen` or `tmux` session

## Bugs, Feature and Enhancement Requests

I'd love to hear your feedback! If something doesn't work, or should work
differently, just file an issue at https://github.com/grn/bash-ctx/issues/new.
