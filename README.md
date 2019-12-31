
## Introduction

**_head_** - to define what is currently checked out: 
- If you checkout a branch, Head symbolically refers to that branch, indicating that the branch name should be updated after the next commit operation.
- If you checkout a specific commit, Head refers to that commit only. is is referred to as a detached head, and occurs, for example, if you check out a tag name.	 

**_tag_** - A tag is also a name for a commit, similar to a branch, except that it always names the same commit, and can have its own description text

**_branch_** - a branch is just a name for a commit, also called a reference (a branch is nothing more than a named reference to a commit). It’s important to realize that Git uses the tip of a branch to represent the entire branch. That is to say, a branch is actually a pointer to a single commit—not a container for a series of commits.

**_working tree_** - a working tree is any directory on your filesystem which has repository associated with it (typically a subdirectory named .git). It includes all the files and subdirectories in that directory.
commit - A commit is a snapshot of your working tree at some point in time. The state of head at the time your commit is made becomes that commit’s parent. This is what creates the notion of a “revision history”.

**_the index_** - Git does not commit changes directly from the working tree into the repository. Instead changes are first registered in something called the index. Think of it as a way of “confirming” your changes, one by one, before doing a commit (which records all your approved changes at once). Some find it helpful to call it as the “staging area”. In reality, a checkout causes contents from the repository to be copied into the index, which is then written out to the working tree.

**_repository_** - A repository is a collection of commits, each of which is an archive of what the project’s working tree looked like at a past date, whether on your machine or someone else’s. It also defines head which identifies the branch or commit the current working tree stemmed from. Lastly, it contains a set of branches and tags, to identify certain commits by name.

**_blob_** - The basic data storage is the blob. The contents of your files are stored in blobs. The contents are then referenced by a 40 character SHA1 hash of the contents, which means it’s pretty much guaranteed to be unique. Pretty much every object, be it a commit, tree, or blob has a SHA. Luckily, they’re easily referenced by the first 7 characters which are usually enough to identify the whole hash.

**There are many ways to name/refer to commits:**

_branchname_ - The name of any branch is simply an alias for the most recent commit on that “branch”. This is same as using the word HEAD whenever that branch is checked out.

_tagname_ - tag aliases never change, whereas branch aliases change each time a new commit is checked out into that branch.

_HEAD_ - The currently checked out commit is always called HEAD. If you check out a specific commit - instead of a branch name - then HEAD refers to that commit only and not to any branch (also called detached HEAD).

_name^_ - The parent of any commit. If a commit has more than one parent, the first is used.

_name^^_ - The parent of the parent of the given commit name.

_name^2_ - If a commit has multiple parents (such as a merge commit), you can refer to the nth parent using name^n.

_name~10_ - A commit nth ancestor may be referenced using a tilde followed by the ordinal number.

_name:path_ - To reference a certain file, specify that file’s name after a colon e.g. to show the difference between two version of a committed file:
 `$ git diff HEAD^1:Makefile HEAD^2:Makefile`

_name1..name2_ - All the commits reachable from name2 back to, but not including name1. If either name1 or name2 is omitted, HEAD is used it its place.

_name1...name2_ - All the commits referenced by name1 or name2, but not by both. The result is then a list of all the unique commits in both branches. 

_master.._ - equivalent to "master..HEAD"

_..master_ - equivalent to "HEAD..master"

_--since="2 weeks ago"_

_--until="1 week ago"_

_--grep=pattern_ - refers to all commits whose commit message matches the regular expression pattern.

_--author=pattern_

_--no-merges_ - all commits in the range that have only parent i.e. it ignores all merge commits

Most of these options can be mixed and matched. Here is an example which shows the changes made to the current branch (branched from master), by johnw, within the last month, which contain the text "foo":

 `$ git log --grep='foo' --author='johnw' --since="1 month ago" master..`

An untracked file is one that is not under version control. Git doesn’t automatically track files because there are often project files that we don’t want to keep under revision control e.g. binary files.

Never, ever rebase commits that have been pushed to a shared repository.

**Fundamental Git workflow:** edit files, stage a snapshot, and commit the snapshot.


## git-reset

### Unstage all files but leave their content as it is
`$ git reset`				without specifying a flag it defaults to `git reset --mixed`

