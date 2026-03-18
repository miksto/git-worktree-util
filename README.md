# git-wt

A lightweight command-line utility that simplifies working with [Git worktrees](https://git-scm.com/docs/git-worktree). Create, check out, and remove worktrees with short, memorable commands.

Worktrees are placed alongside the current repository directory, named `<repo>-<suffix>`. The worktree path is printed to stdout so it can be consumed by `cd` or other tools.

## Installation

Copy the `git-wt` script somewhere on your `PATH` and make it executable:

```bash
cp git-wt /usr/local/bin/
chmod +x /usr/local/bin/git-wt
```

### Requirements

- Git 2.5+ (worktree support)
- Bash

## Usage

```
git-wt -f <name>            Create a new branch in a new worktree
git-wt -b <branch>          Check out an existing branch in a new worktree
git-wt rm                   Remove the current worktree and cd back
```

### Create a feature branch

```bash
git-wt -f APP-1234-new-api
# Creates branch APP-1234-new-api from master/main
# Worktree at ../myrepo-APP-1234-new-api/
```

### Check out an existing branch

```bash
git-wt -b feature/APP-1234-new-api
# Worktree at ../myrepo-APP-1234-new-api/
```

The directory suffix is derived by stripping the first path component from the branch name (e.g. `feature/foo` becomes `foo`).

### Remove a worktree

From inside a worktree:

```bash
git-wt rm
# Removes the worktree and prints the main repo path
```

### Tip: cd into the new worktree

Since `git-wt` prints the worktree path to stdout, you can wrap it in a shell function to automatically cd into the result:

```bash
git-wt() {
  local dir
  dir="$(/path/to/git-wt "$@")" && cd "$dir"
}
```

Then `git-wt -f my-feature` will create the worktree and cd into it in one step, and `git-wt rm` will return you to the main repo.
