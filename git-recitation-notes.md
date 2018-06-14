# Git Lunch and Learn Notes #

ME: Talk about the approach to this lunch and learn

- What is git?
    - Git is a tool to track your code and its history of changes
    - It uses a fully decentralized model
        - You can make changes independent of other developers
        - You can use all features of git, regardless of network connectivity
    - It makes experimenting with new features and bug fixes easy
        - Branches and their flexibility makes it easy to experiment
    - It helps to protect you from yourself or others in a collaborative environment.
    - It's important to understand that git tracks _changes_, not files.

## Making a Repository ##

- A "repository" is all the folders and files you want git to track
- Git maintains a record of all this information in the `.git` folder, at the top-most folder (the root) of the repository.

```
$ mkdir -p ~/Documents/repos/lunch-and-learn

$ cd ~/Documents/repos/lunch-and-learn

$ git init
Initialized empty Git repository in lunch-and-learn/.git/
```

- We made a directory called `lunch-and-learn`, and told git to start tracking changes inside of it

- Run `git status` to look at the state of the repository

## Tracking Changes ##

- Git tracks change5 made to files

```
$ echo "hello world" > my_file.txt

$ git add my_file.txt

```

- Run `git status` to look at the state of the repository

- We made a file called `my_file.txt` and wrote "hello world" to it.
- We told git, through `git add`, to track the change to this file, and any future changes to it.
- After we've added all changes we want to track, we save them, which git calls "committing"

```
$ git commit -m "I made a new file"
[master (root-commit) 08bdc28] I made a new file
 1 file changed, 1 insertion(+)
 create mode 100644 my_file.txt
```

- Run `git status` to look at the state of the repository

## Inspecting the Repository ##

- Now that we have a commit, we can look at the history of the repository:

```
$ git log
```

- press "q" to quit

## Correcting (small) Mistakes ##

- If you want to modify the previous commit (you forgot to include a file, you want to change the message, you want to add more stuff):

```
$ echo "foo bar" >> my_file.txt

$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   my_file.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ git add my_file.txt  # if you changed some files

$ git commit --amend --no-edit
[master 4fea3b7] I made a new file
 Date: Wed Jun 13 21:39:14 2018 -0500
 1 file changed, 2 insertions(+)
 create mode 100644 my_file.txt
```

(Or `git commit --amend -m` if you want to change the message)

- If you want to get rid of the commit all together:

```
$ git revert 4fea3b7
[master 140fcd6] Revert "I made a new file"
 1 file changed, 2 deletions(-)
 delete mode 100644 my_file.txt
```

- You can also revert a revert:

```
$ git revert 140fcd6
[master 5bbe17a] Revert "Revert "I made a new file""
 1 file changed, 2 insertions(+)
 create mode 100644 my_file.txt
```

## Refs for 500 ##

- References (or refs) are how git refers to commits
	- git generates a unique number (a hash)
- A series of commits is called a "branch"
- The latest commit in a branch is called the "HEAD"
- See if you can figure out how its organized in the `.git` folder!

## Working with Br4nches ##

- Make a branch:

```
$ git branch copy-edit

```

- Look at all the branches:

```
$ git branch -a
  copy-edit
* master
```

- Look at the full repository history (in a pretty format):

```
$ git log --graph --decorate --pretty=oneline --abbrev-commit --all
```

- Switch to this branch we just made:

```
$ git checkout new-story
Switched to branch 'new-story'
```

- You can delete a branch with `git branch -d` (but you can't be on the branch you're deleting!):

```
$ git checkout master
Previous HEAD position was 140fcd6... Revert "I made a new file"
Switched to branch 'master'

$ git branch -d new-story
Deleted branch new-story (was 5bbe17a).
```

- Let's go ahead and recr3ate that branch:

```
$ git branch new-story

$ git checkout new-story
Switched to branch 'new-story'
```

## Checkout HEADs, Branches, and Commits ##

- `git checkout` is how we move from ref to ref.
	- You can `checkout` commits, brnches, and more!

```
$ git checkout 140fcd6
Note: checking out '140fcd6'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at 140fcd6... Revert "I made a new file"
```

- Oh yea, git showed us a new way to make a branch. It's messages can be very helpful!
- As it says, your changes when you check out a specific commit won't affect any other branch.
- You can only commit on branches when you are checked out to that branch, so let's go back:

