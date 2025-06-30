---
description: New Repository
---

# 4️ Bitbucket

{% code fullWidth="true" %}
```
// Some code
A.Create a new Repository on Bitbucket
   1. login: Bigbucket  
 
   2. Create-> Select Repository -> Select Web Applications -> Create a new repository
        Project: Web Applications
        Repository name: @@@@@
        Access level: private repository
        Include a README? No
        Default branch name:
        Include .gitignore? No <======
        Create repository

   3. Copy URL:
        https://*****@bitbucket.org/team/*****.git [Remote URL]
```
{% endcode %}

{% code fullWidth="true" %}
```
// Some code
B.Create a git connection using Visual Studio
    1. Open Visual Studio,(right side) Get started, Clone a repository

    2. Repository location: [Remote URL]
        Path: Create a new folder under \git-steven\
        Press the Clone button, connect to the Bitbucke

    3. Copy old projects files into the new folder
        - A. copy .gitignore
        - B. copy \UI (do not include node_modules folder, use 'npm i' to generate)
        - C. copy \Auth, \App, \PowerBI

    4. Commit and Push
```
{% endcode %}

{% code fullWidth="true" %}
```
// How to use SourceTree to pull a specific commit from Main project

1. Clone Project B and open it in SourceTree.

2. 【Add】 Upstream Remote:
	Go to Tob menu, Repository > Repository Settings > Remotes.
	Add upstream with project A’s URL.  (can be commit url)
	Named this commit
	
3. 【Fetch】 Upstream:
	Click Fetch in the toolbar, Unselect "Fetch from all remotes"
	
	then select upstream. （Named commit）
		
4. 【Cherry-Pick】:
   Select "All Branches"
   
   In the commit graph, Select a specific commit.
   Right-click mouse key, Select Cherry Pick. OK   
	
5. 【Resolve Conflicts】:
	If conflicts arise, SourceTree will prompt you to resolve them.
	Select to delete unwanted Commits (files) that would not be merged
	select /package.json, click Unstage
	
6. 【Commit】
   Click Commit on the left top
   
	
7. 【Push】 
   Push to remote
 
```
{% endcode %}
