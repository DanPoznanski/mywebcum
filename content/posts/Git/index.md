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
