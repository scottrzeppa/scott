# Part 1: Unix

## Introduction

In this exercise, you will learn how to use Unix commands to perform exploratory data analysis (EDA) from the command line.  This is an invaluable skill which will help you find problems in data sets and clean data more quickly.  Often, with a little cleverness, you can answer questions about your data without needing to resort to `Python`, `R`, and other heavier tools.

Unix is particularly adroit at handling text files because there are many commands which can operate on this format.  By extension, CSV files are also easy to manipulate.

## Basic
### Getting help and setup things up

What does the `man` command do?  Use `man` to learn how to use `man`.  Often, man-pages are still a useful source of information.

###  Customizing `bash`

You can custom the `bash` shell by creating aliases and shell scripts.  You will also need to modify environment variables so that commands are in your path.


#### Aliases

In which dotfile, should you put BASH customizations such as aliases?  `~/.profile`, `~/.bash_profile`, or `~/.bashrc`?

I find the following aliases helpful:

```bashrc
alias h='history'
alias l='ls -GFh'
alias ll='ls -lFGh'
```

Add this to your `~/.bashrc`.  You may need to modify `~/.bash_profile` to source `~/.bashrc`.  Whenever you change a dotfile, you will need to source it for the changes to take effect:

```bash
$ source ~/.bash_profile
```

What do these aliases do?

You can add other aliases for commands which you often perform, which will boost your productivity.

### Environment variables

It is good practice to store utility scripts in `~/bin`.  Create this directory and add it to your `PATH` so that you can use your scripts anywhere. Don't forget to source `~/.bash_profile` afterward.

For Python libraries, you will need to modify `PYTHONPATH`.  Other applications will also have environment variables which you need to configure, which should be explained in the relevant documentation.

## Advanced: Wikipedia data

Let's analyze some Wikipedia data using Unix tools.  But first some advice:  you will be working with a large file, so commands will take a while to run.  Consequently, test your scripts and commands on a small subset of the file so that you can get them working quickly.  When you working in an environment, like `bash` or regular expressions, where debugging tools are scarce, you should build up your code in small steps, testing each step along the way.  Document anything clever to make future maintenance easier.


#### Getting started

Start by downloading the data using the command line:

```bash
$ wget https://dumps.wikimedia.org/enwiki/latest/enwiki-latest-pages-meta-current1.xml-p1p41242.bz2
```

*   What does the `wget` command do?  Could you also use `curl`?

*   What type of file is `enwiki-latest-pages-articles1.xml-p1p30303.bz2`?  How can you figure out the a file's type? Often files come in different (compressed) archive formats. `tar` and `gzip` are common.  Unpack the file.

*   How big is it?  How many lines does it have?  What type of file is the uncompressed version of the file?

*   Would it be a good idea to load this file into an editor?


#### Examining the file

Loading large files into an editor is a bad idea because it is time consuming and can crash your editor if you run out of memory.  Fortunately, Unix provides tools which work on arbitrarily large text files, including CSV.

*   What do the beginning and end of the file look like?  What commands did you use?

*   Examine the first couple pages of the file.  What do you notice about the structure?

*   Advanced:   if you want to look at just lines 636100 to 636120 how would you do it?  (Hint: use `sed`.)


#### Exploratory analysis

Let's answer some basic questions about contributors.

*   The tag `<contributor>` ... `</contributor>` identifies a contributor.  How many are there?

*   How many users are identified by the tag `<username>`?

*   Who are the most common contributors by username? Hint: you will need to connect several commands via pipes.  `sed` or `perl` with the right regular expression will allow you to extract the name from between the `<username>` ... `</username>` tags.  `sed` uses an older version of REs so you will need to escape the parentheses on the capture group.

*   How many of the contributors are bots?

*   Advanced:   What are the other contributors -- i.e., those entries which have a `<contributor>` tag but not a nested `<username>` tag?  Hint: use `sed` or `perl`.  How many of these contributors are there?  Do they explain all of the missing contributors?

#### Extra Credit: Shell scripts

Often, I like to look at the beginning and end of a file.  A one-liner to do this is:

```bash
$ (head -n 5 ; echo "==========" ; tail -n 5) < some_file_of_interest
```

The expression between the parentheses will run in a sub-shell.  We use I/O redirection so the shell will read from `some_file_of_interest`.  Create an alias to do this.  Hint: leave off the `< some_file_of_interest` part.  Then you can run you alias as `inspect < some_file_of_interest`.  In this case, using an alias is not as clean as using a shell script because you have to specify I/O redirection and you have hard coded the number of lines to display.  Write a script `inspect.sh`.  Make sure you can pass the name of the file as an argument.  Optionally, support an extra argument to specify how many lines to display.  Don't forget to change the permissions so that your script is executable.


# Part 2: Intro to git and Github

## Introduction

The `haiku.txt` file is supposed to contain haikus, preferably of the data
science variety.  However, `haiku.txt` is an empty file presently.

A haiku is a three-line poem with seventeen syllables, written in a 5/7/5 syllable
count, though liberty is often taken with both these requirements.  It's derived
from Japanese poetry.

A couple examples:

```
An old silent pond...
A frog jumps into the pond,
splash! Silence again.
```
-- Matsuo Basho
<br>
<br>
```
Nightfall,
Too dark to read the page
Too cold.
```
-- Jack Kerouac

Parts I - III of this assignment are done individually.  In Part IV you'll pick a partner and work with them.

## Basic

### Modify the history of a repository and save it
1. Fork then clone this repo locally.
2. Add your own haiku to `haiku.txt`
3. Add, commit, and push it to your forked Github repo.  When you commit it, use a message like "first haiku", i.e. `$ git commit -m "first haiku"`
4. View `haiku.txt` on your Github and verify that it has been updated.  Verify that you see the commit message on Github.


