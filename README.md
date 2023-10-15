# Git Cheat Sheet

**A companion to the git presentation _"The Way I Use Git Is Objectively Correct And You Have To Do It This Way Too Or Else: A Megalomaniac's Guide To Git"_.**

## Environment Setup

- **Git Autocomplete**: Make sure to enable git command _and branch_ autocompletion
    - (the setup for this varies between platforms; you'll need to do some research for your specific OS and terminal environment)
- **Custom Prompt**: Customize your prompt to display useful information
  - (can include branch name, whether there are local changes, whether there are staged changes, etc)
  - (the setup for this varies between platforms; you'll need to do some research for your specific OS and terminal environment)
- **Aliases**: Create and manage custom command shortcuts for complex or common git commands
  - [Alex's personal list is maintained here: https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a](https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a) 
- **Editor Configuration**: Set up your text editor for Git operations if you don't want to use the terminal default text editor (Vim, Nano, etc)
  - For VSCode this is done by:
    1. [Install VSCode CLI](https://code.visualstudio.com/docs/editor/command-line) (installed by default for some versions; check your terminal environment)
    1. Run the following command to configure git to use code as its text editor:
        ```
        $ git config --global core.editor "code --wait"
        ```

## Git Basics

### Checking Out

- `git checkout <branch>`: Switch to a branch.
- `git checkout -`: Checkout the last branch you were on.

### Branching

- `git branch`: List local branches.
- `git branch --all --color=always | less -r`: List local and remote branches.
- `git branch <new-branch>`: Create a new branch.
- `git branch -d <branch>`: Delete a branch.
- `git branch -D <branch>`: Force delete a branch.

### Status

- `git status`: View the status of your workspace.

### Fetching/Pulling/Pushing

- `git fetch`: Fetch updates from the remote repository.
- `git pull`: Pull changes from the remote repository.
- `git push`: Push changes to the remote repository.

### Logging

- `git log`: View the commit log for the current branch.
- `git log --color=always --decorate | less -r`: Scrollable log with color.
- `git log --graph --decorate --branches --remotes --color=always | less -r`: Log for all project branches.
- `git log --graph --decorate --branches --remotes --pretty=format:"%C(auto) %h %d %s" --color=always | less -r`: Condensed log for all project branches.

### Diffing

- `git diff`: Show the difference between working directory and last commit.
- `git diff --color=always | less -r`: Scrollable diff with color.
- `git diff <branch1> <branch2>`: Compare arbitrary points of history.

## Aliasing

- Create custom Git aliases for your favorite commands.
- Examples:
  - `git config --global alias.c checkout`: Alias for checkout.
  - `git config --global alias.l '! git log --graph --decorate --branches --remotes --color=always | less -r'`: Alias for a complex log command.
  
## Revertability Strategies

- Learn how to recover from mistakes.
- Revert, reset, and tagging strategies.

## Advanced Git Operations

### Staging Changes

- `git add --all` or `git add .`: Stage all changes.
- `git add <file>`: Stage a specific file.
- `git add -i`: Interactive staging.

### Committing

- `git commit -m "message"`: Commit with a message.
- `git commit`: Edit the message in your default editor.

### Best Practices

- Frequent, scoped, and small commits.
- Avoid mingling whitespace changes and code changes.
- Follow a descriptive commit message format.

### Unstaging Changes

- `git reset` or `git restore --staged .`: Unstage all changes.
- `git restore --staged <file>`: Unstage a file.
- `git restore --staged --patch <file>`: Unstage a subset of changes.

### Discarding Changes

- `git reset --hard`: Discard all changes and files.

## Rebasing

- Rewriting history, moving commits, and other operations.
- Pros and cons of merging-only vs. rebasing strategies.

## Git Bisect

- A semi-automated way to find a specific commit.

## Honorable Mentions

- Git Stash: Temporarily store changes.
- Git Commit --amend: Change the most recent commit.
- Sharing Code with a Patch: Generate and apply patches.
- Git Cherry-pick: Copy a commit from one branch to another.
- Git Blame: Find out who wrote a line of code.
- Combining Commands with `&&`.
- Git Reflog: View a log of your actions.