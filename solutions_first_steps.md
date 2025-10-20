## Exercise solutions - Git first steps [day 1]

<br>

[[_TOC_]]

<br>

:pushpin:  
**Note:** in these corrections, `git switch` is used to checkout/switch branches. If you are using
a Git version older than `2.23`, `git checkout` should be used instead.
Similarly, `git switch -c` (create + switch branch) must be replaced by `git checkout -b`.

<br>

## Exercise 1 - Your first commit

1. After initializing a new Git repository with `git init`, we run `git status`: what we see is
   that, as expected at this point, all files are untracked. The next step will therefore be to
   add some of the files to the git index so they can then be committed.
```yaml
cd exercise_1   # To initialize a new git repository, we must first enter the repository in which
                # we want to track changes.
ls -l           # This command is to list the files present the directory.
                # Alternative: "tree .", if you have "tree" available as command.
git init        # Create a new Git repository.
git status      # At this point, all files are untracked.
```

2. Stage all files except `test_results.out`, then check the status of the repository again.
```yaml
git add R/ tests/ DESCRIPTION LICENSE README.md
git status         # Files that have been added to the Git index for the first time are
                   # displayed as "new file".
                   # These files are now "staged" and are ready to be committed.
```

3. Make a first commit.
```yaml
git commit -m "Initial commit for fake stringr package"
git log        # Show commit history.
git show       # Show content of the last commit.
git status     # Show status of files in the working tree.
```
   At this point there is only 1 untracked files left: `test_results.out`.

4. Since we want the `test_results.out` file to be ignored by all copies of the repository, the
   correct location to exclude it is in `.gitignore`.

   After having created a new `.gitignore` file that contains the text `test_results.out`, we still
   have one untracked file: the `.gitignore` file itself! This file is meant to be tracked by Git,
   so that it can be shared with others. Therefore we add it to the git index and then commit it.
```yaml
echo "test_results.out" >> .gitignore
git status                           # There is still one untracked file: .gitignore !

git add .gitignore                   # Stage the .gitignore file.
git commit -m "Add gitignore file"   # Commit your changes.
git status                           # Now there are no more untracked files displayed.
```

5. Edit the `README.md` file to add the package author and URL information.
   Then commit the changes:
```yaml
vim README.md          # Edit author and URL manually (a different editor can be used).
git status             # Show the status of files in the repo: the README.md file should
                       # now be listed as "modified".
git diff               # Show the differences between the version of the files on disk
                       # and the version of the files in the index.

git add README.md      # Alternatively: git add -u
git status             # the README.md file should now be listed under "Changes to be committed:".
git commit -m "README: add author and URL info"

# Shortcut to stage and commit in a single command:
git commit -m "README: add author and URL info" README.md
```

6. Explore different ways to display a Git repository's history:
```yaml
git log                                     # Prints the full commit message along with author and date.
git log --pretty=oneline                    # 1 commit per line. Full commit hash/ID (checksum).
git log --oneline                           # 1 commit per line. Abbreviated commit hash/ID.
git log --all --decorate --oneline --graph  # Shows commits for all branches.
```

7. Create an `adog` alias Git command and test it:
```yaml
git config --global alias.adog "log --all --decorate --oneline --graph"
git adog
git config --list | grep ^alias    # show your defined Git aliases.
```

8. Run the commands given in the instructions to create 3 new files and modify 2 existing files.
   When running `git status` you should see that 2 files are modified, and 3 are untracked
   (actually, the entire `large_data` directory will be shown as untracked).
```text
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   DESCRIPTION
	modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	large_data/
	personal_notes.txt
```

9. Explore the difference between `git add -u` and `git add --all`.
   First let's see what `git add -u` does: it updates the Git index with the new version of all
   files that are **already tracked** by Git (in our case `DESCRIPTION` and `README.md`), but it
   does *not* stage any new, untracked files. In our case, `personal_notes.txt` and the files in
   `large_data` remain untracked.
