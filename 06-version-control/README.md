# Introduction to Git

**Basic Git configuration**

```coffee
git config --global user.name "Karthik Ram"
git config --global user.email "kram@berkeley.edu"
git config --global color.ui "auto"
```

Please replace my name/email with yours before pasting it into a terminal. If it returns no errors, then verify that these settings were saved into your configuration file (`.gitconfig`) by running:

```coffee
git config --list
git config user.name
```

**Configure your text editor**


```coffee
git config --global core.editor "gedit -w -s" 
```

Default is `vi`.
Other options:
    "nano" 
    "kate"
    "subl -n -w' for Sublime Text 2.

###  Other editors 

On Windows using Notepad++ in the standard location for a 64-bit machine, you would use:

    $ git config --global core.editor "'C:/Program Files (x86)/Notepad++/notepad++.exe' -multiInst -notabbar -nosession -noPlugin"

(thanks to [StackOverflow](http://stackoverflow.com/questions/1634161/how-do-i-use-notepad-or-other-with-msysgit/2486342#2486342)  for that useful tip)

On Mac, with TextWrangler if you installed TextWrangler's command line tools
then you should have an "edit" command. So you can use the git command:

    $  git config --global core.editor "edit -w"



**Adding some aliases**

Aliases make it easy to type in long commands once you are familiar with what they are and what they do. 

```
git config --global alias.st status 
git config --global alias.ci commit 
git config --global alias.co checkout 
git config --global alias.hist 'log --graph --decorate --pretty=oneline --abbrev-commit'
git config --global alias.ls 'log --pretty=format:"%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)[%an]%Creset" --abbrev-commit'
git config --global alias.ll 'log --pretty=format:"%C(yellow)%h%Cred%d %Creset%s%Cblue [%cn]" --decorate --numstat'
```




**Commonly used Git commands**  

* `git init` (will initialize a new Git repository inside a folder. It * also affects all files and subfolders within. Only required once per * project. Projects are managed at a folder level. Do not nest Git projects inside each other).

* `git status` (will show you the current state of the repository. * You'll use this all the time).

* `git show-branch` (see recent commits from branches).

* `git checkout` (switch branches, or grab files from earlier commits).

* `git add` (stage files ready for committing).  
* `git commit` (commit files into Git's memory with a useful message).  
* `git branch` (list branches, create them or delete existing ones).  
* `git push` (sync changes with a remote repository).  
* `git pull` (sync changes from a remote to a local repository).  
* `git merge` (merge changes between branches).  
* `git diff` (show changes between current file and most recent commit)* .
* `git log` (see detailed logs. Using the aliases defined above, you can see shorter one-line logs and also branch activity).

There are many more commands in Git but if you master this set it should serve you well for a long time.


**Searching Git logs**

Show commits only till a certain date.
```
git ls --until="2013-10-01"
```

Show commits since a certain date

```
git ls --since="2013-10-01"
```

Grep through your logs

```
git ls --grep=exercise
```
View only last 5 logs

```
git log -n 5
git ls -n 5
```

Logs by a specific author


```
git log --author="Karthik"
git ls --author="Karthik"
```

Show only commits that were merges

```
git log --merges
```



---


**Practicing Git branching**
[http://pcottle.github.io/learnGitBranching/?NODEMO](http://pcottle.github.io/learnGitBranching/?NODEMO)
In the demo you won't have to add files so just go on comitting. Remove ?NODEMO if you want more instructions. It's a great resource for both beginners and advanced users.

---

**Additional Git resources**

* The official site for the Git (open source project. Not to be confused with GitHub). http://git-scm.com/
Read the entire e-book on Git for free here: http://git-scm.com/book
If you feel the need to use Git outside of the command line, there are many guis availalable for different platforms. http://git-scm.com/downloads/guis

* A [sample gitconfig file](https://github.com/swcarpentry/boot-camps/blob/2013-09-bristol/version-control/sample.gitconfig) with more aliases (rename to .gitconfig and replace with right credentials)

* [Longer explanation on ignore files](https://help.github.com/articles/ignoring-files): 
* [A whole repository of example ignore files for various programming languages](https://github.com/github/gitignore)  
* [Here's the one for R](https://github.com/github/gitignore/blob/master/R.gitignore)

* [Version control by example](http://www.ericsink.com/vcbe/) (free ebook) 

* A (shameless plug) for my paper on why Git is important for advancing science. 
http://www.scfbm.org/content/8/1/7

* [An extremely useful Git cheat sheet](http://www.git-tower.com/blog/git-cheat-sheet/)
* 