```
$ git checkout new-story
Switched to branch 'new-story'
```

## Making Changes on Branches ##

- Let's make some commits on both branches and see how the repository changes:

```
$ echo "breaking news" > new_story.txt

$ git add new_story.txt

$ git status
On branch new-story
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   new_story.txt

$ git commit -m "There's breaking news

Read the new_story.txt for more info"
[new-story aa485ff] There's breaking news
 1 file changed, 1 insertion(+)
 create mode 100644 new_story.txt

$ git log

$ git log --graph --decorate --pretty=oneline --abbrev-commit --all

$ echo "stock report" > stocks.txt

$ git add stocks.txt

$ git commit -m "The Dow jumped 200 points today"

$ git log --graph --decorate --pretty=oneline --abbrev-commit --all

```

## Mrging the Changes ##

- Merging is how you can combine changes on different branches or from different commits
- Git facilitates this process, either handling it atomatically, or provides insights as to what has changed to make it easier for a human to resolve

- Let's merge our two branches:

```
$ git merge new-story
Merge made by the 'recursive' strategy.
 new_story.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 new_story.txt
$ git log

$ git log --graph --decorate --pretty=oneline --abbrev-commit --all

```

### A (Slightly) Harder Merge ###

- Let's set up some commits to trigger what's called a "merge conflict"

- On the master branch:

```
$ echo "new technology fuels optimism" > news.txt

$ echo "solar panels are hot right now" > my_file.txt

$ git add -A

$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   my_file.txt
        new file:   news.txt

$ git commit -m "GE Solar Panels"
[master 3d0e164] GE Solar Panels
 2 files changed, 2 insertions(+), 2 deletions(-)
 create mode 100644 news.txt
```

- Switch over

```
$ git checkout new-story.txt
Switched to branch 'new-story'
```

- Make another commit:

```
$ echo "I got a big scoop" > my_file.txt

$ git commit -a -m "Story dropping next week"
[new-story 13d6d7f] Story dropping next week
 1 file changed, 1 insertion(+), 2 deletions(-)
```

- The `-a` option in `git commit -a -m` stages all modified files before committing.

- Switch back

```
$ git checkout master
Switched to branch 'master'
```

- Now merge

```
$ git merge new-story
Auto-merging my_file.txt
CONFLICT (content): Merge conflict in my_file.txt
Automatic merge failed; fix conflicts and then commit the result.
```

- It fails; git doesn't know how to automatically combine these two sets of changes
- This usually happens when:
	- The same part of the same file has been modified
	- Files have been removed from one branch and modified/added in the other

### Theirs vs Ours ###

- Sometimes, you know that you want the changes from one branch or another
- Git will let you blanket take the changes from the branch being merged in (`new-story`, referred to as `theirs`) or the changes from the branch being merged into (`master`, referred to as `ours`).

```
$ cat my_file.txt
<<<<<<< HEAD
solar panels are hot right now
=======
I got a big scoop
>>>>>>> new-story

$ git checkout --theirs my_file.txt

I got a big scoop

$ git checkout --ours my_file.txt
solar panels are hot right now
```

- If `git checkout --theirs` or `git checkout --ours` works for you, then after the `checkout`, running:

```
git add my_file.txt
```

- Would mark this change as resolved, and then you can follow up with a `git commit -m` to complete the merge

- Most of the time, changes will be complex, and the correct merge will involve taking some changes from both branches. Git has a tool to make this easier

```
$ git mergetool
```

- Explaining the view in Meld:
	- The left view is the state of the file in our current branch (`master`, `ours`)
	- The right view is the state of the file in the branch being merged in (`new-story`, `theirs`)
	- The middle view is the state of the file before either branch changed it, and represents what the file will look like when it is saved.

- Overwrite the contents of the middle view with the lines from the left and right (copy and paste works, but there are also shortcuts and buttons to press). The middle file should look like this:

```
solar panels are hot right now
I got a big scoop
```

- Save and exit
- Let's finish up:

```
$ git status
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

        modified:   my_file.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        my_file.txt.orig

$ git commit -m "Included both diary entries"
[master 5226560] Included both diary entries

$ git log --graph --decorate --pretty=oneline --abbrev-commit --all
```

