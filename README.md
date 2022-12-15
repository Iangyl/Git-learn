# Git learning repo

The main goal of this repo is to get clear understanding some of the commands and learning new (for me, of course) functionality.
Every time I will add something new here to make change in the repo and describe it here

## Short status

`git status -s` this line of code give us a shorted statuses of each file. 
For example:
```
$ git status -s
 M README
 MM Rakefile
 A  lib/git.rb
 M  lib/simplegit.rb
 ?? LICENSE.txt
```

M - modified
MM - modified -> staged -> modified again. For my opinion, this "short status" is deprecated and now Git use AM instead.
A - staged
?? - untracked
