---
title: 'Some Git basics and Github'
date: "2020-02-13"
tags: 
    - Development
    - Git
    - Github

categories: 
    - How-To
    - Git
    - Development
---

# Some Git basics using Github.

Here are some Git versioning basic how to's for creating and updating a new project or repository.
More detailed guides and reference materials can be viewed at __https://git-scm.com__.
More specifically first time setup can be found on this guide: [**here**](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup)

> __*Note:*__
We are using **Github** in this guide as the host for our remote repository

## Setting up git on your machine.
Once git is installed (See [Here](https://git-scm.com/downloads) if you have not done so yet),
we need to setup your username, and email details, as well as the repo's upstream remote origin location (Where the remote repository is on the web).

To setup your username and email for every repository:
``` bash
$ git config --global user.name "my name"
$ git config --global user.email "my.email@example.com"
```
And to check or view username once set:
``` bash
$ git config --global user.name 
> my name
```
And to check or view email once set:
``` bash
$ git config --global user.email
> my.email@example.com
```
To setup your username and email for a single repository:
``` bash
$ git config user.name "my name"
$ git config user.email "my.email@example.com"
```
And to check or view username once set:
``` bash
$ git config user.name 
> my name
```
And to check or view email once set:
``` bash
$ git config user.email 
> my.email@example.com
```

## Creating a new Repository.

There are two ways you can create a new reposotory.
Locally on your machine, or on the Remote Machine (In our case on Github).

For thoes users that just want to edit/use a github project, the easier option is to just create a remote repository and clone from there.

### Create local git repo

The first is by initializing a new repository in a folder on your local machine.

This is done by the **git init** command.

``` bash
$ git init
```
This should generate a ".git" folder.
Now any file/folder changes within the root git folder will tracked.

We can now pull/push updates from our local machine to any remote repository, once we have configured a remote repo target.

### Create remote repo

Creating a remote repository can be done in a few clicks.

Here is a simple to follow guide [Here](https://help.github.com/en/articles/create-a-repo)
from the Github help docs page.

Once the repo is created on the remote side, we can then clone it localy, and proceed to pull/push updates from our local machine to the remote repository.

### Clone from remote.

To clone an existing project from the remote location to you local machine, you can use the **clone** command.

`Using Https`

``` bash
$ git clone "https://github.com/your_name/your_remote_repo/"
```

`Using SSH`

``` bash
$ git clone "git@github.com:user_name/your_remote_repo.git"
```

### Cloning with submodules.

Sometimes you application/project may come with 3rd party dependencies from other repos.

In this case, if you want to clone including these, you can use the "--recursive" flag.

``` bash
$ git clone --recursive "git@github.com:user_name/your_remote_repo.git"
```

## Pull & Push from the Local machine.

After you have been working on your project, you may have some changes that you would like to push to the remote machine, or you need to resync and changes from the upstream repo into your local repo.

### Pull

In order to sync/pull and changes from the upstream remote repository, you can use the **pull** git command.

``` bash
git pull
```

### Stage, Commit & Push

In this case you can first add the changes you want, to the list of staged files. (Staged files are flagged for the next commit)

To do this, run the **add** command with the file name(s).

``` bash
git add newfile.xyz
```

If you want to quickly add all the files with changes, use the **.** flag

``` bash
git add .
```

You can remove any file add in error by the **rm**
command.

``` bash
git rm file.zxy
```

Once all the files are in staging, you can proceed to ready a **commit**.

To create a commit, simply use the **commit** command with the **-m** message flag.

``` bash
git commit -m "Description of change being commited"
```

To create a commit with all recent changes (And skip the **add** command) use the **-a** commit flag.

``` bash
git commit -a -m "Description of new changes worthy of commit"
```

Lastly we can **push** the commit to the upstream remote repository.

This is simply done with the **push** command.

``` bash
git push
```

## Creating a git tag.

Sometimes its helpfull to keep track of particular commits.
One of the ways to do this, is the use of a Git Tag.

#### You can easily create one, using some basics commands:

This should list any existing tags that have been created for the project/repo.

``` bash
$ git tag
```

The following creates a new anotated (-a flag) tag of v1.0, with the description (-m "text here" flag) "my version v1.0":

``` bash
$ git tag -a v1.0 -m "my version v1.0"
```

Once created, this can be pushed to the master origin branch by:
``` bash
$ git push origin v1.0
```



 