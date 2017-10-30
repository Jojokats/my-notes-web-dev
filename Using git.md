# Using git

These are my notes about using **git** on the command line, and weren't really written with the intention of sharing with others, just my own notes to refer to and track what I did to get myself out of trouble. So I'm sorry if some of the below is a tad confusing...

Once you have git installed and configured, you'll mostly be using a handful of commands:

- `git init` at the start of your project
- `git add .`
- `git status`
- `git commit -am "Commit message goes here"`
- you'll also want to know how to create branches, check out a branch, and merge a branch.
- after that you'll want to know how to collaborate with others using github or bitbucket, and if you have a web project, how to push your changes live to your heroku app.

[Git Essentials](https://tutsplus.com/course/git-essentials/) video on Tuts+

## Setting up **git**
- I probably had it installed already, but it wasn't using the right version:
	`git --version` gave me an old version, and `which git` told me where that version was found on the `$PATH`
	I found the right version (1.8.2.2) in `/usr/local/bin` and edited `.bash_profile` with vim, then ran `source .bash_profile` to reload the bash profile.
- Along the way I found out that Xcode was quite old and wouldn't run on Mountain Lion, so I installed Xcode again. MacPorts gave me errors… but I think Gary installed git on my machine using Homebrew (`brew install git` probably, because I have Homebrew installed, according to `which brew` and `brew --version`) so I didn't bother reinstalling Git then, or trying to make MacPorts work.
- There are 3 config levels:
	1. System git config	: `/etc/gitconfig`
	2. Global (= 'user')		: `~/.gitconfig`
	3. Local config			: `<project>/.git/config` -> By default `git config` stores settings as local (you'll need to be in your project directory.
- So then we need to run:
	- `git config --global user.name "Roberta Voulon"`
	- `git config --global user.email "timoneira@gmail.com"`

## Staging a file in git
- first thing to do when you start a project is to run `git init` inside your project directory, which creates a `.git` folder there
- `git status` to find out which files are tracked or not
- `git add <filename>` to start tracking a file -> we have added a file to the staging area
- **Changes** are committed, not a **file**, so within the same file some changes could be added to the staging area, while other changes not yet.
	+ Actually what this means is that if you stage a file and then continue working on it, the changes made *after* you staged are not going to be committed (unless you commit using `git commit -a`, which will stage all previously staged files with changes)
- `git add .` to add all files in the project to the staging area
- `git add -A` adds everything, including deletions (may be safer than `git add .`)
- `git reset HEAD <filename>` to unstage a file

## Making commits
- `git commit` -> opens vim with a commit message file. Type the commit message and `:wq` as you normally would.
- The commit has a number or *hash*, which is a SHA-1 hash (=Secure Hash Algorithm, duh…) of 40 characters
- `git commit -a` = commit all files that were previously staged (but might not currently be staged). Would ignore new untracked files.
- `git commit -m "here\'s the commit message!"`
- `git commit -am 'message here'`

## Ignoring files
- Locally: use a `.gitignore` file in the project directory
	- list all files or directories in the `.gitignore` file
	- use a bang to ensure it does not ignore a file that would otherwise be ignored
	- for example:
		- `tmp` (temp  folder we created)
		- `test/*.txt` (all *.txt files in a test folder…)
		- `!test/master.txt` (…except master.txt)
- There is also a global `~/.gitignore_global` file, which already ignores `.DS_Store`. However, the first line in that file says `*~`; I’m guessing it will ignore cut off Windows filenames?
- Simply commit a local `.gitignore` file in the project directory so that it becomes the one to be used by all on the project repo
- Use `touch <filename>` to quickly create a file with that name
- Use `rm -r <dirname>/` to remove a directory

## Fixing .gitignore issues
- To untrack a single file that has already been added/initialized to your repository, i.e., stop tracking the file but not delete it from your system use: `git rm --cached filename`
- To untrack every file that is now in your `.gitignore`:
	- First commit any outstanding code changes, and then, run this command:
	- `git rm -r --cached .`
	- This removes any changed files from the index(staging area), then just run:
	- `git add .`
	- Commit it: `git commit -m ".gitignore is now working"`
- To undo `git rm --cached filename`, use `git add filename`.

## Git Theory
- Git is **distributed**, everyone has a complete history and backup of the code
- Staging Area (vs working directory and repository):
	- Repositories on GitHub are "bare clones", and don't have a working directory, no changes are to be made
	- Staging area actually is `.git/index`, which keeps track of what changes we have staged and not staged
		- What's the point of not just committing all changes directly?
			- it's important to make sure that all your commits make sense
			- we work on many things at the same time, and we want to only do commits related to a particular feature or bug
	- **Git repository** is actually the `.git` folder. All changes are stored there.
- Once git repository and the working folder are exactly the same, you have a **clean working directory**
- Git uses a **blob** to store content of a file
	- = a type of git object
	- git uses SHA-1 hashes to store git objects in the git database
	- `.git/objects` are all SHA-1 hashes, compressed using the zlib compression library
- **Tree** is also an object, and is how Git stores directory listings
	- header info
	- then for each file/directory
		- file permissions, object type (blob or tree), SHA-1 hash, file or directory name
	- `git cat-file -p <hash_of_a_tree>` to show the above for a particular tree within our project
- a **commit** is also an object, it's how git stores "snapshots"
	- author information
	- committer information (usually same as author)
	- commit message
	- SHA-1 of any parent commits (commit that was made before this commit -- you can have multiple parents for a commit)
	- SHA-1 of the tree that the commit points to
- **git references**:
	- SHA-1 hash
	- Branches, = reference that points to a particular commit
		- `HEAD` points to the latest commit on the current branch
	- Ancestry refs:
		- with ~:
			- `HEAD` -> Commit 4
			- `HEAD~` -> Commit 3
			- `HEAD~2` -> Commit 2
			- `HEAD~3` -> Commit 1
		- with ^ to get parents of a merged commit
			- `HEAD^` -> Commit 1 on Master Branch
			- `HEAD^2` -> Commit 2 on feature branch
			- where Commit 3 merges feature branch back into the Master branch

## git diff tool
- `git diff`
	- This will return the changes in all modified files (working directory vs index).
- `git diff HEAD`
	- Same, but changes in working directory compared to latest commit.
- `git diff README`
	- compares the current state of the file with the file in the index, so that means it compares the file in working directory to the file as it was staged last time
- `git diff --staged README`
	- compares the staged file with our latest commit
- `git diff HEAD README`
	- compares the file in the working directory with the latest commit

## viewing the log
- `git log`
	- shows a list of all the commits we've done on the project
	- once the log gets long enough, it will page to the window size: use j and k to scroll up and down, and q to quit
- `git log --stat`
	- shows stats for each commit
- `git log --oneline`
	- shows each commit on a single line
- So, the log is where you can quickly find the (abbreviated) hash of a commit, which you can then use to do a diff against what you have in the working directory, like so:
	- `git diff a030b7d TODO`
- `git log --graph`
	- visualizes the branches we have for the project
- `git log --oneline --graph`
	- same as before, but compact
- `git log --pretty="%h, %cn, %cr"`
	- prints out a prettified list with the hash, committer name, and relative commit date (how long ago)
	- list can be found at http://git-scm.com/docs/git-log
- `gitk`
	- brings up a tk gui, which can be used to quickly look at the changes, commits, etc.

## creating branches
- `git branch`
	- returns the list of branches, and highlights the current one
	- if you don't see them and it's a cloned repository, try this to see remote branches:
		- `git remote show origin` where `origin` is the remote
- `git branch <branchname>`
	- creates a new branch, but we stay on the current branch
- `git checkout <branchname>` or `git checkout master`
	- switches to that branch
- You can make changes to the working directory, create new files, and even stage them, then switch branches.
	- If you commit, the changes will be committed to the branch you are then on, NOT to the branch you were on when you made the changes or when you staged the files!
	- Also, if your working directory is clean when you switch, the working directory will exactly represent the HEAD of that current branch. So files will disappear if they did not exist at the last commit to that branch, and will reappear when you switch to a branch where they did exist.
- `git checkout -b <branchname>`
	- creates a new branch and switches to it right away
- `git log --oneline --graph --all --decorate`
	- compact log, graphical view, for all branches, use branch names in fancy colours
- `gitk --all`
	- gitk for all branches
- `git branch -m <old_branchname> <new_branchname>`
	- moves/renames a branch
- `git branch --contains <commit>`
	- lists branches with a specific commit you’re looking for.
- `man git-branch`


## merging branches
- `alias glog="git log --oneline --graph --all --decorate"`
	- create an alias for the pretty git log we saw before
- `alias gl="git log --pretty="%h, %cn, %cr"`
	+ create an alias for the log command that shows you the commit, name and how long ago
- merging a bug fix branch back into the master branch:
	- `git checkout master`
	- `git merge bug-fix-1`
	- editor opens, with default commit message for a merge
	- a commit is made, merging the branch back into master
	- done!
- now you can delete the bug fix branch:
	- `git branch -d bug-fix-1`
	- (use `-D` to delete a branch irrespective of its merged status)
- you can also do a *rebase* instead of a merge:
	- keeps history cleaner and more linear
	- `git rebase experimental-1` (from the master branch)
	- experimental-1 branch will be put back on the master branch
	- "rewinding head to replay your work on top of it…" :) then applies the changes to the master branch
	- now we can safely delete the experimental-1 branch with `git branch -d experimental-1`

## committing changes to a different branch
You made changes in `branch1` but meant to make them to `branch2`. Since your files are not yet committed in `branch1`, here's what you can do commit them in `branch2`

	git stash
	git checkout branch2
	git stash pop

or: 

	git stash
	git checkout branch2
	git stash list       # to check the various stash made in different branch
	git stash apply x    # to select the right one

or even if you're 
Check the [man page](https://www.kernel.org/pub/software/scm/git/docs/git-stash.html)

## Setting up Github
- Create an account, then create a repository
- If you don't yet have an existing git repository that you want to bring onto Github, create a new repository on the command line:
	- `touch README.md`
	- `git init`
	- `git add README.md`
	- `git commit -m "first commit"`
	- `git remote add origin https://github.com/toyb0x/tutsplus.git`
	- `git push -u origin master`
- Push an existing repository from the command line
	- `git remote add origin https://github.com/toyb0x/tutsplus.git`
	- `git push -u origin master`
- **TODO**: [SSH tutorial on nettuts+](http://net.tutsplus.com/tutorials/tools-and-tips/ssh-what-and-how/)
- This SSH stuff seems to not be necessary right now, as I was able to push the existing repository onto github, by entering username and password after the `push` command. But here it is for future reference:
	- `ssh-keygen -t rsa -C 'timoneira@gmail.com'`
	- a private and public key are created in `~/.ssh`
	- `cat` the public key, and paste the whole shebang into the settings for SSH keys on the github site
	- `ssh -T git@github.com`
	- add the passphrase to the keychain, and we're good!

## cloning, pushing and pulling
- `git clone <location> [<new_name>]`
	- creates a clone of another repository
	- automatically creates a remote called origin in the clone
	- but there’s no remote created in the original repository, so if we want to do that:
		- `git remote add <our_clone> <location>`
- `git clone -b <branch_name> <location>`
	+ will clone the repo and then checkout the branch
- After cloning, run `git fetch` without arguments to update remote-tracking branches.
- `git push origin <branch_name>`
	- to push the changes to the original repository. If the branch doesn’t exist, it will be added.
- `git pull origin <branch_name>`
	- to pull the changes made to the branch in the original repository

## clone your heroku app to your local machine
- Install the Heroku Toolbelt
- `$ heroku login`
	- login like usual
- clone the repository using git (clone `<app-name>`'s source code to your local machine)
	- `$ heroku git:clone -a <app-name>`
	- `$ cd glacial-beach-1014`
- Make some changes to the code you just cloned and deploy them to Heroku using Git.
	- `$ git add .` (or `$ git add -A`)
	- `$ git commit -am "make it better"`
	- `$ git push heroku master`

## pushing a local git repository to BitBucket
- `cd /path/to/my/repo`
- `git remote add origin https://toyb0_x@bitbucket.org/toyb0_x/mah.git`
- `git push -u origin --all`
	- pushes up the repo and its refs for the first time
- `git push -u origin --tags`
	- pushes up any tags

## fix your remote urls (fetch and push)
- You can configure the remotes manually in `.git/config`
- `git remote`
	- list the remotes that you have set up
- `git remote -v`
	- same, but more verbose: lists the urls for each remote, both fetch and push.
- `git remote show [remote-name]`
	- lists the url for the remote, as well as tracking information for the different branches.
- `git remote rename [old-name] [new-name]`
	- changes the name of the remote
- `git remote rm [remote-name]`
	- removes a remote

## Collaborative Coding with Git - workshop
- wrong way of getting a repository from a remote (on top of our work)
```
git checkout master
git pull origin master
# this will merge your history with the master as if it's happening before the master's history
```

- The right way:
```
git checkout master
git checkout -b my-master
git branch -D master
git fetch
git checkout origin/master
git checkout -b master
git merge my-master
```

- dirty way: merging before doing a pull request
```
git checkout your-branch-that-has-changes
git fetch citrondigital
git merge citrondigital/participants
git push origin your-branch-that-has-changes
```

- proper way: rebase before doing a pull request
```
git checkout your-branch-that-has-changes
git fetch citrondigital
git rebase citrondigital/participants
# fix your changes, follow rebase instructions
git add
git rebase --continue
git push -f origin your-branch-that-has-changes
```

- `git rebase -i HEAD~5`
	- rebases the last 5 commits
	- in your commit message you can then replace `pick` on each line with `r` for reword and `s` for squash. Then change the commit message for the line that you 'reworded'.
- `git reflog`
	- go back in time after doing a wrong rebase... you can checkout the commit right before your rebase:
	- `git checkout HEAD@{3}`

# Fixing commit mistakes
- You committed, but forgot to stage one of the files:
	+ stage the file with `git add`
	+ `git commit --amend`
	+ check your changes with `git log --stat`
- You did a commit that you want to un-commit without losing your changes
	+ make a copy of the project folder...
	+ `git reset --soft HEAD^`