### Unstage files from index and reset pointer to HEAD
`$ git reset HEAD <file>`		`<file>` is optional

### Move HEAD to specified commit reference, index and staging are untouched
`$ git reset --soft HEAD~1`		the committed changes are put back onto stage

### Unstage files and undo any changes in the working directory since last commit
`$ git reset --hard`			hard flag is what update the files (careful: permanent undo on uncommitted changes)

### Move HEAD to specified commit reference, without preserving local changes
`$ git reset --hard HEAD~1`

## git-log

### See messages in commit history & nothing else
`$ git log --oneline	--no-merges` --no-merges to remove merge commits

### See commit history of a specific file
`$ git log --oneline filename`

### See commit history of a specific directory
`$ git log -- <directory-path>`

### Display the commits reachable from `<until>` but not from `<since>`
`$ git log <since>..<until>` These parameters can be either commit ID’s or branch names

![alt text](https://github.com/tabishfayyaz/git-cheat-sheet/raw/master/images/gitlog.png "git log")

### Include extra information about altered files in the log output
`$ git log --stat`

### See commit history for the last two commits
`$ git log -2`

### See commit history for the last two commits, with diff
`$ git log -p -2`
### Show only names and status of changed files.
`$ git log --name-status`
### Look for only commits from specific author
`$ git log --author=<name>`

### Filter commits by date committed
`$ git log --since --before		//--since "1 week ago", --since "1st October 2016", --since "1 day" `

### Filter commits by commit message
`$ git log --grep=<string> --format="%h %an %s"`	the arguments are an OR relationship, use `--all-match` for AND, format specifier glossary in resources	

### Look for differences that change the number of occurrences of the specified string
`$ git log -S<string>`		there is no '=' or space/colon between the `-S` and the string we are searching for

## git-branch

### Create a branch
`$ git branch <feature-branch>`	// it uses the current HEAD as the starting point for the new branch

### Delete a branch
`$ git branch -d <feature-branch>`	// -D would force the removal of an unmerged branch

### List remote branches
`$ git branch -r`

### See all branches
`$ git branch -a`

### Rename a branch
`$ git branch -m <oldname> <newname>`

`$ git branch -m <newname>`			// rename current branch

## git-diff

### View the difference between the working directory and the staging area
`$ git diff`		// to see how a project has changed since a known point in past

### View the difference between the staging area and the most recent commit
```
$ git diff --cached
$ git diff --cached <path-to-file>
```

![alt text](https://github.com/tabishfayyaz/git-cheat-sheet/raw/master/images/git-diff.png "git diff")

### See differences between the current state and a branch
`$ git diff branch-name`

### See differences in a file, between the current state and a branch
`$ git diff branch-name path/to/file`

### See the files changed between two commits
`$ git diff --name-only <commit_id1> <commit_id2>`	// same as specifying with two dots

### View the difference between two commits
`$ git diff <commit-id1>...<commit-id2>`	// to see what unique work is in one branch, note there are 3 dots:

![alt text](https://github.com/tabishfayyaz/git-cheat-sheet/raw/master/images/git-diff2.png "git diff")

## git-checkout

### View a previous commit
`$ git checkout <commit-id>`

### Revert an individual file to match the specified commit without switching branches
`$ git checkout <commit-id> <file>		// the <commit-id> could be a stash reference e.g. stash@{0}`

### Create new branch
`$ git checkout -b <branch-name>`

### Change file back to last commit 
`$ git checkout -- <file-name>`

### Checkout file in a conflicted state
```
$ git checkout --theirs <optional-file-name>
$ git checkout --ours <optional-file-name>
```

## git-commit

### Add staged changes to the most recent commit instead of creating a new one
`$ git commit --amend`

### Stage tracked files and commit them
`$ git commit -a -m “<message>”`	// untracked files are not committed

## git-rebase

### Move the current branch’s commits to the tip of branch-name
`$ git rebase <branch-name>`		// rebasing lets us move branches around by changing the commit they are 
                              based on

### Perform an interactive rebase and select actions for each commit
`$ git rebase -i <new-base>`		// rebasing also allows us to manipulate individual commits

### Continue a rebase after amending a commit
`$ git rebase --continue`

### Abandon the current interactive rebase and return the repository to its former state
`$ git rebase --abort`

## git-merge

### Merge a branch into the checked-out branch
`$ git merge <feature-branch>`	// fast-forward merge: keeps changeset in a linear history, instead of 
                               creating a new commit to represent the merge, git will just point master to the latest commit  
                               of the feature branch 

### Force a merge commit even if git could do a fast-forward merge
`$ git merge --no-ff <branch-name>`

### Merge a remote branch into the checked-out branch
`$ git merge <remote-name>/<branch-name>`

## Fast-Forward Merge

Merging css branch into master. Git does a fast-forward merge (didn’t add any new commits) since it is possible to “fast forward” through the new commits in the css branch:

![alt text](https://github.com/tabishfayyaz/git-cheat-sheet/raw/master/images/merge1.png "git merge")

Fast-forward merges are not reflected in the project history. This is the tangible distinction between fast-forward merges and 3-way merges. 

### 3-Way Merge

Merging master into crazy branch. Git does a 3-way merge (create an extra commit to merge two branches whose history has diverged as it is not possible to “fast forward” through the new commits). Git looks at three commits (numbered in the second figure below) to generate the merge commit. As a result, the merge commit has two parent commits and the crazy branch has access to both its history and the master history.

![alt text](https://github.com/tabishfayyaz/git-cheat-sheet/raw/master/images/merge2.png "git merge")

## git-remote

### List remote repositories
`$ git remote`

### Remove the specified remote from your bookmarked connections
`$ git remote rm <remote-name>`

### Add a remote repository
`$ git remote add <remote-name> <remote-path>`

### View remote urls
`$ git remote -v`

### Change origin url
`$ git remote set-url origin http//github.com/repo.git`

## git-push

### Push a local branch to another repository
`$ git push <remote-name> <branch-name>`	  // An important property of git push is that it does not
                                           automatically push tags 
### Push a tag to another repository
`$ git push <remote-name> <tag-name>`

### Delete remote branch
`$ git push <origin> <branch-name>`

## git-stash

### Temporarily stash changes to create a clean working directory
`$ git stash`

### Re-apply stashed changes to the working directory
`$ git stash apply`

### Remove all local changes from working copy
```
$ git stash save --keep-index
$ git stash drop
```

### Please, commit your changes or stash them before you can merge
```
$ git stash
$ git stash pop
```

## git-config

### Create a shortcut for a command and store it in the global configuration file
`$ git config --global alias.<alias-name> <git-command>`

### Set your details	
`$ git config --global user.name "John Doe"`	 // if git config is used without --global the settings are 
                                              set for the specific project.

`$ git config --global user.email "john@example.com"`

### See your settings
$ git config --list

## git-show

### See the file names changed in a specific commit
`$ git show --name-only COMMIT_ID`

### See details (log message, text diff) of a commit
`$ git show COMMIT_ID`

## git-clean

### Remove untracked files
`$ git clean -f` (careful: permanent undo on uncommitted changes)

### Including directories:
`$ git clean -f -d`

### Dry run:
`$ git clean -n -f -d`


## git-fetch

### Get all branches
`$ git fetch origin`

### Download remote branch information, but do not merge anything
`$ git fetch <remote-name>`		 // fetching and pushing are almost opposites, in that fetching imports 
                              branches, while pushing exports branches to another repository. Fetch won’t merge your changes 
                              unlike pull


## git-config
```
<repo>/.git/config 		// repo specific settings

$ git config --global merge.tool filemerge
$ git config --global color.ui true
$ git config --global core.editor vim
```

### Edit the git config file
`$ git config --global --edit`

### Setting your email, user in git
```
$ git config --global user.email <email-id>
$ git config --global user.name <firstname lastname>
```

## Miscellaneous

### See commit history for just the current branch
`$ git cherry -v <branch-name>`	// branch-name is the branch you want to compare with

### Remove from repository all locally deleted files
`$ git rm $(git ls-files --deleted)`

### Remove a file from staging area and also off your disk (the working directory)
`$ git rm <file-name>`

### Display the local, chronological history of a repository
`$ git reflog`

### Create a clone of a remote Git repository
`$ git clone <remote-path>`

### Create an annotated tag pointing to the most recent commit
`$ git tag -a <tag-name> -m "<description>"`

### Undo this commit by applying a new commit
`$ git revert 91f42ec`		// the unique hash is of the commit we want to undo (not restore)

### Undo last commit
`$ git reset --soft HEAD~1`  // changes in undone revisions are preserved

`$ git reset --hard HEAD~1`  // changes are not preserved 

### Look for conflicts in your current files
```
$ grep -H -r "<<<" *
$ grep -H -r ">>>" *
$ grep -H -r '^=======$' *
```

### Remove files that have been deleted
`$ git add -u`

### Show branches history and their commits
`$ git show-branch`

### Find common ancestor for a merge 
`$ git merge-base feature master`

### Get help for a specific git command
`$ git help clone`

### Merge selected commits into current branch
`$ git cherry-pick <hash-1> <hash-2> <hash-3>`

### Take a branch version for submodule reference e.g. when git rebase or git merge causes conflict
`$ git reset <master> path/to/submodule`

## Sample .gitconfig file
```
[color]
       ui = true
[alias]
 s = status
 b = branch -a -v
 d = diff
 r = remote -v
 f = fetch
 gd = diff
 ss = submodule status
 su = submodule update
 plog = log --format='%C(red) %h %C(green) %ad %C(yellow) %an %C(black) %s %C(red bold) %D %C(reset)'
 forall = "!f(){ git "$@" && git submodule foreach git "$@"; }; f"      # where $@ is any argument passed 
                                                                        to forall e.g. $git forall status will run it on all       
                                                                        including submodules      
 graph = log --graph --branches --remotes --tags  --format=format:'%Cgreen%h %Creset• %<(75,trunc)%s (%cN, %cr) %Cred%d' --date-order

 conflicts = diff --name-only --diff-filter=U
    
[user]
       name = <firstname> <lastname>
       email = <firstname.lastname@email.com>

```

## Sample bash file for git

```
alias h='history'
alias hg='history | grep'
alias gs='git status'
alias gb='git branch'
alias gd='git diff'

function gfo(){
        git fetch origin && git submodule foreach "git fetch origin"
}

function gco() {
  if [ -n "$1" ]; then
        git checkout $1 &&  git submodule foreach "git checkout $1"
  fi
}

#create branch
function gcb() {
  if [ "$#" -eq 2 ]; then
        git checkout -b $1 $2 && git submodule foreach "git checkout -b $1 $2"
  else
    echo "Missing 2nd argument: name of branch to base new branch upon"
  fi
}

#delete branch
function gdb() {
  if [ -n "$1" ]; then
        git branch -d $1 && git submodule foreach "git branch -d $1"
  fi
}

function gpull() {
  if [ -n "$1" ]; then
        git pull $1 &&  git submodule foreach "git pull $1"
  fi
}

function gpush() {
  if [ -n "$1" ]; then
        git push $1 &&  git submodule foreach "git push $1"
  fi
}

function gmb() {
  if [ -n "$1" ]; then
        git merge $1 &&  git submodule foreach "git merge $1"
  fi
}

function gl(){
        if [ -n "$1" ]; then
                git log --format="%C(red) %h %C(green) %ad %C(yellow) %an %C(black) %s %C(red bold) %D %C(reset)" -n $1
        else
                git log --format="%C(red) %h %C(green) %ad %C(yellow) %an %C(black) %s %C(red bold) %D %C(reset)"
        fi
}
```

## Resources
- http://gitref.org/index.html
- http://rypress.com/tutorials/git/index
- https://orga.cat/posts/most-useful-git-commands
- https://www.kernel.org/pub/software/scm/git/docs/user-manual.html
- http://ndpsoftware.com/git-cheatsheet.html#loc=workspace
- http://gitready.com/
- http://rogerdudler.github.com/git-guide/
- http://www-cs-students.stanford.edu/~blynn/gitmagic/ch02.html
- git rebase: https://www.atlassian.com/git/tutorials
- git tags: http://alblue.bandlem.com/2011/04/git-tip-of-week-tags.html
- format specifiers: https://www.kernel.org/pub/software/scm/git/docs/git-log.html#_pretty_formats
- human git aliases: http://gggritso.com/human-git-aliases
- https://www.codeschool.com/learn/git
- http://learngitbranching.js.org/
- https://try.github.io/levels/1/challenges/1

