## Alternate approach to create a new branch:

1 - Go to Master on Github, under the Code tab
	https://github.com/RichardLitt/the-travel-shelf

2 - click the Branch field and type a new branch name like so:
ex:  feat/navigation-padding

## The new branch will be created but we need to pull that information locally, it does not matter what the current branch is:
Git pull
[new branch]   feat/remove-links -> upstream/feat/remove-links

3 - Switch to this new branch in terminal:
	git checkout feat/………………..


4 - Start working on the files, like index.html to make changes

5 -
	git status
	git add .  (for all)
	git commit -m "Added navbar and Hero section"
	git push

6- go to Travel Shelf github
https://github.com/RichardLitt/the-travel-shelf

	* Click Compare & pull request (on far right)

7 - Write description of what you did in dialogue box.

# To Make a global .gitignore:

* Enter this command in terminal once:
git config --global core.excludesfile ~/.gitignore_global
now it’s done, no need to redo this on your system

## To edit it in vi from terminal:
cd ~ (only for vi)

## Use text editor and open file.
	- Open files
	- CMD Shift . (dot) to display hidden files
	- look for .gitignore_global

##	Enter the files to be ignored:
		*~
		.sass-cache
		style.css.map
		.DS_Store
******************


