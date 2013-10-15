
# Version control exercise 02  
**Branching and merging**  

1. Create a new branch using `git branch <branch_name>`.
2. Then checkout the new branch
3. Make some changes (either edit exisitng files or add new ones)
4. Commit those changes.
5. checkout the master branch.
6. Do a directory listing using `ls -l` to make sure the new files aren't in your master.
7. Now merge changes from the new branch into master by running `git merge <branch_name>`

**Testing merge conflicts**

Git will automatically merge as much as possible. However, when the same lines are edited on two different merge branches, Git cannot resolve these changes automatically and will ask you to fix those. Let's try to create such a situation and fix it.

1. Make a distinct change to a file on the `master` branch.
2. checkout an existing branch or create a new one and then check it out. Tip: You can do both in the same step using `git checkout -b <new_branch>`. 
3. Edit the same line on the new branch.
4. `git checkout master`. Tip: To checkout the last branch, just use `git checkout -`
5. Now merge.
6. Note the errors.
7. Open the file and notice how Git has highlighted the location of the problem.
8. Keep the right change, delete the other one, and the Git marker text.
9. Now commit this resolved merge.
10. Put up a green sticky if you've reached this point without any trouble (otherwise red sticky to get some help).