```yaml
git status
git add -u       # The long form of -u is: git add --update
git status       # personal_notes.txt and files in large_data remain unstaged, because
                 # the "-u/--update" option only stages files that are already tracked.

git reset HEAD   # Remove the staged changes for `DESCRIPTION` and `README.md`
                 # so we can test the "add --all" option.
```

   Now let's test what `git add --all` does: it updates the Git index with all modified and
   untracked files. As can be seen, our 5 files (modified and new) have now been staged:
```yaml
git status
git add --all
git status

Changes to be committed:
    modified:   DESCRIPTION
    modified:   README.md
    new file:   large_data/large_1.csv
    new file:   large_data/large_2.csv
    new file:   personal_notes.txt
```
   The difference between `git add -u` (`-u` is a shortcut for `--update`) and `git add --all` is
   that using `--update` will only add files that are already tracked in Git, while `--all` will
   add all files (except ignored files), whether they are already tracked (modified) or not
   (untracked). In a sense, `--update` is safer because it prevents you from adding completely new
   files to the Git repo by mistake.

10. let's unstage each of our 3 files with one of the possible command to remove content from the
    git index:
```yaml
git restore --staged personal_notes.txt
git reset HEAD large_data/large_1.csv
git rm --cached large_data/large_2.csv
```
   The difference between `git rm --cached` and the two other commands is that `git rm --cached`
   will **unstage the entire file**, not just the new changes that were made to it since the last
   commit.

   The reason why in this particular case `git rm --cached` does exactly the same as
   `git restore --staged ` and `git reset HEAD` is because the 3 files that we unstaged are new
   and have never been added to the Git repo before. There is thus no difference between removing
   them completely, or just resetting them back in the index to their version from the latest
   commit (since they are all absent from the latest commit).

11. Since the content of `large_data` should be ignored by all copies of the repository, we
    ignore it using the `.gitignore` file.

    `personal_notes.txt`, on the other hand, should only be ignored only by the local instance of
    our Git repo, and must therefore be added to `.git/info/exclude`.
```yaml
echo "large_data" >> .gitignore
echo "personal_notes.txt" >> .git/info/exclude
git status
```
    At this point, the output of `git status` should look like this:
    ```text
    On branch master
    Changes to be committed:
    	modified:   DESCRIPTION
    	modified:   README.md

    Changes not staged for commit:
    	modified:   .gitignore
    ```

12. Commit all remaining changes:
```yaml
git add .gitignore                              # Stage changes in the .gitignore file.
git status
git commit -m "Update DESCRIPTION and README"
git status                                      # The working tree should now be clean.
git log --oneline                               # Show commit history of the current branch.
```


<br/>
<br/>


## Exercise 2 - The git reference web page
Change into the `exercise_2/` directory, and look at the current status of the git repository.
```yaml
cd exercise_2/
git log
git log --oneline     # There are 3 commits in the repo.
git branch            # There is currently only 1 branch: master.
git status            # There is one tracked file with uncommitted changes: references.html
```

### A) Fix the broken "ProGit" link.
Make a new commit with the non-committed changes:
```yaml
git diff                   # Check what the changes are.
git add references.html    # Stage the changes in the references.html file.

git commit -m "Add Git logo placeholder to the Git reference webpage"
# Shortcut for the above 2 lines:
#  git commit -m "Add Git logo placeholder to the Git reference webpage" references.html
#  or
#  git commit -am "Add Git logo placeholder to the Git reference webpage"

git status   # There are no more uncommitted changes.
```

Fix the "ProGit" link in a temporary `fix` branch. After testing that the HTML page works
correctly, commit the change (note: the `git switch` command is only available in Git
versions >= 2.23).
```yaml
git branch fix
git switch fix     # Alternatively, you can also use "git checkout fix"
# Shortcut for the above 2 lines: "git switch -c fix" or "git checkout -b fix"

vim references.html           # Edit the HTML page... then verify
                              # in the browser that it works.
git add references.html
git commit -m "Fix broken ProGit link"
# Shortcut for the above 2 lines:
#  git commit -m "Fix broken ProGit link" references.html
#  or
#  git commit -am "Fix broken ProGit link"
```

