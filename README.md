# bash-ctx - Make working on multiple projects a breeze!

I spend most of my time in a terminal and frequently issue commands that are
_almost_ similar. Unfortunately, _almost_ makes a big difference. Here are a
few things that can go wrong:

* You rebase a branch on top of `master` instead of `develop` because another
  project you're working on doesn't have `develop` and you were confused.
* You start the development server with wrong parameters as they're slightly
  different for every project.
* You type `bundle exec rails routes | grep <pattern>` only to discover this is
  a Rails 4 project and you should have used `rake` instead of `rails`.

`bash-ctx` helps you avoid such annoyances by helping you define
project-specific aliases and configuration variables. You can use it to:

* Start the development server with `.start` - doesn't matter whether it's
  Rails, Django, or any other framework.
* Run the tests with `.test` - again, no need to think about which tool the
  current project is using.
* Pull `master` and rebase the current branch on top of it just by calling
  `.update`.

## Installation

Download and extract https://github.com/grn/bash-ctx/archive/master.zip and run
`install`:

```
$ wget https://github.com/grn/bash-ctx/archive/master.zip
$ unzip master.zip
$ cd bash-ctx-master
$ ./install
```

This will install `bash-ctx` in `~/.bash-ctx`. Remember to load it in your
`.bashrc`:

```bash
# Load bash-ctx if available.
if [ -r "${HOME}/.bash-ctx/bash-ctx" ]; then
  source "${HOME}/.bash-ctx/bash-ctx"
fi
```

## Basic Concepts

The two essential concepts in `bash-ctx` are:

* Contexts - they represent project-specific settings and are stored in
  `~/.bash-ctx/ctx`. Each context is a file with bash commands to execute when
  entering the context.
* Libraries or libs - common pieces of functionality you can share across your
  contexts. For example, all your Rails 5 projects can just use the `rails5` lib
  instead of redefining the same functions and aliases over and over again.

You first create a context and then enter it. If you enter a context then its
name is passed in an environment variable to a newly created bash child process
(instead of just sourcing it in the calling process). Your `.bashrc` will then
source the right context file. The benefit of this approach is that you avoid
polluting your top-level shell with context-specific stuff and keep contexts
separated.

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
set environment variables, change directories, and so on.

In order to enter the context run:

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

## Example Context File

```
ctx_libs git rails5

# I often need to reset accounts but wouldn't like to reset the other tables.
function .cleanup() {
  .run 'Account.update_all status: 0'
}

cd ~/Projects/MyProject
ctx_libs tmux
```

## Libs

Libs are small shell script libraries that you can share across projects. It's
an easy way to define aliases, functions, and set environment variables.
Creating your own lib is easy. Just run `ctx lib <name>` and call `ctx_libs
<name>` in your context file to include the lib in it.

Below is a list of provided libs.

### `git`

Allowed commands:

* `.update [<remote>] [<branch>]` - stash changes, pull the specified remote
  branch (as defined in `$CTX_GIT_ORIGIN` and `$CTX_GIT_BRANCH` which default to
  `origin` and `master`) and rebase the current branch on top of it.
  default) from the specified remote and rebase the current branch on top of it.
* `.squash <n>` - squash the last `n` commits.
* `.amend` - edit the current commit message (`git commit --amend -v`).
* `.fix` - amend the current commit with the changes from the work tree
  (equivalent of `git commit -a --amend -C HEAD`).
* `.commit <message>` - equivalent of `git commit -m <message>`.

Allowed configuration variables:

* `CTX_GIT_ORIGIN` - the default origin. Defaults to `origin`.
* `CTX_GIT_BRANCH` - the default branch from origin. Defaults to `master`.

### `rails4` and `rails5`

All commands are prefixed by `bundle exec` and use `rake` (Rails 4) or `rails`
(Rails 5):

* `.migrate` - `rails db:migrate`
* `.rollback` - `rails db:rollback`
* `.seed` - `rails db:seed`
* `.reset` - `rails db:reset`
* `.start [<args>]` - `rails s <args>`
* `.run [<args>]` - `rails runner <args>`
* `.console [<args>]` - `rails console <args>`
* `.database [<args>]` - `rails dbconsole <args>`
* `.generate [<args>]` - `rails generate <args>`
* `.destroy [<args>]` - `rails destroy <args>`
* `.test [<args>]` - `rails test <args>`
* `.rspec [<args>]` - `rspec <args>`
* `.routes` - `rails routes`
* `.routes <pattern>` - `rails routes | grep <pattern>`

### `tmux`

Does not define any commands but runs `tmux` upon inclusion via `ctx_libs`
unless already in a `tmux` session.

## Inspirations

Upon entering a context you can:

* Go to the project directory.
* Start a `screen` or `tmux` session.
* Set project-specific environment variables (e.g. `PATH` or the prompt).
* Set project- or client-specific aliases and functions.
* Start some services in the background (e.g. Redis, Guard).
* Activate a Python virtual environment.
* Use an rbenv gem set.

## Bugs, Feature and Enhancement Requests

I'd love to hear your feedback! If something doesn't work, or should work
differently, just file an issue at https://github.com/grn/bash-ctx/issues/new.
