---
description: dotnet new gitignore  (create a new .gitignore)
---

# 3️ Git command

dotnet new gitignore (create a new .gitignore)

#### [**git command**](https://mp.weixin.qq.com/s/rlDgU8a\_8GbHeDRduHXazQ)

* [x] git checkout -b feature/new-branch (create a new branch)&#x20;
* [x] git push (-u) origin feature/new-branch (push to remote server)
* [x] git status (current branch action status)&#x20;
* [x] git branch (list local branch and current branch)&#x20;
* [x] git branch -a (list all branch)&#x20;
* [x] git checkout master (switch to master)

#### In visual studio&#x20;

Create a new branch: at bottom-right select master, than select new branch \
&#x20; a. input new branch name such as: feature\new-branch \
&#x20; b. select origin/master \
&#x20; c. don't select 揟rack remote branch? (\*\*\* <mark style="color:red;">**very important**</mark> \*\*\*) \
&#x20; d. press Create Branch change something, writing command and publish

#### Resolving the conflict in merge

Entry repository directory

Check out and pull the recent version of the destination (master)branch from the remote. Git checkout master git pull origin master

Check out the source (new-names) branch. git checkout feature/booking-interface git pull origin feature/booking-interface

Merge the master Branch into the new-names branch.

git merge master

see different use merge and push

fix migration, in VS, press merge, see different, select and press Target





At migration folder, select and add some migration have not include in VS.

1. Update-database -TargetMigration: \[second last good migration name]
2. Exclude new migrations from project
3. Include one by one migration Add-Migration \[full name of migration]
4. Finally, update-database

\-----------------------------------

\
