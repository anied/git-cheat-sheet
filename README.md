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
> - Establish and adhere to a commit message format (consider [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) or [cbeams' proposal](https://cbea.ms/git-commit/))

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

The log output from `git reflog` shows a history of all the positions the `HEAD` has occupied.  You can use this to recover things that are otherwise missing from history.  Here's an example of recovering a lost branch.

1. You create a branch `feature/my-cool-feature` and do all your work for a feature on it.
1. For some bizarre reason you never push it to the remote; it only exists on your machine.
1. You checkout your `main` branch.
1. You somehow accidentally delete `feature/my-cool-feature` branch!!! 😫😫😫
1. Since the branch was never pushed to the remote, there is no reference to it at `origin` from which you can recover it
1. However, you can run `git reflog`:
    1. Find the reference for the last time you were on `feature/my-cool-feature` (if you had completed this exact sequence, it would probably be `HEAD@{0}`)
    1. `git checkout <that-reference>`
    1. Follow the instructions show on screen to restore the branch: `git switch -c feature/my-cool-feature`
1. Voilà! Your branch is restored! 🎉

> **NOTE:** The reflog is a _local_ reference; it does not get pushed to the remote.

## Git Rebasing Workflow

Rebasing is marginally more complex than simply merging, but the tradeoff is a highly organized, readable history, well-suited for collaboration.  It is an manner in which you can move a group of commits (generally a branch) from their current point in history to a new base.  _Optionally_, if rebasing "interactively," one can also apply additional operations, such as omitting certain commits, squashing certain points of history, editing commits, and more.

> **NOTE**: Destructive history operations&mdash;such as squashing&mdash;are an _optional_ component of rebasing.  There is nothing required or necessarily implied about squashing history when rebasing; rebasing is generally, by default, a non-destructive operation in which you can retain your original separate and discrete commits.

When rebasing, it is wise to make use of a "rescue tag;" this is a tag generated at the tip of the branch _prior_ to starting the rebase operation that can be used as a point to reset to should any issue be encountered and the operation need to be undone.  The whole process would look roughly like so:

### Setup

Make sure you have the latest versions of both the branch being rebased and the target branch onto which you will be rebasing:

1. Get up-to-date reference for remote: `$ git fetch origin`
1. Get latest of branch to be rebased:
    ```shell
    $ git checkout <branch-to-be-rebased>
    $ git pull
    ````
1. Get latest of branch on to which you are rebasing (we'll assume `main` for example purposes here):
    ```shell
    $ git checkout main
    $ git pull
    ```

### Set recovery tag

Set a tag to which you can hard reset your branch should you need to undo your rebasing operation:

1. Checkout your branch you will be rebasing: `git checkout <branch-to-be-rebased>`
1. Tag the commit at the tip of the branch with a unique, recognizable tag.  One potential format would be a string concatenation of `RR_` (for rebase rescue), plus the branch name, plus a date string.  So for instance if the branch name was `my-cool-feature` you would tag it with:
    ```shell
    $ git tag RR_my-cool-feature_202310161013
    ```
    If you leverage this format and you are using [my aliases](https://gist.github.com/anied/fb7b9abdfe861205b23ed78be2a05a1a) then you could also accomplish this with:
    ```shell
    $ git rrt my-cool-feature
    ```
1. You can find this tag again by either listing all tags (`git tag` or `git tag --list`) or by reviewing a history log with all tags (`git log --decorate --graph --tags`)

### Rebase

Now that you've gotten the latest for each of the branches and set your rebase recovery tag you can run the rebase itself.

1. Checkout the branch you intend on rebasing `git checkout <branch-to-be-rebased>`
1. How you rebase depends on your needs:
    + If you have no need to review nor edit the rebase steps (meaning you don't want to squash, edit, etc, nor verify the commits being rebased before running the operation), you can simply rebase to the target:
        ```shell
        $ git rebase <your-target-branch> # most likely `git rebase main` or `git rebase master`
        ```
    + If you want to review the rebase script or make additional changes, then use the `-i` flag to run an interactive rebase:
        ```shell
        $ git rebase -i <your-target-branch>
        ```
        Follow the on-screen directions.  **Note that if you haven't set an alternative editor for git ths script will open in whatever is the terminal default, which might not be a text editor with which you are familiar.**
1. Handle any conflicts that may arise.  It is possible that you will encounter and have to resolve multiple conflicts at several steps of your rebase.  This is _potentially_ (but not necessarily) more labor-intensive than a simple merge, but the benefit of an organized commit history is worth the cost.

### Recovery and/or clean-up

#### Recovery

Perhaps after your rebase you learn that you accidentally resolved a conflict incorrectly, and wish to re-run the rebase.  This is easy enough to do because you set a rebase rescue tag before you rebased.

1. Find the name of your rebase rescue tag (if you recorded it you can just copy it to your clipboard, otherwise you'll need to find it by either listing all tags (`git tag` or `git tag --list`) or by reviewing a history log with all tags (`git log --decorate --graph --tags`))
1. Checkout the branch you rebased:
    ```shell
    $ git checkout <branch-you-just-rebased>
    ```
1. Reset hard to your rescue tag:
    ```shell
    $ git reset --hard <your_rescue_tag>
    ```

#### Clean-up

If everything went great and you have no need to undo the rebase, you can finalize things and clean-up.

1. A simple `git push` won't work because you've rewritten history with the rebase.  To get your rebased branch to the remote you'll need to force push:
    ```shell
    $ git checkout <your-rebased-branch> # if it isn't already checked out
    $ git push --force-with-lease # forces pushes unless there's new commits on the branch; useful if there are multiple collaborators working on a single branch
    ```
1. If you have any collaborators working on the same branch, alert them to the fact that you've rebased the branch; they'll need to pull down a fresh copy of the branch.  Having multiple developers working directly on a single branch is often complicated and fraught with extra overhead and potential for error, so hopefully this is seldom the case.
1. Delete the rescue tag you previously made, as you have no further need for it:
    ```shell
    $ git tag --delete <your_rescue_tag>
    ```

## Git Bisecting

## Additional Miscellaneous Commands




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