Merge the changes into branch `master` and delete branch `fix`.
```yaml
git switch master     # Alternatively, on older Git versions: "git checkout master"
git merge fix         # Note: no additional commit is created by the merge,
                      # because this is a "fast-forward" merge.

git log --all --decorate --oneline --graph   # Display repo commit history.

git branch -d fix     # Delete the "fix" branch, as we no longer need it.
git branch            # Verify that the "fix" branch is gone.
git log --all --decorate --oneline --graph    # Show repo history again.
```


### B) Add an image and new links to the HTML page.
Add new links to the web page:
```yaml
git switch -c dev       # Alternatively, on older git versions: "git checkout -b dev".
vim references.html     # Edit HTML page to add the new links...

# After having checked that the two new links are working, stage and commit the changes:
git diff
git diff --cached
git add references.html
git diff
git diff --cached
git commit -m "Add two new books to Git reference page"
```
The difference between `git diff` and `git diff --cached` is that the former will display the
difference between the working tree (files on disk) and the git index, while the later shows the
difference between the git index and the latest commit.

Add a Git logo to web page:
```yaml
vim references.html             # Edit HTML page to add logo...
git commit -m "Add git logo" references.html
```

Merge changes into `master`, delete branch `dev`:
```yaml
git switch master      # To merge "dev" into "master", we must be on the "master" branch.
git merge dev          # merge dev into master.

git log --all --decorate --oneline --graph   # Verify that both "dev" and "master" now point
                                             # to the same commit.
git branch -d dev                            # Delete the "dev" branch.
```


<br/>
<br/>


## Exercise 3 - The crazy peak sorter script
Clone the peak sorter project from GitHub, test run the peak sorter script, and display the Git
repository's history.
```yaml
# Clone the Git repository from GitHub.
cd exercise_3/
git clone https://github.com/sibgit/peak_sorter.git   

# Test-run the peak sorter script.
cd peak_sorter
./peak_sorter.sh                                      

# Display the Git repository's history
git log --all --decorate --oneline --graph
```

### A) Add a fix to the master branch
Apply Jimmy's fix using cherry-picking on a temporary `hotfix` branch.
```yaml
git switch -c hotfix                         # Alternatively: git checkout -b hotfix
git log --all --decorate --oneline --graph   # Search for the commit ID of Jimmy's fix: 1c695d9
git cherry-pick 1c695d9
git log --all --decorate --oneline --graph   # Verify the commit was properly cherry-picked.
git show HEAD                                # Have a look at changes introduced by the cherry-pick.
./peak_sorter.sh                             # Test run the script to verify nothing is broken.
```

Merge the fix back into `master`.
```yaml
# Merge branch hotfix into master
git switch master                           # Alternatively: git checkout master
git merge hotfix

# Verify that hotfix and master not point to the same commit.
git log --all --decorate --oneline --graph

# Test script and delete the temporary hotfix branch.
./peak_sorter.sh                            # Test run: check the script is still working.
git branch -d hotfix                        # Delete hotfix branch, as we no longer need it.
```

### B) Add the Dahu count feature to the master branch
Checkout the feature-dahu branch. Run the script to test that it works.
```yaml
git switch feature-dahu      # Alternatively: git checkout master
./peak_sorter.sh
```

Rebase `feature-dahu` on master. As there are conflicts, we need to manually resolve them by
opening the conflicted file in an editor, resolving the conflict, then running
`git add peak_sorter.sh` and `git rebase --continue`.
```yaml
git log --all --decorate --oneline --graph   # display the repo history, so we can then compare
                                             # it before and after the merge.
git rebase master

# There are 3 conflicts, so repeat the steps below 3 times.
vim peak_sorter.sh        # Open file in editor to solve the conflict.
git add peak_sorter.sh
git rebase --continue

./peak_sorter.sh          # Test run to see if everything is
                          # still working after the rebase.
```

