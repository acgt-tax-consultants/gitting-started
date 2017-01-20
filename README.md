# gitting-started
A quick guide on GitHub Organization workflows

#### Table of Contents
- [Setting up a work environment](#setting-up-a-work-environment)
  - [Fork it](#fork-it)
  - [Clone it](#clone-it)
  - [Managing remotes](#managing-remotes)
- [Working with the codebase](#working-with-the-codebase)
  - [Branches](#branches)
- [Submitting code to the organization](#submitting-code-to-the-organization)
  - [Pull Requests](#pull-requests)
  - [Merge Conflicts](#merge-conflicts)
  - [Code Review](#code-review)

In a team environment, it does not work very well to push directly to the
shared main repository. This can cause many issues across local copies, so the
preferred way to utilize GitHub is through pull requests and feature branches.
This guide goes through that process.

## Setting up a work environment
---

First thing is first, you need to get your environment ready to work with
GitHub and GitHub organizations.

### Fork it

First, go to the GitHub repository you would like to work on.

*For example: https://github.com/acgt-tax-consultants/orchard*

Then, **Fork** it. Forking makes a copy of the main repo under your own profile.  
This is as easy as finding the Fork button in the upper righthand corner and
clicking it.  

### Clone it

Next, you will want to get your local copy ready to go on your machine.  
This is the same as any cloning of a repository, but you just want to make sure
you clone your own profile fork to your local machine first:

```bash
git clone https://github.com/<your_username>/orchard.git
cd orchard
```

That's it for cloning...

### Managing remotes

Now that you are in your local directory, we will want to link the organization
repo.  

Typing `git remote -v` will show you your list of remote repositories
available, and at the moment there should only be one named `origin`:

```bash
$ git remote -v
origin	https://github.com/<your_username>/orchard.git (fetch)
origin	https://github.com/<your_username>/orchard.git (push)
```

In order to continually get the most recent codebase to your local system, the
organizational repo must be linked up. The general practice for this is to set
a remote named `upstream` that is your non-working repository. That's just a
simple command:

```bash
$ git remote add upstream https://github.com/acgt-tax-consultants/orchard.git
$ git remote -v
origin	https://github.com/<your_username>/orchard.git (fetch)
origin	https://github.com/<your_username>/orchard.git (push)
upstream	https://github.com/acgt-tax-consultants/orchard.git (fetch)
upstream	https://github.com/acgt-tax-consultants/orchard.git (push)
```

And that's that...  
You are now fully set up and ready to get on with coding...

## Working with the codebase
---

Coding is coding, but in order to keep things neat and organized `git` has what
are called branches. Branches allow you to have multiple different changes
happening at once on a single machine/folder/repository. You could essentially
have an unlimited number of different versions of the same codebase all in a
single folder on your machine that you can simply pop in and out of.

A typical way to use branches is to create one for each feature you are working
on, and to leave the master branch alone entirely, other than to pull the
`upstream`'s master to your machine to have and to be able to branch off of
later on.

### Branches

When you first create or clone a `git` repository you are most likely just on
the `master` branch. This is basically where the current version of your
project lives.

Now, let's say you wanted to work on a new feature, say updating the README or
something just so there is an example to work with.

You first would want to go to your master branch (if you aren't already there).
You can check which branch you are on just by typing `git branch`, and it
should display all the branches on your machine with your current one being
highlighted and an asterisk next to it:

```bash
$ git branch
* master
```

From there you will want to get the current organization codebase by running:

```bash
$ git pull upstream master
```

This will either give you anything that is new OR just spit out `Already
up-to-date.` in which case you are good to go!

Now that you have the most recent code you will want to create your feature
branch.

```bash
$ git checkout -b update-readme
Switched to a new branch 'update-readme'
```

Now if you run `git branch` you'll see your new one, and that you are currently
on it:

```bash
$ git branch
  master
* update-readme
```

On this branch you make any changes you want and then finally do your normal
`git` routine of:

To see all the files you have changed you can run:

```bash
$ git status
On branch update-readme
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README.md

```

Now, this depends on if you have extra files in your folder that you don't want
to update, but you can do a few variants of adding the files to be tracked in
your commit.

```bash
$ git add README.md             # Add a single file
$ git add specific_directory/   # Add everything in a directory
$ git add -A                    # Add everything in the entire directory
```

Afterwards if you run `git status` again you will see your files to be
committed:

```bash
$ git status
On branch update-readme
Your branch is up-to-date with 'origin/tutorial'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.md
```

From there you can commit it and you are almost done.   Adding (above) can be
skipped if you didn't add any new files, but just edited existing ones by
running:

```bash
$ git commit -a
```

That will automatically add all tracked files that were modified to the commit.


So you have your commit made and you're good to go... Push it.

```bash
$ git push
fatal: The current branch update-readme has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin update-readme
```

This means that your local branch doesn't have a branch it is linked with yet
on your origin repository. This will only need to be done the first time you
push to the branch, and from then on it will remember it. A nice shortcut for
the above is to just run:

```bash
$ git push -u origin update-readme
```

Boom, you're done. It'll spit out a bunch of stuff and let you know when it is
complete. Now your code is in your profile fork of the Organization's
repository. Next we'll get it sent over to the actual organization.

## Submitting code to the organization
---

### Pull Requests

The method that GitHub has for allowing people to submit code is a Pull
Request. This is not a `git` feature itself, so it must be done from the GitHub
website. The process is super simple though. If you just pushed to your branch,
you can actually just go to your site at
`https://github.com/<your_username>/orchard.git` and it should pop up with a
big yellow alert bar saying something `Your recently pushed branches:` and a
huge green button that says `Compare & pull request`. Clicking that will bring
you to a page where you can give your PR a title and a description to let
people get a better idea of what it changes.

At the bottom of that page there will be a `Create pull request` button that
will open your PR, and start running any automated tests the repo might have
set up through something like Travis-CI.

Sometimes you want to make a PR for a branch that you haven't just pushed to,
so GitHub might not pop up with the convenient bar to do a one-click PR. To do
it manually you just go to your profile's repo, and on the right side there is
a dropdown that should say `master` at first, and you find the branch you want
to open a PR for. Switch over to it and then right next to it there is a `New
pull request` button that will bring you to the same page as the automated way.

### Merge Conflicts

Sometimes multiple people will be working in the same areas and one of their
PR's will be merged first, causing the other to be blocked from being able to
be merged. This is called a merge conflict, and if done correctly can be solved
fairly easily. Say someone else edited the `README.md` from the earlier example
and their PR was merged causing a conflict with the one that was made in your
PR. You could just pull the organization code on top of yours and let there be
a "MERGE:..." commit added to it, but that is kind of ugly, so there is a
better way.

If you have a merge conflict `cd` into to your repository folder on your local
machine and run the following:

```bash
$ git pull --rebase upstream master
```

Now, this will puke because there are conflicts, but it is fixable. It will
tell you that there were conflicts found in so and so files. Open the file in
your editor and you will see a bunch of `<<<<<<<<<<<<<<< a38f8bna` etc mixed in
the content. Each one divides your code from the code that was in master, so if
you want to keep both you simply remove all those ugly lines and fix the code
base so it will have both of your code and the master code in it neatly and
then add the fixed files and continue the rebase, as done by:

```bash
$ git add README.md   # or whatever filename had the conflicts
$ git rebase --continue
```

There is a chance it might puke a few times if there were multiple commits
since your branch was last updated.

Conflicts can end up being a real nightmare, so if there are any questions just
find someone to ask.

### Code Review

Voila, your code is now in a PR on the organization's repository that has been
tested by any automated testing suites linked to the GitHub organization. Now
it just has to be reviewed by someone ***other than yourself*** and merged by
them. No one should ever merge their own code. In our case the testing suite
will include `flake8` which checks your syntax formatting in respect to
Python's PEP-8 style guide. If anything is wrong the tests will fail and you
should correct any issues on your local machine's branch and commit and push
the changes. Because that branch has an open PR it will automatically show up
in the PR that is open and tests will be re-run automatically again.

Once it passes the automated testing someone will have to look at it and this
is where people can kind of just give constructive criticisms, including
suggesting performance improvements by changing how an algorithm works, or
seeing something awesome that could be pulled into a helper function and used
somewhere else in the codebase as well. It's kind of just a back and forth
before finally merging the code.


That's all...

fin
