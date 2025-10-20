## Exercise solutions - Git advanced topics [day 2]

**Table of Content:**

[[_TOC_]]

<br>

:pushpin:  
**Note:** in these corrections, `git switch` is used to checkout/switch branches. If you are using
a Git version older than `2.23`, `git checkout` should be used instead.
Similarly, `git switch -c` (create + switch branch) must be replaced by `git checkout -b`.


## Exercise 1 - The vim cheat-sheet rebase

1. Explore the `vim_cheatsheet.git` repository:
```yaml
cd exercise_1/vim_cheatsheet.git
git log --all --decorate --oneline --graph
cat *.md
git switch dev
git switch master
```

2. Re-order the commits on master: `git rebase -i 0fbf901`

3. Rebase `dev` branch on `master`:
```yaml
git log --all --decorate --oneline --graph
git switch dev
git rebase master
git switch master
git log --all --decorate --oneline --graph     # To check the result.
```

4. To merge the different commits for each file, we start by asking Git to interactively rebase
   on the first commit `git rebase -i HEAD~7`. When editing the rebase command, there are 2
   possibilities:
    * Use the command "fixup" to automatically discard the message of the commit being squashed
      into its parent. Note that in this case we have to use "reword" for the first commit of the
      list, since we were explicitly asked in the exercise to change the commit message for this
      commit (change to "Edit README file: add vim modes and open file example")-
        ```
        reword 9cf4b60 Edit README file
        fixup fa952bb Edit README file: add open file example
        pick 32d2385 Add 'normal_mode' file: commands for vim's normal mode
        fixup f4001cc Edit 'normal_mode' file: add 'dd' and 'yy' commands
        fixup 21a503e Edit 'normal_mode' file: add more commands
        pick 87dc1eb Add 'command_mode' file: commands for vim's command mode
        fixup 62c8c02 Edit 'command_mode' file: add more commands
        ```

    * Use the command "squash" to squash the commits into their parents. With this option, Git
      will open an editor at each squash operation and you will need to edit the commit message
      manually.
        ```
        pick 9cf4b60 Edit README file
        squash fa952bb Edit README file: add open file example
        pick 32d2385 Add 'normal_mode' file: commands for vim's normal mode
        squash f4001cc Edit 'normal_mode' file: add 'dd' and 'yy' commands
        squash 21a503e Edit 'normal_mode' file: add more commands
        pick 87dc1eb Add 'command_mode' file: commands for vim's command mode
        squash 62c8c02 Edit 'command_mode' file: add more commands
        ```

    * You can also use a combination of the above approaches.
   <br>

5. Rebase the `dev` branch:
```yaml
git log --all --decorate --oneline --graph   # Alternative: "git adog", if you have this alias.
git switch dev
git rebase -i master    # Important: discard all duplicated commits in the rebase file.
git switch master
git log --all --decorate --oneline --graph   # To check the result.
```

6. Fix errors in the `README.md` and `normal_mode.md` files, then:
```yaml
git commit --fixup=HEAD~2 README.md
git commit --fixup=HEAD~1 normal_mode.md
git rebase -i --autosquash HEAD~5
git log --all --decorate --oneline --graph       # To check the result.
git switch dev
git rebase -i master    # Important: discard all duplicated commits in the rebase file.
git switch master
```

7. Merge the first two commits:
```yaml
git rebase -i --root master
```

   Change the commit message of latest `dev` commit:
```yaml
git switch dev
git commit --amend -m "Add xkcd comic image file"
```


<br/>
<br/>


## Exercise 2 - The big reset
1. , 2. and 3.:
```yaml
cd exercise_2/the_big_reset.git
git init
git add README.md
git commit -m "First commit"
git add my_quotes.md
git commit -m "Add a file to keep track of quotes"
```

4. Amend the commit message of the second commit:
```yaml
git commit --amend -m "Starting The Big Reset"
git log --pretty=oneline
git reset --soft HEAD~1
git commit -m "Starting The Big Reset!"
```
Comparing the two commit ID values, we can see that they are different, despite the two commits
having the same content and commit message. The reason is because they were made at a different
time, and since time is part of the commit's metadata and thus used to compute the commits' SHA-1
hash value, the value differ.  

5. We must do a reset to the first commit, then add "chuck_norris_quotes.txt" to a `.gitignore`
   file, and amend the first commit with the addition of the `.gitignore` file. Then we can
   do the second commit again.
