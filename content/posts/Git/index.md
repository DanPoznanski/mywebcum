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
