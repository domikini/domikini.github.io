https://github.com/todotxt/todo.txt-cli

## Installation

## Download

Download the latest stable release for use on your desktop or server.

### OS X / macOS

brew install todo-txt

### Linux
From command line

    make
    make install
    make test

NOTE: Makefile defaults to several default paths for installed files. Adjust to your system:

    INSTALL_DIR: PATH for executables (default /usr/local/bin)
    CONFIG_DIR: PATH for todo.txt config
    BASH_COMPLETION: PATH for autocompletion scripts (default to /etc/bash_completion.d)

`make install CONFIG_DIR=/etc INSTALL_DIR=/usr/bin BASH_COMPLETION_DIR=/usr/share/bash-completion/completions`

### Usage

todo.sh [-fhpantvV] [-d todo_config] action [task_number] [task_description]

For example, to add a todo item, you can do:

todo.sh add "THING I NEED TO DO +project @context"

Read about all the possible commands in the [USAGE](https://github.com/todotxt/todo.txt-cli/blob/master/USAGE.md) file.


### Configure todo.cfg to store lists on Dropbox

Change the following line to point to Dropbox shared folder on local server/workstation

`export TODO_DIR=<path-to-dropbox-shared-folder>`

### Configure terminal to use aliases

Add following lines to .bashrc or .zshrc

    export PATH=$PATH:"<path-to-app>/todo.txt-cli"
    alias t="todo.sh -d <path-to-config-file>/todo.cfg"
    export TODOTXT_DEFAULT_ACTION=ls
