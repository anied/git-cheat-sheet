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
    ```shell
    $ git checkout <branch>
    ```
- Checkout the last branch you were on:
    ```shell
    $ git checkout -
    ```

### Branching

- List local branches:
    ```shell
    $ git branch
    ```
- List local and remote branches:
    ```shell
    $ git branch --all
    ```
- Create a new branch:
    ```shell
    $ git branch <new-branch>
    ```
  - Alternate to create branch and check it out in a single command:
    ```shell
    $ git checkout -b <new-branch>
    ```
- Delete a branch:
    ```shell
    $ git branch --delete
    ```
    or
    ```shell
    $ git branch -d <branch>
    ```
- Force delete a branch:
    ```shell
    $ git branch --delete --force
    ```
    or
    ```shell
    $ git branch -D <branch>
    ```

### Status

- View the status of your workspace:
    ```shell
    $ git status
    ```

### Fetching/Pulling/Pushing

- Fetch updates from the remote repository:
    ```shell
    $ git fetch
    ```
- Pull changes from the remote copy of the branch into the local copy of the branch:
    ```shell
    $ git pull
    ```
- Push local changes to the remote copy of the current branch:
    ```shell
    $ git push
    ```

### Logging

- View the commit log for the current branch:
    ```shell
    $ git log
    ```
- Log for all project branches ([best used as an alias](https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a)):
    ```shell
    $ git log --graph --decorate --branches --remotes
    ```
- Condensed log for all project branches ([best used as an alias](https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a)):
    ```shell
    $ git log --graph --decorate --branches --remotes --pretty=format:"%C(auto) %h %d %s"
    ```

### Diffing

- Show the difference between working directory and last commit:
    ```shell
    $ git diff
    ```
- Compare arbitrary points of history:
    ```shell
    $ git diff <branch1|commit1> <branch2|commit2>
    ```

### Staging Changes

- Stage all changes:
    ```shell
    $ git add --all
    ```
    or
    ```shell
    $ git add .
    ```
- Stage a specific file:
    ```shell
    $ git add <file>
    ```
- [Interactive staging](https://git-scm.com/book/en/v2/Git-Tools-Interactive-Staging):
    ```shell
    $ git add -i
    ```

### Committing

- Commit with a message:
    ```shell
    $ git commit -m "message"
    ```
- Edit the message in your default editor:
    ```shell
    $ git commit
    ```

> #### Best Practices for committing
> 
> - Frequent, scoped, and small commits.
> - Avoid mingling whitespace changes and code changes.
> - Follow a descriptive commit message format.

### Unstaging Changes

- Unstage all changes:
    ```shell
    $ git reset
    ```
    or
    ```shell
    $ git restore --staged .
    ```
- Unstage a file:
    ```shell
    $ git restore --staged <file>
    ```
- Interactively unstage a subset of changes by patch:
    ```shell
    $ git restore --staged --patch <file>
    ```

### Discarding Changes

> **⚠️ Caution:** removing changes before they are committed can be _irreversible_. To remove local changes while keeping them recoverable, consider `stash`ing them instead of discarding them.

- Discard all changes:
    ```shell
    $ git reset --hard
    ```
- Discard all changes and all new files:
    ```shell
    $ git reset --hard && git clean -f -d
    ```
- Discard a file:
    ```shell
    $ rm path/to/your.file
    ```
    (or whatever the delete/remove command is for your terminal environment)
- Discard a subset of changes within a file:
    ```shell
    $ git restore --patch path/to/your.file
    ```

## Aliasing

Create custom Git aliases for your favorite commands.

[Alex's personal alias list is maintained here: https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a](https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a)

- Examples:
  - Alias for checkout:
      ```shell
      $ git config --global alias.c checkout
      ```
  - Alias for a complex log command (including manual pager piping):
      ```shell
      $ git config --global alias.l '! git log --graph --decorate --branches --remotes --color=always | less -r'
      ```
  
## Revertability&trade; Strategies & Workflows

This is a collection of strategies and workflows that help you avoid and/or recover from potential data loss.

### Reverting

Creates a commit that undoes a previous commit. _The further in the past of the repo history the target commit is, the higher the likelihood that it will not revert cleanly._

```shell
$ git revert <commit>
```

### Resetting

Allows you to move the current branch label to an arbitrary commit.

```shell
$ git reset --hard <commit|tag|branch>
```

Example: You accidentally merged a feature branch to you `main` branch before you intended to.  You identify the last commit on `main` before you merged as `12345`.  As such you run:

```shell
git reset --hard 12345
```

### Tagging

Assigns a tag to a _commit_ that will remain _even if the branch is deleted_.

- Assigns a tag to the current position of the `HEAD`:
    ```shell
    $ git tag <your-tag-name>
    ```
- Assigns a git tag to a specific location:
    ```shell
    $ git tag <branch|commit>
    ```

Example: You want to rebase, but also want a way to undo it if anything goes wrong in the process:

1. Prior to rebasing, you checkout the branch you're trying to rebase
1. Run `git tag rebase-rescue` to set a tag at the commit at the current branch tip position
1. Run your rebase
1. If there was a problem after you finished rebasing, you can run `git reset --hard rebase-rescue` from the rebased branch to hard reset it to the commit of the recovery tag

### Stashing

Allows you to store code for later without committing it.

- Stash your changes:
    ```shell
    $ git stash
    ```
- Stash your changes and any new files:
    ```shell
    $ git stash --include-untracked
    ```
- Stash with a custom message to show in the list:
    ```shell
    $ git stash push -m "Saving for later"
    ```
- See the list of the current stashes:
    ```shell
    $ git stash list
    ```
- Review the patch of a specific stash reference `stash@{3}`:
    ```shell
    $ git stash show -p stash@{3}
    ```
- Apply `stash@{4}`
    ```shell
    $ git stash apply stash@{4}
    ```
- Clear all stashes
    ```shell
    git stash clear
    ```

Example: You need to stop in the middle of some feature work in order to work on a bug.  Your feature code isn't ready to be committed yet, though:

```shell
$ git stash push -m "feature work partially underway; stashing to work on a bug"
```

Later, when you come back, you can find the specific stash reference with `git stash list` and then apply it with `git stash apply <stash_reference>`.

### Reflog

- Learn how to recover from mistakes.
- Revert, reset, and tagging strategies.

## Git Rebasing Workflow

## Git Bisecting

## Additional Miscellaneous Commands











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