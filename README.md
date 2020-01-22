# GitCheatSheet

This document presents a scenario of the creation of a Git controlled application. It goes through as many steps as possible to demonstrate the various functions in Git.

## Application creation

To create a new application we first need to have a console open to the directory where we want the application to reside. This will include the folder named after the application.

`c:\repos\MyGitApp>`

Now initialize the application

(`git init`)

    Initialized empty Git repository in C:/repos/MyGitApp/.git/

## Create Content

Now create some content in the root folder `c:\repos\MyGitApp`.
I have created a file and saved it. Now add the file to track it with:

(`git add addresses.json`) or (`git add .`) which will add all files everywhere

Now you can either choose which files to stage or commit everything.

To stage just use (`git add addresses.json`)

or just commit with the -a flag to include everything:

(`git commit -am "my commit message"`)

## Undo a commit

### Scenario 1

You have just made a commit and realized that you put the wrong commit message. You can edit the commit message with the following command:

(`git commit --amend`)

This will bring up a file that shows the commit message, lets you alter it, save and close the file, and then the message will be change. You are replacing the previous commit.

### Scenario 2

You have just made a commit and you have realized that you forgot to include a file in the commit. Follow these steps:

1. `git add forgotten_file.txt`
1. `git commit --amend`
1. Edit (if necessary) and save the commit message file

Note that if you are changing contents, but do not care to change the message you can use the `--no-edit` flag to skip the message change.

## Cleaning you Working Directory

Let's say that you want to delete all the files in your directory that are not tracked. These could be build objects, temporary file, etc.

(`git clean -f -d`)

The `-f` flag means force and the `-d` means to delete any subdirectories that become empty as a result.

You can use the flag `--dry-run` or `-n` to find out what it would delete.

Only not-ignored files are deleted by default. To include these as well use the `-x` flag.

-   I have just done this with `--dry-run` and it told me that it would delete the node_modules folder

#

## Stashing

Suppose you are working for a bit and you have made some changes that are not ready to commit, but you have been instructed to switch to another branch to make a hotfix. With the stash command in Git, you can save that work separately and return to it later. You can do this without having to mess up the commit history. Here are the steps:

1. In a branch with two files, make changes to both of them
1. Stage only one of them
1. `git stash`
    - notice that the working directory is returned to HEAD for _both_ files
    - If you wish to stash everything, but only removed the unstaged files include the `--keep-index` flag
    - To include untracked files: `--include-untracked` (excludes ignored files)
    - To include all files (including ignored): `--all`
    - To choose which files to include: `--patch`
1. `git stash list` to view the stashes you've stored
    - stash@{0}: WIP on erik: 5cf09dc made a change and added a file
1. Switch to master and make your hotfix
1. Switch back to the branch you did your stash on
1. `git stash apply`
    - If you had more than one stash, you can apply an older one with `git stash apply stash@{2}`
    - Applying does not delete the stash, so you can apply it again if you wish
    - Notice that the one staged file is no longer staged. If you wanted it to be you would have needed to use the command `git stash apply --index`
1. `git stash drop stash@{0}` to delete the stash
    - Alternatively you could have used `git stash pop` to apply and then auto drop

### Create Branch from Stash

Say that you have created a stash in a branch and then continued to work in that branch. You may have difficulty trying to apply that branch later on without merging. An alternative is to create a branch from the stash with:

(`git stash branch my-stashed-branch`)

This will create, switch, and apply those changes to the new branch. They will _not_ be checked in. The stash will be deleted.

#

## Push to Remote Repository

(`git push origin master`)

## Pull from the Remote Repository

(`git pull`)

#

## Create a new branch

Now we want to create a new branch so that we can work on a new feature. You create branches with the (`git branch`) command, but then you have to checkout (switch to) that branch after it is created. A quicker way to do this is to use the following:

(`git checkout -b my-new-feature-branch`)

This creates the new branch and checks it out at the same time. The names of branches cannot have spaces.

## What is HEAD???

Head is nothing more than an indicator as to what is the current branch.

## Merging

You have changes in one branch that you want to merge into another. You have to first have the destination branch be the current branch. Do this with the (`git checkout branch-name`) command. Next you do the merge command with:

(`git merge my-new-feature-branch`)

    Auto-merging addresses.json
    Merge made by the 'recursive' strategy.
     addresses.json | 7 +++++++
     1 file changed, 7 insertions(+)

This example shows that a merge did take place and that it was successful. In the case where no merge is necessary, it is called fast forwarding. This would occur if you had a feature branch that you did work on and you were merging back onto master that had not changed. When auto merge doesn't work, here is the message:

    Auto-merging addresses.json
    CONFLICT (content): Merge conflict in addresses.json
    Automatic merge failed; fix conflicts and then commit the result.

This will change the file with indications as to what has happened.

    {
        "name": "home",
        "line1": "Flat 26 Building 39",
    <<<<<<< HEAD
        "line2": "London",
    =======
        "line2": "Marlborough Road",
    >>>>>>> hotfix
        "city": "Ely",
        "postCode": "CB6 2SW"
    },

You must now manually edit to make the code correct. Staging the file will mark it as resolved and then commit as normal.

## Cancelling a Merge

If you find yourself doing a merge that you just want to get out of:

(`git reset --hard`)

## Deleting a branch

In the above example, we have merged our branch back onto master and we now have no more need of the my-new-feature-branch branch. Use the following command to delete it:

(`git branch -d my-new-feature-branch`)

    Deleted branch my-new-feature-branch (was 3ee2d8e).

## Branch Utilities

To see what branches have been merged and not merged onto the current (HEAD) branch:

(`git branch --merged`) or (`git branch --no-merged`)

You could use this determine which branches are safe to delete. You cannot delete a branch that has not been merged unless you use:

