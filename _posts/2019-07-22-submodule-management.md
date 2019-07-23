---
layout: post
author: jayahariv
title: submodule management
---
# Git Submodule

## update all submodules when checkout branches
```
#!/bin/bash
exec git submodule update --recursive
```
1. add the script to: `<projectdir>/.git/hooks/post-checkout`
2. `chmod u+x <projectdir>/.git/hooks/post-checkout`



## git submodule update
Update the registered submodules to match what the superproject expects by cloning missing submodules and updating the working tree of the submodules.

The "updating" can be done in several ways depending on command line options and the value of submodule.<name>.update configuration variable.

The command line option takes precedence over the configuration variable. If neither is given, a checkout is performed.

The update procedures supported both from the command line as well as through the submodule.<name>.update configuration are: checkout, rebase, merge, custom, none.

# Latest hash of a repo
The Repo Commits API supports listing, viewing, and comparing commits in a repository.

## List commits on a repository
#### API Syntax: `GET /repos/:owner/:repo/commits`
