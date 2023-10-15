# Git Cheat Sheet

**A companion to the git presentation _"The Way I Use Git Is Objectively Correct And You Have To Do It This Way Too Or Else: A Megalomaniac's Guide To Git"_.**

## Environment Setup

- **Git Autocomplete**: Make sure to enable git command _and branch_ autocompletion
    - (the setup for this varies between platforms; you'll need to do some research for your specific OS and terminal environment)
- **Custom Prompt**: Customize your prompt to display useful information
  - (can include branch name, whether there are local changes, whether there are staged changes, etc)
  - (the setup for this varies between platforms; you'll need to do some research for your specific OS and terminal environment)
- **Aliases**: Create and manage custom command shortcuts for complex or common git commands
  - [Alex's personal alias list is maintained here: https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a](https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a) 
- **Editor Configuration**: Set up your text editor for Git operations if you don't want to use the terminal default text editor (Vim, Nano, etc)
  - For VSCode this is done by:
    1. [Install VSCode CLI](https://code.visualstudio.com/docs/editor/command-line) (installed by default for some versions; check your terminal environment)
    1. Run the following command to configure git to use code as its text editor:
        ```
        $ git config --global core.editor "code --wait"
        ```
- **Consider using [warp](https://www.warp.dev/) as your terminal environment,** as it includes many developer quality of life improvements as well as the ability to AI-generate commands from plain English input.  

## Git Basics

> **NOTE:** To pipe output to a text pager such as nano or less, you'll need to make some adjustments to the actual command
to tell it to include color, and potentially to tell the pager how to interpret the colors it is receiving.  For
example, a `log` command must be told to include color (`--color=always`), to include "decoration" (`--decorate`), and
then if piping to `less` it must include the `-r` flag to handle colors properly, like so:
> ```
> git log --graph --decorate --branches --remotes --color=always | less -r
> ```
> Generally for something this complex it might be simpler to alias the command.

### Checking Out

- Switch to a branch:
    ```
    git checkout <branch>
    ```
- Checkout the last branch you were on:
    ```
    git checkout -
    ```

### Branching

- List local branches:
    ```
    git branch
    ```
- List local and remote branches:
    ```
    git branch --all
    ```
- Create a new branch:
    ```
    git branch <new-branch>
    ```
  - Alternate to create branch and check it out in a single command:
    ```
    git checkout -b <new-branch>
    ```
- Delete a branch:
    ```
    git branch --delete
    ```
    or
    ```
    git branch -d <branch>
    ```
- Force delete a branch:
    ```
    git branch --delete --force
    ```
    or
    ```
    git branch -D <branch>
    ```

### Status

- View the status of your workspace:
    ```
    git status
    ```

### Fetching/Pulling/Pushing

- Fetch updates from the remote repository:
    ```
    git fetch
    ```
- Pull changes from the remote copy of the branch into the local copy of the branch:
    ```
    git pull
    ```
- Push local changes to the remote copy of the current branch:
    ```
    git push
    ```

### Logging

- View the commit log for the current branch:
    ```
    git log
    ```
- Log for all project branches ([best used as an alias](https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a)):
    ```
    git log --graph --decorate --branches --remotes --color=always | less -r
    ```
- Condensed log for all project branches ([best used as an alias](https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a)):
    ```
    git log --graph --decorate --branches --remotes --pretty=format:"%C(auto) %h %d %s" --color=always | less -r
    ```

### Diffing

- Show the difference between working directory and last commit:
    ```
    git diff
    ```
- Compare arbitrary points of history:
    ```
    git diff <branch1|commit1> <branch2|commit2>
    ```

## Aliasing

Create custom Git aliases for your favorite commands. [Alex's personal alias list is maintained here: https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a](https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a)

- Examples:
  - Alias for checkout:
      ```
      git config --global alias.c checkout
      ```
  - Alias for a complex log command:
      ```
      git config --global alias.l '! git log --graph --decorate --branches --remotes --color=always | less -r'
      ```
  
## Revertability Strategies

- Learn how to recover from mistakes.
- Revert, reset, and tagging strategies.

## Advanced Git Operations

### Staging Changes

- Stage all changes:
    ```
    git add --all` or `git add .
    ```
- Stage a specific file:
    ```
    git add <file>
    ```
- Interactive staging:
    ```
    git add -i
    ```

### Committing

- Commit with a message:
    ```
    git commit -m "message"
    ```
- Edit the message in your default editor:
    ```
    git commit
    ```

### Best Practices

- Frequent, scoped, and small commits.
- Avoid mingling whitespace changes and code changes.
- Follow a descriptive commit message format.

### Unstaging Changes

- Unstage all changes:
    ```
    git reset` or `git restore --staged .
    ```
- Unstage a file:
    ```
    git restore --staged <file>
    ```
- Unstage a subset of changes:
    ```
    git restore --staged --patch <file>
    ```

### Discarding Changes

- Discard all changes and files:
    ```
    git reset --hard
    ```

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