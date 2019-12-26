
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
`$ git branch <feature-branch>	// it uses the current HEAD as the starting point for the new branch`

### Delete a branch
$ git branch -d <feature-branch>	// -D would force the removal of an unmerged branch

### List remote branches
$ git branch -r

### See all branches
`$ git branch -a`

### Rename a branch
`$ git branch -m <oldname> <newname>`
`$ git branch -m <newname>			// rename current branch`


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
  if [ -n "$1" ]; then
        git checkout -b $1 && git submodule foreach "git checkout -b $1"
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

#nicer git log
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

