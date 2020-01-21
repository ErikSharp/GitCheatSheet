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

## Push to Remote Repository

(`git push origin master`)

## Pull from the Remote Repository

(`git pull`)

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

This example shows that a merge did take place and that it was successful. In the case where no merge is necessary, it is called fast forwarding. This would occur if you had a feature branch that you did work on and you were merging back onto master that had not changed.

## Deleting a branch

In the above example, we have merged our branch back onto master and we now have no more need of the my-new-feature-branch branch. Use the following command to delete it:

(`git branch -d my-new-feature-branch`)

    Deleted branch my-new-feature-branch (was 3ee2d8e).
