# Git and GitHub

## Intro

Welcome to a git and github lesson. Thanks for coming.

Today we will cover version control, why it is important, and how to use one flavour, git,
with one of the more popular web interfaces, github. This will shortly be our setup here at Precima.

Nearly everyone has had a version of the problem in the below comic, either with a paper, thesis or with code:

![](phd101212s.gif)

“Piled Higher and Deeper” by Jorge Cham, http://www.phdcomics.com

Most people are now familiar with microsoft words 'track change' tool - why doesn't something similar exist for code?

The answer is that it does, and there are multiple versions.

The current winner of the race is git. Older versions such as SVN and RCS are still extant, and
mercurial, or Hg, is still widely used. The benefit of newer versions, such as git or Hg is their distributed nature, and the fancy merging capabilities. GitHub and its associated pull request has lead git to
be the most commonly used version control software, and thus this is what we will use.

### What is git?

Simply put, git is a free, open source command line utility for tracking changes to text based files.

It was developed by Linus Torvalds, the same Linus who created Linux, in order to track changes to the linux kernel.

Under the hood, git has a vety nice tree based model and is incredibly powerful. For our purposes, we will use the beginners version today:

![](git.png)

Git - XKCD https://xkcd.com/1597/

As git is notoriously difficult to understand, there are many git front ends:
either guis, like [sourcetree](https://www.sourcetreeapp.com/) or [gitkraken](https://www.gitkraken.com/), as well as web SaaS like [GitHub](https://github.com/), [Gitlab](https://gitlab.com/) and [BitBucket](https://bitbucket.com/).

Precima is moving to git and GitHUb, so that's what we will focus on today.

GitHub offers free hosting of projects, with the caveat that they are open to the public internet.

The Model Engine team are currently using an organisation account, where we can keep our code private with up to five users with a cost of $25 per month, and extra users at $9 per user/month. I believe we are eventually planning to host a central github server locally for global use.

Git projects are organised into repositories, which contain a directory of code (or other text) under version control. We can track binary files, but this is not super efficient.

### A brief run through of the pmengine organisation

- Multiple repos in a project
- Web based view
- Issues
- Pull requests
- Commenting

### Instructions


#### Set Up a New Repo

The First thing we need to do is setup our local system. Open up your favourite terminal (I use powershell), and enter in some global config:

```BASH
$ git config --global user.name "Jeremy Gray"
$ git config --global user.email "jgray@precima.com"
$ git config --global color.ui "auto"
$ git config --global core.autocrlf true
```

We change our user name, email, colours and line endings.

Now we can set an editor (I use Atom, most of you use Notepad++, a couple use Sublime):

```BASH
$ git config --global core.editor "atom --wait"
$ git config --global core.editor "'c:/program files/sublime text 3/sublime_text.exe' -w"
$ git config --global core.editor "'c:/program files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"
```

Now we are ready for a new repo! Make a new directory:

```BASH
$ mkdir mygitrepo
$ cd mygitrepo
```

Now init a new repo - you should not do this is a directory with anything else in it! You will be tracking changes to everything in this directory. Try not to put git repos inside other git repos....

```BASH
$ git init
$ git status
```

We can see a little detail - and that we made a new folder called .git:

```BASH
$ ls -a
#or dir -Force
```

Inside this folder we store all our history and some other fun stuff - you probably shouldn't touch anything in there for now.

#### Adding Content

Let's make a simple python file:

```python
def helloworld():
  print('Hello World')

if '__name__' == __main__:
  helloworld()

```

And save it in this directory as 'helloworld.py'

Try running git status again:

```BASH
$ git status
```

We now have code here that is not 'tracked':
```
# On branch master
#
# Initial commit
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#	helloworld.py
nothing added to commit but untracked files present (use "git add" to track)
```

Let's add the files as suggested by the message -

```BASH
$ git add helloworld.py #or git add --all
$ git status
```

Now the files are in the staging area:
```
# On branch master
#
# Initial commit
#
# Changes to be committed:
#   (use "git rm --cached <file>..." to unstage)
#
#	new file:   helloworld.py
#
```

Git has a three stage system - changes that are present, but untracked, changes that are
added to the staging area, and changes that have been committed to the history.

We don't have to commit all the current changes - I regularly keep trash hanging around my repos.

Now we can commit the changes - this will store a permanent (mostly) history of the change.

Using the -m flag, we give a commit message. Make this as informative as possible!!!

```BASH
$ git commit -m 'initial commit of helloworld.py'
```

```
[master (root-commit) c23453] initial commit of helloworld.py
 1 file changed, 1 insertion(+)
 create mode 100644 helloworld.py
```

More info is better:

![](git_commit.png)

Now we have a history of our changes - we can see them using git log:

```BASH
$ git log
```

What if we have a large binary file, or csv, or other type of file we don't want to track?

The optional file .gitignore can be used to tell git not to track them:

```
*.csv
*.log
*.pyc
```

Save this in the root of the directory - now we can put csvs without tracking.

#### Tracking Content

Now we might want to make a change to the file:

```python
def helloworld():
  print('Version 1.0')
  print('Hello World')

if '__name__' == __main__:
  helloworld()

```

Save this, and run git status:

```
On branch master
nothing to commit, working directory clean
PS C:\Users\jgray\Desktop\mygitrepo> git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   helloworld.py

no changes added to commit (use "git add" and/or "git commit -a")
```

We can see helloworld is modified but the changes are not staged:

Try a diff:

```BASH
$ git diff
```

git diff also works on files or dirs - git diff file

```
diff --git a/helloworld.py b/helloworld.py
index e217053..9de4007 100644
--- a/helloworld.py
+++ b/helloworld.py
@@ -1,4 +1,5 @@
 def helloworld():
+  print('Version 1.0')
   print('Hello World')

 if '__name__' == __main__:
```

stage them

commit them

run status

As well as a diff to the most recent version, we can diff to older versions:

```BASH
$ git diff HEAD helloworld.py
```

Here head refers to the last commit. HEAD~1 refers to the one before, HEAD~2 etc etc.

We can also refer by the commit id - it's hexadecimal, so we only need the first few:

```BASH
$ git diff db97e helloworld.py
```

As well as diffing, we can check out:

```BASH
$ git checkout db97e helloworld.py
```

This will change the version of the file to that in that commit! Notepad++ users may have to reload,
better text editors will change straight away. If you do this without committed changed, git may complain - to
revert everything to your latest commit, use a hard reset:

```BASH
git reset --hard
```

Warning: this will delete any local changes!

#### Working With Remotes

So far, everything we have done is local to our machine - we have not used anything from github - only git.

Now, imagine we want to share our code across computers, or with collaborators. We could use dropbox or box, or some other solution.

But, these are terrible for code!

What we want to do is set up a remote, and we will do that using github.

#### Caution!

GitHub is a public website, and any code on there is by default open to the world. Do not put anything on here that is not in a private organisation or repo.

To set up a remote, we need to go to github and set up a repo.

The github instructions are hopefully self explanatory - we make a new repo, then set our remote as the git repo:

```BASH
git remote add origin https://github.com/user/repo.git
git push origin master
```

The push command will hopefully ask for your github username and password.

Now we have uploaded our code to GitHub!

Try browsing the history, and the diffs.

We can make edits on the website (if we really must), using the edit button, and commit from there.

The opposite of push is pull - this will let us update our local code from the github version:

```BASH
$ git pull origin master
```

#### Collaborating

Once we have a repo set up, we can add others to have write access (or read access if private).

Once we have more than one user, we might want to clone a repo - this will pull down the current version from github into a new directory:

```BASH
$ git clone https://github.com/user/repo.git
```

Now the user can commit to the repo like the other user - the git log will reflect the user who last made changes.


#### Branching, Merging and Conflicts

##### Conflicts

Once this is set up, the first thing that will happen is a merge conflict.

This happens when two people edit the same file, at the same place. Git is normally extremely good at fixing these changes, but sometimes will not be able to:

This will most likely look like this upon pushing to github:

```
To https://github.com/user/repo.git
 ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'https://github.com/user/repo.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull')
hint: before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

When you try to fix it using git pull, as recommended, we get another error:

```
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.
```

You will need to open the file, and edit it. Merge conflicts are automatically flagged by most newer text editors,
but will look like this:

```
<<<<<<< HEAD
We added a different line in the other copy
=======
This line added to one copy
>>>>>>> dabb4c8c450e8475aee9b14b4383acc99f42af1d
```

Once this is fixed, add, commit and push.

Remember the hint from above:

Merge the remote changes (e.g. 'git pull') before pushing again.

#### Branches

To reduce merge conflicts, it is useful to work on branches.

Branches allow different versions of the same code to coexist - for the model engine, we have a master branch,
which should match the current release, a dev branch with completed features since then, and feature specific branches for adding in new
features.

This is roughly the [gitflow workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow/).
Others exist, but make sure you and your collaborators choose one!

To create a branch, use the git branch command

```BASH
$ git branch newbranch
```

To switch to this newbranch, use checkout:

```BASH
$ git checkout newbranch
```

Now you can work safely, without having to worry about merge conflicts with other branches or collaborators.

At some point however, you will want to merge back into the master, so it is worth while merging as often as possible:

```BASH
git merge master
```

Most of the time this will work, but if git cannot automatically update, you will get the same problem.

You can fix this with a merge conflict resolution, or simply copy across files outside of git and make a new commit
(this might lead to more problems down the line though).


### Summary

We now know why to use git, and how to use it locally and remotely with GitHub!

#### Key Commands

![](git.jpg)

- git init - start annew git repo in the current directory
- git status - get current status of changes
- git add - add files to the staging area
- git commit - add current staged files to the repo
- git log - check history of the repo
- git diff - find the diffs of all files (or specific ones)
- git checkout - change to a historical commit, or a branch
- git reset - rest all changes in a repo. --hard helps!
- git remote - add or interact with a remote repo
- git push - push local changes to a remote repo
- git pull - pull changes from a remote branch
- git fetch - get all remote changes, but do not apply them (risky!)
- git clone - clone a remote repo
- git branch - make a new branch
- git merge - merge between branches (or commits)

There is much more to git, but this covers 95% or everyday use.

### Further Resources

- Git lesson, Software Carpentry http://swcarpentry.github.io/git-novice/
- Try Git - https://try.github.io/levels/1/challenges/1
- GitHub Guides - https://guides.github.com/
- Atlassian git tutorials - https://www.atlassian.com/git/tutorials/
- Version Control with Git, 2nd Edition - http://shop.oreilly.com/product/0636920022862.do


Copyright Notice - Contains material substantially modified from the [Software Carpentry](http://software-carpentry.org/) git lesson,
 [available here](http://swcarpentry.github.io/git-novice/).
This content is copyrighted by Software Carpentry, under the [CCBY 4.0 license](https://creativecommons.org/licenses/by/4.0/)
