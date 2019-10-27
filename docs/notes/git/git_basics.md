# Git Basics

> Courtesy of [rogerdudler.github.io](https://rogerdudler.github.io/git-guide/)

## Create a Repo

Create a new repo with ```git init```:

```bash
bbearce@bbearce-XPS-15-9560:~/Desktop/git_practice$ git init
Initialized empty Git repository in /home/bbearce/Desktop/git_practice/.git/

```

## Checkout a Repo

Checkout a repo with ```git clone```:

```bash
$ git clone /path/to/repository
```
When using a remote server, your command will be:

```bash
git clone username@host:/path/to/repository
```

## Workflow

Your local repository consists of three "trees" maintained by git. the first one is your ```Working Directory``` which holds the actual files. The second one is the ```Index``` which acts as a staging area and finally the ```HEAD``` which points to the last commit you've made.

![Basic Git Workflow](/notes/git/git_trees.png)

## Add and Commit

You can propose changes (add it to the Index) using:

```bash
git add <filename>
```
or
```
git add *
```

This is the first step in the basic git workflow. To actually commit these changes use:

```bash
git commit -m "Commit message"
```

Now the file is committed to the ```HEAD```, but not in your remote repository yet.

> ```git remote -v``` will tell you which remote you are connected to.

```
SYNOPSIS
       git remote [-v | --verbose]

...

OPTIONS
       -v, --verbose
           Be a little more verbose and show remote url after name. NOTE: This must be placed between remote and subcommand.
```

## Adding a New Remote

> Courtesy of [articles.assembla.com](https://articles.assembla.com/en/articles/1136998-how-to-add-a-new-remote-to-your-git-repo#targetText=To%20add%20a%20new%20remote%2C%20use%20the%20git%20remote%20add,tab%20of%20your%20Git%20repo)

To add a new remote, use the ```git remote add``` command on the terminal, in the directory your repository is stored at.

The git remote add command takes two arguments:

* A remote name, for example, “origin”  
* A remote URL, which you can find on the Source sub-tab of your Git repo  


```bash
#set a new remote
git remote add origin git@git.assembla.com:portfolio/space.space_name.git

#Verify new remote
git remote -v

origin  git@git.assembla.com:portfolio/space.space_name.git (fetch)
origin  git@git.assembla.com:portfolio/space.space_name.git (push)
```


## Pushing Changes

Your changes are now in the ```HEAD``` of your local working copy. To send those changes to your remote repository, execute:

```bash
git push origin master
```

Change master to whatever branch you want to push your changes to.

If you have not cloned an existing repository and want to connect your repository to a remote server, you need to add it with:
```bash
git remote add origin <server>
```
Now you are able to push your changes to the selected remote server.

## Push Without User:Pass

> Courtesy of [medium.com](https://medium.com/@amanze.ogbonna/accessing-pushing-to-github-without-username-and-password-3022feb077fb)

A way to skip typing my username/password when using https://github, is by changing the HTTPs origin remote which pointing to an HTTP url into an SSH url.

For example:

https url: ```https://github.com/<Username>/<Project>.git```  
ssh url: ```git@github.com:<Username>/<Project>.git```  

You can do:
```bash
git remote set-url origin git@github.com:<Username>/<Project>.git
```
to change the url.

> You need to have an ssh key pair generated and added to github

Steps:  
1. [Generate Key Pair](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key)  
2. [Add public key to git repo](https://developer.github.com/v3/guides/managing-deploy-keys/#deploy-keys)  




## Branching

Branches are used to develop features isolated from each other. The master branch is the "default" branch when you create a repository. Use other branches for development and merge them back to the master branch upon completion.

![Branching Image](/notes/git/branches.png)

Create a new branch named "feature_x" and switch to it using:

```git checkout -b feature_x```

Switch back to master:

```git checkout master```

Delete the branch again:

```git branch -d feature_x```

A branch is not available to others unless you push the branch to your remote repository:

```git push origin <branch>```


## Update and Merge

To update your local repository to the newest commit, execute:

```git pull```

In your working directory to fetch and merge remote changes. To merge another branch into your active branch (e.g. master), use:

```git merge <branch>```

In both cases git tries to auto-merge changes. Unfortunately, this is not always possible and results in conflicts. You are responsible to merge those conflicts manually by editing the files shown by git. After changing, you need to mark them as merged with:

```git add <filename>```

Before merging changes, you can also preview them by using:

```git diff <source_branch> <target_branch>```

## Tagging

It's recommended to create tags for software releases. This is a known concept, which also exists in SVN. You can create a new tag named 1.0.0 by executing:

```git tag 1.0.0 1b2e1d63ff```

The 1b2e1d63ff stands for the first 10 characters of the commit id you want to reference with your tag. You can get the commit id by looking at the...log.

## Log

In its simplest form, you can study repository history using:

```git log```

You can add a lot of parameters to make the log look like what you want. To see only the commits of a certain author:

```git log --author=bob```

To see a very compressed log where each commit is one line:

```git log --pretty=oneline```

Or maybe you want to see an ASCII art tree of all the branches, decorated with the names of tags and branches:

```git log --graph --oneline --decorate --all```

See only which files have changed:

```git log --name-status```

These are just a few of the possible parameters you can use. For more, see ```git log --help```

## Replace Local Changes

In case you did something wrong, which for sure never happens ;), you can replace local changes using the command:

```git checkout -- <filename>```

This replaces the changes in your working tree with the last content in ```HEAD```. Changes already added to the index, as well as new files, will be kept.

If you instead want to drop all your local changes and commits, fetch the latest history from the server and point your local master branch at it like this:

```bash
git fetch origin
git reset --hard origin/master
```

## Useful Hints

If you want a built-in git GUI, use:

```gitk```

Use colorful git output:

```git config color.ui true```

To show log on just one line per commit, use:

```git config format.pretty oneline```

Use interactive adding:

```git add -i```

