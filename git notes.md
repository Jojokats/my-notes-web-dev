1 - cd to directory
  - make sure you have created a .gitignore file
2 - git init
3 - ls -al
4 - Create a repo and copy URL
5 - We need to sync our remote repo to our local repo
6 - run: git remote add origin (URL.....)
7 - git status
8 - git add .
9 - git status
10 - git commit -m "....."
11 - git push origin master
12 - refresh github page


# Setting up the _____ project


## Creating a branch from master
	git clone --branch master
  * Ex: https://github.com/RichardLitt/the-travel-shelf.git

## Adding the directory to the project with git
  git add app

## To stage files for tracked files for a commit
	git commit -m "Initial commit”

## Create new branch on github website based on master

## Pull changes from remote
	git pull

## To switch to another branch:
	git checkout homepage

## When ready to finish my work,
	      1st- merge changes from origin master
	git pull

	git merge origin/master


## Now, I need to make my origin page branch current by pushing these changes from local to it:
	   git push

Now, that homepage branches (local & origin) are in sync with the master branch,
I can open my pull request in github.

On Branch homepage, I made changes to index.html, then ran in terminal:

	git status
	git add .  (for all)
	git commit -m "Added navbar and Hero section"
	git push



### Info:

Created index.html and added navbar and hero section

	1.	Rename your local branch. If you are on the branch you want to rename: git branch -m new-name. ...
	2.	Delete the old-name remote branch and push the new-name local branch. git push origin :old-name new-name.
	3.	Reset the upstream branch for the new-name local branch. Switch to the branch and then: git push origin -u new-name.

Created index.html and added navbar and hero section

  git merge origin/master
  git pull
  git merge origin/master
  git status
  git push
  git status
  git pull
  git merge origin/master
  ls -al
  git branch