We are now ready to merge the new feature into `master`. Note that since we have rebased the
branch `feature-dahu` on `master`, the merge will be **fast-forward** and there will be no
conflicts.
```yaml
git checkout master
git merge feature-dahu       # This is now a fast-forward merge -> no conflicts.
./peak_sorter.sh             # Test run to see if everything is
                             # still working after the merge.
```


<br/>
<br/>


## Exercise 4 - The Awesome Animal Awareness Project

#### A) Organize your team
Clone the Awesome Animal Awareness Project (A3P) repo.
```yaml
cd exercise_4/
git clone https://github.com/sibgit/sibgit.github.io.git
cd sibgit.github.io
```

The team leader creates the main development branch for the team and pushes it to the remote
repository on GitHub. The example here is for the team `yeti`.
```yaml
git branch yeti-dev
git switch yeti-dev          # Alternative for older git versions: git checkout yeti-dev

# Alternative: create a branch and switch to it in a single command:
git switch -c yeti-dev       # Alternative for older git versions: git checkout -b yeti-dev

git push -u origin yeti-dev  # Note: -u is the short option for --set-upstream
```

Other team members can now pull the new team branch `yeti-dev` branch and create a local copy of
it by checking-it out.
```yaml
git fetch
git checkout yeti-dev
```

### B) Add content for your awesome animal
Each team member creates his personal work branch. The example here is for `Alice`, working in team
`yeti`. Alice edits the `yeti.html` page in her favorite editor. When she is done, she load the
page in her browser to make sure the rendering is looking good. Then she can commit her changes.
```yaml
git switch -c yeti-alice      # Alternative for older git versions: git checkout -b yeti-alice
git add yeti.html
git commit -m "Add habitat and distribution info to Yeti page."
```

### C) Merge your branch with your team's animal-dev branch
Each member of the team must now add the changes they made on their personal branch to the team
branch. Let's assume that Alice is either the second or third team member to add her changes,
and therefore she has to rebase her branch `yeti-alice` onto `yeti-dev`.

1. Alice updates he local version of the team branch.
```yaml
git switch yeti-dev   # Older versions of Git must use "checkout" instead of "switch".
git pull              # Fetch updates from remote and merge with local yeti-dev branch.
```

2. Alice rebases her personal branch on the team branch.
```yaml
git switch yeti-alice
git rebase yeti-dev     # Conflicts will appear, so Alice must solve them manually.
git add yeti.html
git rebase --continue
```

3. Now that the rebase is completed, Alice can merge her branch into `yeti-dev`. Then she pushes
the updated `yeti-dev` branch to the remote and let her team members know she is done.
```yaml
git switch yeti-dev
git merge alice-dev           # This is now a simple fast-forward merge.
git push

# Optional: Alice can now delete her personal branch, if it is no longer needed.
git branch -d alice-dev
```

### D) Create a pull request for the top-level management to verify and approve your work.
Now that the yeti page is completed, the team leader can make a pull request to the owner of the
project on GitHub (i.e. one of the class teachers).

When the request is accepted, the changes introduced in `yeti-dev` will be merged into the `master`
branch of the project.

All members of the team can now view their work at https://sibgit.github.io, and update their local
git repository.
```yaml
git switch yeti-dev   # Older versions of Git must use "checkout" instead of "switch".
git fetch             # This is optional, to make sure the merge from "git pull" will be fast-forward.
git pull
git switch master
git pull
```

### E) branch cleanup
Delete the local and remote instances of the team branch, delete the local instance of the
personal branch, update the local `master` branch.
```yaml
# Delete the remote instance of the team branch.
#  -> Note: only 1 person in the team needs to do this.
git push origin --delete yeti-dev

# Update the local instance of the master branch.
git fetch --prune
git switch master
git pull

# Delete local instances of branches.
git branch -d yeti-alice
git branch -d yeti-dev
```

<br/>
<br/>
