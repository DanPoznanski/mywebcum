---
title: "Git"
date: 2021-05-18
description: "Git"
tags: ["GIT",]
type: post
weight: 20
showTableOfContents: true
---


![git1](images/git1.svg)



Create a repository in an existing directory:

Linux:
```
cd home user my_project
```
MacOS:
```
cd Users user my_project
```
Windows:
```
cd C:/Users/user/my_project
```
then run the command:
```
git init
```
Add existing files under version control:
```
git add *.c
git add LICENSE
git commit -m ‘Initial project version’
```

Clone an existing repository
```
git clone https://github.com/libgit2/libgit2
```

Clone the repository to a directory named mylibgit:
$ git clone https://github.com/libgit2/libgit2 mylibgit

## Working with Git

### Git Add 

Add existing files under version control:
```
$ git status

On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean  
```
Adding README + status:
```
$ echo ‘My project’ > README
$ git status
On the master branch
Your branch is updated with ‘origin/master’.
Untraceable files:
(use ‘git add <file>...’ to include them in what will be committed)
README
nothing added to the commit, but untracked files are present (use ‘git add’ to
track)
```
Tracking new files:
```
$ git add README
$ git status
On the master branch
Your branch has been updated to ‘origin/master’.
The changes should be committed:
(use ‘git restore --staged <file>...’ to uncommit)
new file: README
```

### Git Status

Indexing the modified files:
```
$ git status
On the master branch
Your branch has been updated to ‘origin/master’.
The changes should be committed:
(use ‘git reset HEAD <file>...’ to uncommit)
new file: README
Changes not placed to commit:
(use ‘git add <file>...’ to update what will be committed)
(use ‘git checkout -- <file>...’ to remove changes in the working directory)
modified: CONTRIBUTING.md
```
Add this content to the next commit:
```
$ git add CONTRIBUTING.md
$ git status
On branch master
Your branch is up-to-date with ‘origin/master’.
Changes to be committed:
new file: README
modified: CONTRIBUTING.md
```
Let's make changes to the file via `vim` and request a new status:
```
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
new file: README
modified: CONTRIBUTING.md
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)
modified: CONTRIBUTING.md
```
A shorter status output:
```
$ git status -s
```
```
M README
MM Rakefile
A lib/git.rb
M lib/simplegit.rb
?? LICENSE.txt
```
### Git Ignore

Ignore files:
```
$ cat .gitignore
```
```
*.[oa].
*~
```

The following rules apply to templates in the `.gitignore` file:
- Blank lines, as well as lines beginning with #, are ignored
- Default templates are global and are applied recursively
for the entire directory tree
- To avoid recursion, use a slash (/) at the beginning of the template.
- To exclude a directory, add a slash (/) at the end of the template
- You can invert the template by using an exclamation mark (!) as the first character in the template.
as the first character

Example `.gitignore`:
Exclude all files with the extension `.a`
```
*.a
```
But keep track of the lib.a file even if it falls under the rules listed
```
!lib.a
```
Exclude the TODO file in the root directory, but not the file in subdir/TODO
```
/TODO
```
Ignore all files in the `build/` directory
```
build/
```
Ignore the file `doc/notes.txt`, but not the file `doc/server/arch.txt`
```
doc/*.txt
```
Ignore all `.txt` files in the `doc/` directory.
```
doc/**/*.txt
```
for more details `.gitignore` are available in manual
```
$ man gitignore.
```
### Git Diff

View indexed and unindexed changes:
```
$ git diff
```
```
diff --git a/CONTRIBUTING.md b/CONTRIBUTING.md
index 8ebb991..643e24f 100644
--- a/CONTRIBUTING.md
+++ b/CONTRIBUTING.md
@@ -65,7 +65,8 @@ branches directly, things can get confusing.
Please include a nice description of the changes when submitting PRs;
If we have to read the entire diff to understand why you are contributing
in the first place, you'll be less likely to get feedback and get your changes
-included.
+included. Also, break your changes into extensive chunks if your patch is
+longer than a dozen lines.
If you start working on a specific area, feel free to send a PR
describing your work (and mention in the title of the PR that it's 
```
See what you've indexed and what will go into the next commit:
```
$ git diff --staged
```
```
diff --git a/README b/README
new file mode 100644
index 0000000..03902a1
--- /dev/null
+++ b/README
@@ -0,0 +1 @@
+My Project
```

View changes to the file in different programs:
```
$ git difftool
```
Difftool Help:
```
$ git difftool --tool-help
```
### Git Commit

