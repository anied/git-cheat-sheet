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

When rebasing, it is wise to make use of a "rescue tag;" this is a tag generated at the tip of the branch _prior_ to starting the rebase operation that can be used as a point to reset to should any issue be encountered and the operation need to be undone.

### Example Rebase Workflow

#### Setup

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

#### Set recovery tag

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

#### Rebase

Now that you've gotten the latest for each of the branches and set your rebase recovery tag you can run the rebase itself.

1. Checkout the branch you intend on rebasing `git checkout <branch-to-be-rebased>`
1. How you rebase depends on your needs:
    + If you have no need to review nor edit the rebase steps (meaning you don't want to squash, edit, etc, nor verify the commits being rebased before running the operation), you can simply rebase to the target:
        ```shell
        $ git rebase <your-target-branch> # most likely `git rebase main` or `git rebase master`
        ```
    + If you want to review the rebase script or make additional changes, then use the `-i` flag to run an [interactive rebase](#interactive-rebasing-example):
        ```shell
        $ git rebase -i <your-target-branch>
        ```
        Follow the on-screen directions.  **Note that if you haven't set an alternative editor for git ths script will open in whatever is the terminal default, which might not be a text editor with which you are familiar.**
1. Handle any conflicts that may arise.  It is possible that you will encounter and have to resolve multiple conflicts at several steps of your rebase.  This is _potentially_ (but not necessarily) more labor-intensive than a simple merge, but the benefit of an organized commit history is worth the cost.

#### Recovery and/or clean-up

##### Recovery

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

##### Clean-up

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

### Interactive rebasing example

When you include the interactive flag (`-i`) when rebasing, you are telling git you wish to run an [interactive rebase (official docs)](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History#_changing_multiple).  You will be presented with a text representation of the rebasing script that will look something like this:


```
pick f155792 Temp updates robots.txt to disallow indexing
pick 1e7734a Initial attempt at clean-up of Gitlab CI YAML
pick edb5dc4 Updates to newer syntaxes; fixes YAML to valid Gitlab CI syntax
pick 63dd013 Makes start/stop logs clearer; removes allow failure from build task
pick affd125 Fixes echo statements lacking quotation marks
pick 01efee3 More logging fixing; some debugging around jobs not running
pick f930732 More debugging
pick 350f93a DEBUGGING: adding conditional rule to build job
pick 17cccf8 DEBUG: attempting built-in deploy stage name for testing
pick 631d63e Updated rules for other stages (missed last time)
pick d77484e Checking filesystem with debugging task before fixing clone
pick c8a36d9 TEMP COMMIT FOR DEBUGGING
pick 5bf0170 Apparently the jobs are cached by commit :/
pick aed1755 Reconsolidates stages into build stage
pick a657d05 Removes preclean step from build; adds cspell.json
pick eef27c6 Fills in .gitlab-ci.yml for attempting a deploy
pick fc0b2cf Moves `before_script` steps into `build site` job
pick 36fb1ed DEBUG: verifying env vars are available in the assume_role context
pick c94ac71 DEBUG: Fixing typo from last commit
pick 90591ad DEBUG: Trying more methods of accessing vars in fn context
pick 20ff010 DEBUG: Troubleshooting syntax error with logging
pick 1c5a4c1 DEBUG: Verifying vars are available in deploy job script
pick f687602 DEBUG: Last commit yielded unexpected results; testing in build now
pick 213472b DEBUG: Verified build sees injected ones; looking for vars now
pick fdb262b Removing debugging logs

# Rebase ce4065b..fdb262b onto ce4065b (25 commands)
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup [-C | -c] <commit> = like "squash" but keep only the previous
#                    commit's log message, unless -C is used, in which case
#                    keep only this commit's message; -c is same as -C but
#                    opens the editor
# x, exec <command> = run command (the rest of the line) using shell
# b, break = stop here (continue rebase later with 'git rebase --continue')
# d, drop <commit> = remove commit
# l, label <label> = label current HEAD with a name
# t, reset <label> = reset HEAD to a label
# m, merge [-C <commit> | -c <commit>] <label> [# <oneline>]
# .       create a merge commit using the original merge commit's
# .       message (or the oneline, if no original merge commit was
# .       specified); use -c <commit> to reword the commit message
#
# These lines can be re-ordered; they are executed from top to bottom.
#
```

> **Note that if you haven't set an alternative editor for git ths script will open in whatever is the terminal default, which might not be a text editor with which you are familiar.**

The anatomy of these lines is as follows:

```
pick f155792 Temp updates robots.txt to disallow indexing
```
- `pick`: The operation being applied to the commit
- `f155792`: The commit to which the operation will be applied
- "`Temp updates robots.txt to disallow indexing`": The commit message for the commit

So this essentially states that the commit `f155792` (with commit message "`Temp updates robots.txt to disallow indexing`") will be cherry-picked onto commit `ce4065b` (from the top of the comments section: "`# Rebase ce4065b..fdb262b onto ce4065b (25 commands)`").

Each line will be executed in order, stopping as necessary for conflict resolution or editing, until the script completes or you abort the rebase process with `git rebase --abort`.  To omit a commit from the script, you can delete the line, or comment the line out with `#`, or change `pick` to `drop`.  There are other options, such as `squash` (collapsing into previous commit), `reword` (lets you update the commit message), `edit` (lets you make changes to a commit), and more.  The entire list of options is available in the comment at the bottom of the rebase script.

Edit the script to adjust the rebase to exactly what you need to do for your rebasing operation.  Once completed, save and close the text editor, and git will attempt to apply your rebase script.

## Git Bisecting

Git bisecting is a useful, semi-automated manner in which to efficiently review a range of commits to find when a specific thing happened.  This is particularly helpful for finding out when, how, and why something broke or otherwise changed in the codebase.  For the example below, we'll use a scenario in which something potentially broke in code and we need to track down the commit in which it happened to read the message and gain more context about the change.

### Example

In your user checkout flow for your e-commerce platform, there was previously a checkbox on the last step of the UI the user could click to mark the order as tax-exempt and remove a charge for tax.  This is now missing.  However, you can't find any tickets referencing this change, and are not certain if it was intentional or not.  There are no unit tests complaining that the tax exempt checkbox is missing, but it is possible that they were removed if this was an intentional change.  You decide to use `git bisect` to find the commit where this feature was removed in order to better understand if it was intentional or a mistake.

1. You search history to find where the feature was added and checkout that commit.
1. You start the bisecting operation:
    ```shell
    $ git bisect start
    ```
1. You mark the current commit as `good` because the feature was included and working properly at this point
    ```shell
    $ git bisect good
    ```
1. You checkout the tip of `main`, which is what is in Production where you know the feature is now missing; you mark this commit as `bad`:
    ```shell
    $ git checkout main
    $ git bisect bad
    ```
1. Git begins running the bisecting algorithm.  It checkouts commits at the midpoint between the good and bad commits, showing you roughly how many more steps until the operation should be complete.  At each step, you check if the feature is missing or present by marking it `git bisect good` or `git bisect bad` in the terminal
1. After a few steps, you find the offending commit.  You note that in the commit message it is noted the feature is being temporarily disabled, but should be re-enabled before being deployed to Production.  However, it appears this never happened.  Now that you understand the situation, you are able to speak with your project manager to get a ticket created to hotfix this issue.

## Additional Miscellaneous Helpful Commands

### Git Stash

A mechanism to temporarily store changes without committing them

- Add your changes to the stash (excludes new files, excludes a custom message in the stash list):
    ```shell
    $ git stash
    ```
- Push your changes to the stash _including_ new files with a custom message in the stash list
    ```shell
    $ git stash --include-untracked push -m "Dear git please take care of this awesome code i wrote"
    ```
- List what's in the stash
    ```shell
    $ git stash list
    ```
- See what's in a specific stash item
    ```shell
    $ git stash show -p stash@{3}
    ```
- Remove and apply the most recent stash (fast, but risky because if if doesn't apply cleanly you've lost the reference)
    ```shell
    $ git stash pop
    ```
- Apply the most recent stash without removing it (safer)
    ```shell
    $ git stash apply
    ```
- Apply a specific stash
    ```shell
    $ git stash apply stash@{2}
    ```

### Amending Commits

Change the most recent commit (meaning the commit at `HEAD`):

```shell
$ git commit --amend
```

Opens a text editor to update the commit message; any changes currently staged will be included in the new commit.  If you do this after having already pushed to origin you'll need to `--force-with-lease` as this is a history-changing operation.

> If you need to amend older commits, you're better off using an [interactive rebase](#interactive-rebasing-example), finding the commit you wish to update, and change `pick` to `edit` or `reword` in the rebase script (depending on what it is you are trying to do).

### Sharing Code with a Patch

You can take the current `diff` of your local environment and push it into a patch file, which can then be shared to other developers and applied as needed.  This is particularly useful if you are pairing and need to send changes to another developer to commit.

#### Generating a patch file

To generate a patch file:

```shell
$ git diff >> my-patch-file.patch
```

> (The command to push to a file may differ based on platform)

You will now see a new file in your local directory called `my-patch-file.patch`; this file can be shared to any other developer.  

#### Applying a patch  file

If you are the recipient of a patch file, place it in your repo and apply it like so:

```shell
$ git apply my-patch-file.patch
```

Note that you ideally should be on the same branch/commit as the person who recorded the patch; any deltas between your local filesystem and the filesystem in which the patch was generated could lead to merge conflicts or other unexpected issues when applying.

#### Clean-up

Make sure to delete the file before you make your next commit (only because there's generally no reason to clutter up your source code with old patch files).

### Git Cherry-pick

A simple command that lets you copy a commit onto your local branch:

`git cherry-pick <commit_hash>`

Generally a rebase is your preferred workflow, but there are some circumstances in which direct cherry-picking is the most convenient or efficient way to move around one or more commits.

### Git Blame

Find out who most recently edited a line of code by printing out the file with commit details on a per line basis with `git blame`:

```shell
$ git blame path/to/file
```

> If you are using [git-lens](https://gitlens.amod.io/) in VSCode you have no need for this, as the plug-in will generate this for you inline in your editor.

### Combining Commands with `&&`

Not a git-specific operation, but sometimes it can be efficient to combine multiple commands with `&&`.  For example, say I need to checkout my `main` branch, pull the latest, then re-checkout my feature branch and rebase it onto `main` and push it.  I know this will all execute cleanly, and I don't want to sit around waiting for the asynchronous network commands to complete.  I can combine all these commands with `&&` and go do something else while they complete.  If any one of them fails for some reason the others will not execute, so it is a relatively safe shortcut:

```shell
$ git checkout main && git pull && git checkout - && git rebase main
```

## Contributing

If you have suggestions, recommendations, or see problems that need fixing in this repo, feel free to open an issue or a pull request.  Thanks!