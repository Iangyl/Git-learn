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

## Viewing your staged and unstaged changes

If the `git status` command is too vague for you — you want to know exactly what you changed, not just which files were changed — you can use the `git diff` command. `git diff` shows you the exact lines added and removed — the patch, as it were.

`git diff` - compares your commited files with their modified version.

`git diff --staged` - compares your staged changes to your last commit.

`git diff --cached` - to see what you’ve staged so far (--staged and --cached are synonyms)

## Removing files

To remove file from Git, you have to remove it from your tracked files and then commit. The `git rm` command does that, and also removes the file from your working directory so you don't see it as an untracked file the next time around.

For example:
```
$ rm PROJECTS.md
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

            deleted:    PROJECTS.md

	    no changes added to commit (use "git add" and/or "git commit -a")
```

```
$ git rm PROJECTS.md
rm 'PROJECTS.md'
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

      deleted:    PROJECTS.md
```

Another useful thing you may want to do is to keep the file in your working tree but remove it from your staging area. In other words, you may want to keep the file on your hard drive but not have Git track it anymore. This is particularly useful if you forgot to add something to your .gitignore file and accidentally staged it, like a large log file or a bunch of .a compiled files. To do this, use the --cached option:

```
$ git rm --cached README.md
```

## Viewing the commit history

```
$ git log
```
#### Common options to git log

|Option | Description|
|-------|------------|
|`-p`   |Show the patch introduced with each commit.|
|-------|------------|
|`--stat`|Show statistics for files modified in each commit.|
|-------|------------|
|`--shortstat`|Display only the changed/insertions/deletions line from the --stat command.|
|-------|------------|
|`--name-only`|Show the list of files modified after the commit information.|
|-------|------------|
|`--name-status`|Show the list of files affected with added/modified/deleted information as well.|
|-------|------------|
|`--abbrew-commit`|Show only the first few characters of the SHA-1 checksum instead of all 40.|
|-------|------------|
|`--relative-date`|Display the date in a relative format (for example, “2 weeks ago”) instead of using the full date format.|
|-------|------------|
|`--graph`|Display an ASCII graph of the branch and merge history beside the log output.|
|-------|------------|
|`--pretty`|Show commits in an alternate format. Option values include oneline, short, full, fuller, and format (where you specify your own format).|
|-------|------------|
|`--oneline`|Shorthand for --pretty=oneline --abbrev-commit used together.|
|-------|------------|

##### Useful specifiers for git log --pretty=format

|Specifier | Description of output   |
|----------|-------------------------|
|`%H`      |Commit hash              |
|----------|-------------------------|
|`%h`      |Abbreviated commit hash  |
|----------|-------------------------|
|`%T`      |Tree hash                |
|----------|-------------------------|
|`%t`      |Abbreviated tree hash    |
|----------|-------------------------|
|`%P`      |Parent hashes            |
|----------|-------------------------|
|`%p`      |Abbreviated parent hashes|
|----------|-------------------------|
|`%an`     |Author name              |
|----------|-------------------------|
|`%ae`     |Author email             |
|----------|-------------------------|
|`%ad`     |Author date              |
|----------|-------------------------|
|`%ar`     |Author date, relative    |
|----------|-------------------------|
|`%cn`     |Committer name           |
|----------|-------------------------|
|`%ce`     |Committer email          |
|----------|-------------------------|
|`%cd`     |Committer date           |
|----------|-------------------------|
|`%cr`     |Committer date, relative |
|----------|-------------------------|
|`%s`      |Subject                  |
|----------|-------------------------|

## Unstaging a staged file

```
$ git add *
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

      renamed:    README.md -> README
          modified:   CONTRIBUTING.md
```

Right below the “Changes to be committed” text, it says use git reset HEAD <file>... to unstage. So, let’s use that advice to unstage the CONTRIBUTING.md file:

```
$ git reset HEAD CONTRIBUTING.md
Unstaged changes after reset:
M	CONTRIBUTING.md
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

      renamed:    README.md -> README

      Changes not staged for commit:
        (use "git add <file>..." to update what will be committed)
	  (use "git checkout -- <file>..." to discard changes in working directory)

	      modified:   CONTRIBUTING.md
```

## Git tagging

Example:
```
$ git tag
v1.0
v2.0
```

### Creating tags

Git supports two types of tags: lightweight and annotated.

A lightweight tag is very much like a branch that doesn’t change — it’s just a pointer to a specific commit.

Annotated tags, however, are stored as full objects in the Git database. They’re checksummed; contain the tagger name, email, and date; have a tagging message; and can be signed and verified with GNU Privacy Guard (GPG). It’s generally recommended that you create annotated tags so you can have all this information; but if you want a temporary tag or for some reason don’t want to keep the other information, lightweight tags are available too.

### Annotated tags

Creating an annotated tag in Git is simple. The easiest way is to specify -a when you run the tag command:
```
$ git tag -a v1.4 -m "my version 1.4"
$ git tag
v0.1
v1.3
v1.4
```

You can see the tag data along with the commit that was tagged by using the git show command: `git show v1.3`.

### Lightweight tags

Another way to tag commits is with a lightweight tag. This is basically the commit checksum stored in a file — no other information is kept. To create a lightweight tag, don’t supply any of the -a, -s, or -m options, just provide a tag name:

```
$ git tag v1.4-lw
$ git tag
v0.1
v1.3
v1.4
v1.4-lw
v1.5
```

### Sharing tags

By default, the git push command doesn’t transfer tags to remote servers. You will have to explicitly push tags to a shared server after you have created them. This process is just like sharing remote branches — you can run git push origin <tagname>.

```
$ git push origin v1.5
Counting objects: 14, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (12/12), done.
Writing objects: 100% (14/14), 2.05 KiB | 0 bytes/s, done.
Total 14 (delta 3), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.5 -> v1.5
```
If you have a lot of tags that you want to push up at once, you can also use the --tags option to the git push command. This will transfer all of your tags to the remote server that are not already there.
```
$ git push origin --tags
Counting objects: 1, done.
Writing objects: 100% (1/1), 160 bytes | 0 bytes/s, done.
Total 1 (delta 0), reused 0 (delta 0)
To git@github.com:schacon/simplegit.git
 * [new tag]         v1.4 -> v1.4
  * [new tag]         v1.4-lw -> v1.4-lw
```

### Deleting tags

To delete a tag on your local repository, you can use git tag -d <tagname>. For example, we could remove our lightweight tag above as follows:

```
$ git tag -d v1.4-lw
Deleted tag 'v1.4-lw' (was e7d5add)
```
remote:
```
$ git push origin --delete <tagname>
```