Commit changes:
```
$ git commit
```
VIM window example:
```
# Please enter a message to commit your changes. Lines starting
# with ‘#’ will be ignored, and an empty message will abort the commit.
# On the master branch
# Your branch is updated at ‘origin/master’.
# Changes that will be committed:
# new file: README
# changed: CONTRIBUTING.md
‘.git/COMMIT_EDITMSG’ 9L, 283C
```
Detailed changes:
```
$ git commit -v
```
Add your comment to the commit:
```
$ git commit -m ‘Story 182: fix benchmarks for speed’
```
```
[master 463dc4f] Story 182: fix benchmarks for speed
2 files modified, 2 inserts(+)
creation mode 100644 README
```
Ignoring indexing:
```
$ git status
```
```
On the master branch
Your branch has been updated to ‘origin/master’.
The changes have not been put on commit:
(use ‘git add <file>...’ to update what will be committed)
(use ‘git checkout -- <file>...’ to remove changes in the working directory)
changed: CONTRIBUTING.md
changes not added to commit (use ‘git add’ and/or ‘git commit -a’)
```
- `-a` automatic add all changed files to commit

- `-m` its need to add quotes commit ’new cicd'

```
$ git commit -a -m ‘Add new benchmarks’
```
```
[master 83e38c7] Add new benchmarks
1 file modified, 5 inserts(+), 0 deletes(-)
```




clone repository:
```
git clone git@lab.com:danpoznanski/example.git
```



take commit from outsorce need be in directory folder of project
```
git remote add out git@github.com:danpoznanski/pythonrobotics.git
```

see if all add you can :
```
git remove -v 
```

to get commit from outsource
```
git fetch out
```
to create new branch to secure project 
```
git branch develop out/master
```
to choice other branch for example `develop` 
```
git checkout develop
```

to get code from remote repository
```
git pull 
```

to transfer code to your repository
```
git push -u origin -o merge_request.create
```

`-u origin` - this option sets upsteam for your current branch contact to remote branch, this allows in the future use `git push` and `git pull` without indication remote repository and branch

`-o merge_request.create` its command for gitlab merge request create 




create script auto push and pull 
```
sudo nano autogit.sh
```
```
#!/bin/bash
cd ./folder/
git checkout develop
git pull 
git push -u origin -o merge_request.create
cd .. 
```
after add rules `chmod +x autorun.sh`

to run ./autogit.sh

to automatic i use cron 
crontab -e

every 5 minute to run
```
5 * * * * /home/admin/scripts/autogit.sh
```

commit 
```bash
git commit -am 'new cicd'
```
`-a` automatic add all changed files to commit 

`-m` its need to add quotes commit 'new cicd'

or

```
git add .; git commit -m "Update website"; git push
```


### Git Commit


no need do `git add .` 
```
git commit -a 
```
no need open texteditor
```
git commit -m
```
(--no-edit) open commit through reset and do new commit
```
git commit --amend  
```

### Git Reset

Mixed Reset: `git reset <commit>` resets the staging area to match the specified commit but it doesn't affect the working directory. Your changes are unstaged but not lost, allowing you review you changes before committing again
```
git reset
```

Soft Reset: `git reset --soft <commit>` keeps  you changes in the staging area. it means you can re-commit the changes after the reset. 
```
git reset --soft HEAD-1
````
Hard Reset: `git reset --hard <commit>` resets the staging area and the working directory to match the specified commit. Be careful with this option as it discards all changes in the working directory 

`HEAD-1` delete 1 commit

```bash
git reset --hard  
```

### Git rebase 

Assume the following history exists and the current branch is "topic":
```
          A---B---C topic
         /
    D---E---F---G master
```
From this point, the result of either of the following commands:
```
git rebase master
git rebase master topic
```
would be:
```
                  A'--B'--C' topic
                 /
    D---E---F---G master
```

### Git Merge 


```
	  A---B---C topic
	 /
    D---E---F---G master
```
```
git merge topicg
```
```
	  A---B---C topic
	 /         \
    D---E---F---G---H master
```




### Git cherry-pick 

With the "cherry-pick" command, Git allows you to integrate selected, individual commits from any branch into your current HEAD branch.

Contrast this with the way commit integration normally works in Git: when performing a Merge or Rebase, all commits from one branch are integrated.

![git2](images/git2.png)

Cherry-pick, on the other hand, allows you to select individual commits for integration. In this example, only C2 is integrated into the master branch, but not C4.

![git3](images/git3.png)

for example :
```
git cherry-pick af02e0b
```

### git rebase 2 commit

```
git rebase -i HEAD~2
```
in commit
```
squash or fixup
```
```
git rebase --continue
```


```
git rebase -i HEAD~3
```
in commit
```
edit
```
```
git rebase --continue
```

### git revert




```
git revert <hash commit>
```


### How restor git commit

`git reflog` or `git log -g` 

```
git reset --hard master@{"15 minutes ago"}
```

```
git fsck --full 
```


git besect
```
git besert <hash-commit>
```
git besert good or bad

git besert reset




git slash

git slash list 

git slash pop 

git slash apply


```
git slash drop slash@{1}
```
clear all git slash
```
git slash clear

```


git diff


```
git diff master <hash-commit1> 
```


git blame
```
git blame index.html
```
