# Git Tricks with Rob

```bash
# Show config
git config -l

# Show origin tracking 
git remote show origin


https://stackoverflow.com/questions/6591213/how-do-i-rename-a-local-git-branch


# To rename a branch while pointed to any branch:
git branch -m <oldname> <newname>
To rename the current branch:

# Rename local branch
git branch -m <newname>



# ssh key login
ssh -T git@github.com

# remote 
git branch -r

git remote update # get's all new remote branches; fetch for all versus 1 remote

git hist upstream


# 
git checkout upstream/cervix_fgs -- <path to file name - relative>


# Unstage (un-add) back to git branch creation if this is a new branch
git reset HEAD <path to file name - relative>
```