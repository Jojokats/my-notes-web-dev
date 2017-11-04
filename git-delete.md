## To Delete a file:

1- make sure you’re in the right branch
	feat/worldmap-proposal

2- git status
	On branch feat/worldmap-proposal
	Your branch is up-to-date with ‘upstream/feat/worldmap-proposal'.

3- delete file from folder in Finder, if you delete via terminal, it’s permanent. If you delete via Finder, it  puts it in the trash.

4- deleted:  img/worldmap.jpg
shows up in red

## Your upstream repository doesn’t know about this change, therefore you need to add, commit, push

5- git add img/worldmap.jpg

6- git commit -m "Deleted worldmap.jpg”

7- git push