### Further modify the history, then revert to an old version (an old commit)
1. Open up your `haiku.txt` file again and add another haiku underneath the first one.
2. Add, commit, and push it.  When you commit it, use a different message than the first, e.g. `$ git commit -m "second haiku"`
3. Verify that the modified file and the second commit message are visible on your Github.
4. At this time, you should have added two commits to this repository.  View the commits to the history of the repository using [git log](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History).  Note that the repository has more commits than just yours (Galvanize has modified it since its creation).  If you just want to see the last 2 commits, type `$ git log -2`
5. Note the **hash** (the long sequence of characters) that follows the word 'commit' for each of your commits.  This is the unique identifier for the state of the respository at that commit. The commit that you are presently using is called the `HEAD`. Luckily, you don't need to write down all those characters and remember them to reference each commit.  Try this command: `git log --oneline -2`.  You should be seeing the same commits, and the **first seven characters of the hash that uniquely identify the commit**.
6. You've decided that you don't like your second haiku.  So you want to switch back to the commit that only had your first haiku using `$ git checkout`. See an example of `git checkout` usage to switch to an older commit in this [blog post](https://opensource.com/life/16/7/how-restore-older-file-versions-git) in the **Restore a file** section. After checkout, make sure you add, commit, and push it so that the old version is contained in the current HEAD of the project.
7. FYI, there's often more than one way to do things:  check out the use of `$ git reset` and `$ git revert` [here](https://stackabuse.com/git-revert-to-a-previous-commit/).

### Using branches
In software development, often the code associated with the version that is being used commercially is in the `master` branch.  If you were to modify that code and add and commit it, it may break a currently deployed piece of software, upsetting users and the company.  To safeguard against this, you make a new **branch**, say `dev` (short for development), and work in that instead of the `master` branch. Then, when you are ready, you can **merge** the `dev` branch into the `master` branch so that the master branch incorporates the new development effort.  

So, what is a **branch** in git?  As shown in Figure 11 [in this blog](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell), a branch is simply a pointer to a commit.  In Figure 11, the `master` branch points to commit `f30ab` (which points to commit `34ac2`, which points to `98ca9` etc.). `HEAD` in Figure 11 points to `master`, signifying which branch the user is presently working on.  Figure 12 shows the addition of another branch (called `testing`) that also points to commit `f30ab`.  In Figure 13, the `testing` branch exists but the user is still on the `master` branch (as indicated by `HEAD`), while in Figure 14 the user has switched to the `testing` branch.  Figure 15 indicates that in the `testing` branch the user has done some work and added and commited it, so that the `testing` branch is now 1 commit ahead of `master`.
1. Replicate this process for your haikus (a `haiku-dev` branch?) using git commands in the [blog](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell), starting at the **Creating a New Branch** section.  
2. Add and commit a new haiku on your `haiku-dev` branch.  You can push this branch to Github using [this syntax](https://stackoverflow.com/questions/2765421/how-do-i-push-a-new-local-branch-to-a-remote-git-repository-and-track-it-too).  Go to your Github and verify that your new branch exists there, too.
3. You like the haiku you made in the `haiku-dev` branch.  You think it should be included in the haikus that you present on the `master` branch.  So you want to **merge** it into master. As explained in step-by-step detail in this [blog](https://stackabuse.com/git-merge-branch-into-master/), the steps are to 1) make sure you are on the `master` branch using `$ git checkout master`, then 2) merge the branch you created (`haiku-dev`) into master using `git merge haiku-dev`.
4. After material in branches has been merged into master, it's considered best practice to delete the branch locally and remotely (on Github).  Locally: `$ git branch -d localbranchname`, remotely (Github): `$ git push origin --delete remotebranchname`.  Do yourself a favor and keep your branch names consistent locally and on Github (don't call it `haiku-dev` locally and `haiku-development` on Github).

## Advanced: Git with a partner
There's a [Git quick reference guide in the reference folder.](https://github.com/GalvanizeDataScience/git-intro/blob/master/reference/Git.pdf) There's a section on pair programming.  Even though you're pair "haikuing" at this point, it should help with the steps below.  
1. Pick a partner to write some haiku with you. For now, decide to work out of the `master` branch.
2. Decide whose repo to work out of.  Call this person `A`.  `A` will need to add the other (`B`)
   as a collaborator on `A's` repo.  [Here's how.](https://help.github.com/articles/inviting-collaborators-to-a-personal-repository/)
3. `B` needs to add `A's` repo as a remote in `B`'s repo.  [Here's how.](https://help.github.com/articles/adding-a-remote/)
4. Now take turns writing lines of haiku.  Only one person should be typing at a time on his/her laptop. Generally the steps should be:
   * `git pull ...`
   * type some haiku...
   * `git add ...`
   * `git commit ...`
   * `git push ...`
   * ...and then other person starts with `git pull ...` <br>
   You should both be pushing and pulling to the *same* repository.  Otherwise it would be hard to collaborate, neh?  For one person it will be `origin`, for the other it will the name they give that person's remote.  See the Git quick reference guide for more on this.
5. If you run into a Merge Conflict (and you should), resolve it.  [Here's how.](https://help.github.com/articles/resolving-a-merge-conflict-using-the-command-line/)
6. Merge conflicts can be a pain.  This could have been avoided if each person that was writing haikus branched off of master, wrote their haikus, and then the branches with the new haikus were sequentially merged into master.  This is how you'll be working in git on case studies.