```yaml
echo "chuck_norris_quotes.txt" > .gitignore
git reset --mixed HEAD~1
git add .gitignore
git commit --amend --no-edit
git add my_quotes.md
git commit -m "Starting The Big Reset!"
```

6. Add 3 favorite quotes in 3 different commits.
```yaml
echo -e "When Chuck Norris does division, there are no remainders.\n" >> my_quotes.md
git commit -m "Add 1st quote" my_quotes.md
echo -e "When Chuck Norris throws exceptions, it’s across the room.\n" >> my_quotes.md
git commit -m "Add 2nd quote" my_quotes.md
echo -e "'I don't always test my code. But when I do, I do in production' - Chuck Norris\n" >> my_quotes.md
git commit -m "Add 3rd quote" my_quotes.md
```

7. Create a new branch at the second commit. Note that with `switch -c` we create the branch
   and check it out at the same time.
```yaml
git switch -c second_choice HEAD~3
cat my_quotes.md                      # Verify there are no quotes in the file.
```

8. Add 3 other quotes, then commit.
```yaml
echo -e "Chuck Norris can divide by zero.\n" >> my_quotes.md
echo -e "Chuck Norris never gets a syntax error. Instead, the language gets an DoesNotConformToChuck error.\n" >> my_quotes.md
echo -e "Chuck Norris doesn't wear a watch. He simply decides what time it is.\n" >> my_quotes.md
git commit -m "Add alternate favorite quotes" my_quotes.md
```

9. Merge branch `second_choice` into `main` (note: if your main branch is `master`, then use
   `master` instead of `main` in the commands below).
```yaml
git switch main
git merge second_choice
vim my_quotes.md           # There is a conflict, so we need to solve it manually.
git add my_quotes.md
git commit
```

10. Revert back to before the merge. Then rebase `second_choice` onto `main` to get a cleaner
    history.
```yaml
git reset --hard HEAD~1     # Now we are back at the state of the repo
                            # as before the merge.
git switch second_choice
git rebase main
vim my_quotes.md            # There is a conflict, so we need to solve it manually.
git switch main
git merge second_choice
```
<br/>
<br/>
<br/>


## Exercise 3 - The backport
1. Enter repo and explore. We can see there are 2 branches and 3 tags. Also, the current working
   tree is not clean, there are 2 files with uncommitted changes.
```yaml
cd exercise_3/backport.git
git log --all --decorate --oneline --graph    # show history.
git status                                    # reveal uncommited changes.
```

2. We store changes in the Git stash, then proceed to revert to version 1.0.1 with a checkout.
   Looking at the history, we can now see that:
    * `HEAD` is now on commit `d2c319e`, which is where the tag `v1.0.1` is attached.
    * A new (temporary) "commit" has appeared at the tip of the `dev` branch: it's the stashed
      content.
```yaml
git stash
git checkout v1.0.1
git log --all --decorate --oneline --graph
```

3. Add backport fix, then commit the change:
```yaml
git commit -m "Backport: add support for 'json' file type" functions.py
```

4. Upon switching to `dev`, Git gives us a warning:  
   "Warning: you are leaving 1 commit behind, not connected to any of your branches:"  
   If we leave a "lose" commit (i.e. not part of any branch), it will eventually be pruned (i.e.
   deleted) by Git at some point. So it's not a safe option for commit that we want to keep.  
   So we follow Git's advice and create a new branch. We then switch the new branch, and add a
   tag `v1.0.1b`.
```yaml
git switch dev
git branch backport 1a2fa0c    # Note: your commit ID will differ.
git switch backport
git tag v1.0.1b
```

5. Go back to `dev`, restore the stashed changes, then make a commit.
```yaml
git switch dev
git stash pop
git commit -am "Improve file import tests"
git log --all --decorate --oneline --graph
```

6. Delete the `backport` branch. Note that since this branch is not fully merged, git will give
an error `error: The branch 'backport' is not fully merged.` if we try to use `git branch -d`, so
we must "force delete" the branch with `git branch -D`.
```yaml
git branch -D backport
```

7. Rebase and merge `dev` into `master`. Add a `v2.0.0` tag.
```yaml
git rebase master
git switch master
git merge dev
git tag v2.0.0
git log --all --decorate --oneline --graph
```