- I'll give you a less trivial merge conflict at the end, time permitting


## Remote Repositories ##

- Their just normal repositories
	- This is core to git's decentralized model
- They are a common node that all contributors can connect to
	- This is how developers actually share their work with each other
- They can serve as a golden model
	- You communicate with the remote repository in order to get a new copy of the repository
	- The remote repository is usually used for release and authoring projects
- They can serve as a backup (though, every copy of a repository _is_ a backup of that repository)

## Duplicating a Repository ##

- Let's make our repository remotable:

```
$ git config receive.denyCurrentBranch ignore
```

- Starting outside of the repository we already made:

```
$ cd ../
```

- We're going to initialize a new repository, link it to the one we made, retrieve all the changes, and checkout the master branch:

```
$ mkdir copy-of-lunch-and-learn

$ cd copy-of-lunch-and-learn

$ git init
Initialized empty Git repository in copy-of-lunch-and-learn/.git/

$ git remote add origin ~/Documents/repos/lunch-and-learn

$ git fetch --all
Fetching origin
remote: Counting objects: 24, done.
remote: Compressing objects: 100% (15/15), done.
remote: Total 24 (delta 3), reused 0 (delta 0)
Unpacking objects: 100% (24/24), done.
From ~/Documents/repos/lunch-and-learn
 * [new branch]      master     -> origin/master
 * [new branch]      new-story  -> origin/new-story

$ git merge origin/master

$ ls
my_file.txt  new_story.txt  news.txt  stocks.txt

$ git branch -a
* master
  remotes/origin/master
  remotes/origin/new-story

$ git log

```

- `fetch` is a command used to retrieve the changes from remote repositories
	- `--all` fetches all types of changes from all remote repositories
- Every remote has a name used to refer to it.
	- `origin` is the conventonal name for the "main" remote

- `clone` also does all the above for us:

```
$ git clone ~/Documents/repos/lunch-and-learn copy-of-lunch-and-learn
Cloning into 'copy-of-lunch-and-learn'...
done.
```

## Synchronizing Changes ##

### Let's Make a Commit ###

```
$ echo "ping" > ping.txt

$ git add ping.txt

$ git commit -m "Did you receive the file?"
[master d777460] Did you receive the file?
 1 file changed, 1 insertion(+)
 create mode 100644 ping.txt

```

- And then let's push the commit to the remote:

```
$ git push
Counting objects: 3, done.
Delta compression using up to 10 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 275 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To ~/Documents/repos/lunch-and-learn
   5226560..d777460  master -> master
```

- `push` is a command that sends new commits to another repository
	- The branch `master` is already configured to 

### Let's Receive a Commit ###

- First, let's make a commit to retrieve.

- Make a new repository:

```
$ cd ../

$ git clone ~/Documents/repos/lunch-and-learn another-copy-of-lunch-and-learn
Cloning into 'another-copy-of-lunch-and-learn'...
done.

$ cd another-copy-of-lunch-and-learn
```

- Make a new commit and psh it

```
$ echo "pong" > pong.txt

$ git add pong.txt

$ git commit -m "File received"
[master 085c8aa] File received
 1 file changed, 1 insertion(+)
 create mode 100644 pong.txt

$ git push
Counting objects: 3, done.
Delta compression using up to 10 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 266 bytes | 0 bytes/s, done.
Total 3 (delta 1), reused 0 (delta 0)
To ~/Documents/repos/lunch-and-learn
   d777460..085c8aa  master -> master
```

- Let's switch back to the other copy repository:

```
$ cd ../copy-of-lunch-and-learn
```

- And let's retrieve and merge the changes:

```
$ git fetch --all
Fetching origin
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From ~/Documents/repos/lunch-and-learn
   d777460..085c8aa  master     -> origin/master

$ git merge origin/master
Updating d777460..085c8aa
Fast-forward
 pong.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 pong.txt

```

- Oh, there's also a command to make this easier

```
$ git pull
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (3/3), done.
From ~/Documents/repos/lunch-and-learn
   d777460..085c8aa  master     -> origin/master
Updating d777460..085c8aa
Fast-forward
 pong.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 pong.txt
```

- It combines the `fetch` and `merge`
