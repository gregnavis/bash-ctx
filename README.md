# bash-ctx - Make Bash Context-aware

Do you want to set up project- or client-specific bash functions, aliases or
variables? You can do that easily with `bash-ctx`! Just follow the instruction
here.

## Installation

Just run

	curl -fsSL https://raw.githubusercontent.com/grn/bash-ctx/master/install | bash

Restart bash after the installation is complete.

Alternatively, copy `bash-ctx` to a directory of your choice (for instance
`/usr/libexec`) and add the following line at the end of your `.bashrc`:

	[ -r /usr/libexec/bash-ctx ] && source /usr/libexec/bash-ctx

Remember to restart your bash so it loads the script.

## Usage

Create a new context for your project:

	ctx new my-project

Add project-specific aliases by editing `~/.bash-ctx/my-project/enter`:

	alias _deploy='git push heroku master'
	alias _seed='rake db:seed'

In order to enter the newly created context run `ctx switch my-project`.

If you want to leave the current context run `ctx leave`. Issue
`ctx switch another-context` to switch to `another-context` (and leave the
current context if any).

You can also list all contexts with `ctx list`:

	ctx list
	my-context

Additionally `bash-ctx` keeps a separate command history for each context.

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