(`git branch -D some-branch`) This will force the deletion. You will lose work!!!

## Workflows

[This page](https://git-scm.com/book/en/v2/Git-Branching-Branching-Workflows) has a great explanation of different workflows that you can do with Git.

# Remote Branches

[Information comes from here](https://git-scm.com/book/en/v2/Git-Branching-Remote-Branches)

## Fetching

To synchronize your work with a given remote:

(`git fetch origin`)

This will lookup which server is origin (`git remote -v`) and fetches any data you don't have yet and move your origin/master pointer to its new position.

After you do this, you will have to merge the changes to your local branch. A shortcut for doing this is to use (`git pull`) instead which does both the fetch and the merge in one command.

## Remotes

https://git-scm.com/book/en/v2/Git-Branching-Remote-Branches

You can have multiple remotes. One is created automatically called origin when you do:

(`git clone https://github.com/ErikSharp/GitCheatSheet.git`)

The name defaults to origin, but you can change that with a flag with the clone.

You can add additional remotes like this:

(`git remote add dev-team git://git.dev-team.ourcompany.com`)

This could be useful if you wanted to have a server that contained work that did not go to the main server.

You can view your remote branches easily with:

(`git branch -vv`)

    iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
    master    1ae2a45 [origin/master] deploying index fix
    * serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
    testing   5ea463a trying something new

It’s important to note that these numbers are only since the last time you fetched from each server. This command does not reach out to the servers, it’s telling you about what it has cached from these servers locally. If you want totally up to date ahead and behind numbers, you’ll need to fetch from all your remotes right before running this. You could do that like this:

(`git fetch --all; git branch -vv`)

## Remote Tracking Branches

Any data that comes from a remote goes to a remote tracking branch. Think of this like another branch called origin/master. In the case of a second remote (dev-team), we would have another remote tracking branch called dev-team/master.

You can merge these updates onto your branch with (`git merge origin/master`).

If the remote contains more than just a master branch and you don't yet have it, you can easily create a local branch that will track the remote branch with:

(`git checkout serverfix`)

    Branch serverfix set up to track remote branch serverfix from origin.
    Switched to a new branch 'serverfix'

## Pushing to the Remote

When you want to share your changes with the remote:

(`git push origin hotfix`) hotfix is the name of a branch

Suppose that you have a local branch that you want to push to a remote server. Doing this will create the branch on the server and set it to be an upstream branch that your local branch will now track.

(`git branch -u origin/serverfix`)

    Branch serverfix set up to track remote branch serverfix from origin.

## Delete Remote Branch

You can delete a remote branch when everything from it has been merged with:

(`git push origin --delete serverfix`)

      erik      6c44595 [origin/erik] how to delete
    * master    6c44595 [origin/master] how to delete
      serverfix d0f40b4 [origin/serverfix: gone] removed server changes

#

## Credential Storage

[Here](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage#_credential_caching) is the official documentation on this.

GitHub is moving away from this in favour of their Personal Access Tokens (PAT). I got this email:

You recently used a password to access an endpoint through the GitHub API using git-credential-manager (Microsoft Windows NT 6.2.9200.0; Win32NT; x64) CLR/4.0.30319 git-tools/1.20.0. We will deprecate basic authentication using password to this endpoint soon:

https://api.github.com/user/subscriptions

We recommend using a personal access token (PAT) with the appropriate scope to access this endpoint instead. Visit https://github.com/settings/tokens for more information.

#

## Rebasing

Here's the scenario: you have done some work in a branch called experiment and now you want to put it into master. You don't want there to be a separate change to the master branch that puts all these changes in experiment into a single change (a merge). You probably don't another person to have the job of doing that either as they might have to makes decisions on how that is to be done with other code.

Create the scenario:

1. Create an experiment branch
1. Make two commits
1. Checkout the master branch
1. Make two commits

Steps to rebase:

1. `git checkout experiment`
1. `git rebase master`
    - this will put the two master commits before the experiment commits
    - experiment now contains all four of the changes
1. `git checkout master`
1. `git merge experiment`
    - This is only doing a fast forward merge to add the two experiment commits to the end

Another way to think about the rebase command (`git rebase master`) is that I want you to take the commits from master and put them before my commits in this branch, so that the stuff that I have done appears at the end.

Often, you’ll do this to make sure your commits apply cleanly on a remote branch — perhaps in a project to which you’re trying to contribute but that you don’t maintain. In this case, you’d do your work in a branch and then rebase your work onto origin/master when you were ready to submit your patches to the main project. That way, the maintainer doesn’t have to do any integration work — just a fast-forward or a clean apply.

The main benefit with rebasing is that you don't have a change in the history which is a combination of two separate changes (a merge). It will mean that the history will show each individual change. This is good because it often occurs that mistakes are made when merging and not when the individual commits were made.

In general the way to get the best of both worlds is to rebase local changes you’ve made but haven’t shared yet before you push them in order to clean up your story, but never rebase anything you’ve pushed somewhere.

## Interactive Rebasing

The scenario is that you wish to change a commit message that was two commits ago. Since `git commit --amend` only works on the previous commit, this will not work. Here is how to accomplish this:

1. `git rebase -i HEAD~2`
    - This will start the interactive rebase that will work with the previous two commits
1. Overwrite the _pick_ word with the correct verb for each commit
    - Notice that the commits are listed from oldest to newest
1. In this case overwrite _pick_ with _reword_
1. Save and close the file
    - The editor will then show a file where you can change the commit message
1. Change the commit message, save, and finally close the file
    - You will now notice that the message is changed in the history

Depending on the choices that you make in the rebase file, there will be different steps to completing the rebase. Instructions are provided as you go along.

## Rewriting History

You are free to manipulate history in your local repository, but there is much less that you can after you have pushed to a remote repository.
