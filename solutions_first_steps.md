# Exercise solutions - Git first steps

<br>

[[_TOC_]]

<br>

:pushpin:  
**Note:** in these corrections, `git switch` is used to checkout/switch
branches. If you are using a Git version older than `2.23`, `git checkout`
should be used instead.
Similarly, `git switch -c` (create + switch branch) must be replaced by
`git checkout -b`.

<br>
<br>

## Exercise 1 - Your first commit

1. To initialize a new git repository, we must first enter the repository in
   which we want to track changes.
    ```yaml
    cd exercise_1  # Enter the directory.
    ls -l          # This command lists the files present the directory.
    ```

2. After initializing a new Git repository with `git init`, we run `git status`:
   what we see is that, as expected at this point, all files are untracked.
   The next step will therefore be to stage some of the files (add them to the
   git index) so they can then be committed.
    ```yaml
    git init    # Create a new Git repository.
    ls -la      # We can observer that a new ".git" hidden directory was created.
    git status  # At this point, all files are untracked.
    ```

3. Stage all files except `test_results.out`, then check the status of the
   repository again.
    ```yaml
    git add R/ tests/ DESCRIPTION LICENSE README.md
    git status  # Files that have been added to the Git index for the first
                # time are displayed as "new file". These files are now
                # "staged" and are ready to be committed.
    ```

4. Make a first commit.
    ```yaml
    git commit -m "Initial commit for fake stringr package"
    git log     # Show commit history.
    git show    # Show content of the last commit.
    git status  # Show status of files in the working tree.
    ```
   At this point there is only 1 untracked files left: `test_results.out`.

5. Since we want the `test_results.out` file to be ignored by all copies of the
   repository, the correct location to exclude it is in `.gitignore`.  
   After having created a new `.gitignore` file that contains the text
   `test_results.out`, we still have one untracked file: the `.gitignore`
   file itself! This file is meant to be tracked by Git, so that it can be
   shared with others. Therefore we stage and commit it.
    ```yaml
    echo "test_results.out" >> .gitignore
    git status                           # There is still one untracked file: .gitignore !

    git add .gitignore                   # Stage the .gitignore file.
    git commit -m "Add gitignore file"   # Commit your changes.
    git status                           # Now there are no more untracked files displayed.
    ```

6. Edit the `README.md` file to add the package author and URL information.
   Then commit the changes:
    ```yaml
    # Editing the README.md file:
    vim README.md      # Edit author and URL manually (any text editor can be used).
    git status         # Show the status of files in the repo: the README.md file should
                       # now be listed as "modified".
    git diff           # Show the differences between the version of the files on disk
                       # and the version of the files in the index.

    # Staging (i.e. adding to the git index) the file:
    git add README.md  # Alternatively: git add -u
    git status         # The README.md file should now be listed under "Changes to be committed:".

    # Commit the changes to README.md:
    git commit -m "README: add author and URL info"

    # Shortcut to stage and commit in a single command:
    git commit -m "README: add author and URL info" README.md
    ```

7. Explore different ways to display a Git repository's history:
    ```yaml
    git log                                     # Prints the full commit message along with author and date.
    git log --pretty=oneline                    # 1 commit per line. Full commit hash/ID (checksum).
    git log --oneline                           # 1 commit per line. Abbreviated commit hash/ID.
    git log --all --decorate --oneline --graph  # Shows commits for all branches.
    ```

8. Create an `adog` alias Git command and test it:
    ```yaml
    git config --global alias.adog "log --all --decorate --oneline --graph"
    git adog

    # List your defined Git aliases.
    git config --list | grep ^alias
    ```

<br>

### Additional Tasks

9. Run the commands given in the instructions to create 3 new files and modify
   2 existing files. When running `git status` you should see that 2 files are
   modified, and 3 are untracked (actually, the entire `large_data` directory
   will be shown as untracked).

    ```
    On branch main
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

10. Explore the difference between **`git add -u`** and **`git add --all`**.

    * **First let's see what `git add -u` does**: it updates the Git index with
      the new version of all files that are **already tracked** by Git (in our
      case these are `DESCRIPTION` and `README.md`). But it does *not* stage
      any new, untracked files. In our case, `personal_notes.txt` and the files
      in `large_data` thus remain untracked.

        ```yaml
        git status
        git add -u       # The long form of -u is: git add --update
        git status       # personal_notes.txt and files in large_data remain unstaged, because
                         # the "-u/--update" option only stages files that are already tracked.

        git reset HEAD   # Remove the staged changes for `DESCRIPTION` and `README.md`
                         # so we can test the "add --all" option.
        ```

    * **Now let's test what `git add --all` does:** it updates the Git index
      with all modified and untracked files. As can be seen, our 5 files
      (modified and new) have now been staged:

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

    :question:
    **Question answer:** the difference between `git add -u/--update` and
    `git add --all` is that using `--update` will only add files that are
    already tracked in Git, while `--all` will add all files (except ignored
    files), whether they are already tracked (modified) or not (untracked).  
    In a sense, `--update` is safer because it prevents you from adding
    completely new files to the Git repo by mistake.

11. let's unstage each of our 3 files with one of the possible commands to
    remove content from the git index:
    ```yaml
    git restore --staged personal_notes.txt
    git reset HEAD large_data/large_1.csv
    git rm --cached large_data/large_2.csv
    ```

    :question:
    **Question answer:**
    * The difference between `git rm --cached` and the two other commands is
      that `git rm --cached` will **unstage the entire file**, not just the new
      changes that were made to it since the last commit.
    * The reason why in this particular case `git rm --cached` does exactly the
      same as `git restore --staged ` and `git reset HEAD` is because the 3
      files that we unstaged are new and have never been added to the Git repo
      before.
      There is thus no difference between removing them completely, or just
      resetting them back in the index to their version from the latest commit
      (since they are all absent from the latest commit).

12. Since the content of `large_data` should be ignored by all copies of the
    repository, we ignore it using the `.gitignore` file.  
    `personal_notes.txt`, on the other hand, should only be ignored only by the
    local instance of our Git repo, and must therefore be added to
    `.git/info/exclude`.

    ```yaml
    echo "large_data" >> .gitignore
    echo "personal_notes.txt" >> .git/info/exclude
    git status
    ```

    At this point, the output of `git status` should look like this:
    ```
    On branch main
    Changes to be committed:
    	modified:   DESCRIPTION
    	modified:   README.md

    Changes not staged for commit:
    	modified:   .gitignore
    ```

13. Commit all remaining changes:
    ```yaml
    # Stage the .gitignore file (add it to the index).
    git add .gitignore
    git status

    # Commit the file.
    git commit -m "Update DESCRIPTION and README"
    git status                                      # The working tree should now be clean.

    # Show commit history of the current branch.
    git log --oneline
    ```


<br>
<br>
<br>


## Exercise 2 - The git reference web page
Change into the `exercise_2/` directory, and look at the current status of the
git repository.
```yaml
cd exercise_2/
git log
git log --oneline  # There are 3 commits in the repo.
git branch         # There is currently only 1 branch: main.
git status         # There is one tracked file with uncommitted changes: references.html
```

<br>

### A) Fix the broken "ProGit" link.
Make a new commit with the uncommitted changes:
```yaml
# 1. Check if the working tree is clean, and see uncommitted changes.
git status  # This shows that the references.html file has uncommitted changes.
git diff    # Display the uncommitted changes in the file.

# 2. Commit the changes.
git add references.html  # Stage the changes in the references.html file.
git commit -m "Add Git logo placeholder to the Git reference webpage"

# You can also use these shortcut for the above 2 lines:
git commit -m "Add Git logo placeholder to the Git reference webpage" references.html
# or
git commit -am "Add Git logo placeholder to the Git reference webpage"

git status  # There are no more uncommitted changes.
```

Fix the "ProGit" link in a temporary `fix` branch. After testing that the HTML
page works correctly, commit the change.
```yaml
# 3. Create a new "fix" branch and switch to it:
git branch fix
git switch fix
# You can use the following shortcut to create + switch to a branch in
# a single command:
git switch -c fix

# 4. Edit the HTML page, then verify in the browser that the link now works.
#    Note: you can use any text editor to do this.
vim references.html

# 5. Make a commit with the changes:
git add references.html                 # Stage your changes (i.e. add changes to the Git index).
git commit -m "Fix broken ProGit link"  # Make a commit.
# Here are shortcuts for the above 2 lines:
git commit -m "Fix broken ProGit link" references.html
# or
git commit -am "Fix broken ProGit link"
```

Merge the changes into branch `main` and delete branch `fix`.
```yaml
# 6. Merge "fix" into "main":
# Note that no additional commit is created by the merge, because this is a "fast-forward" merge.
git switch main
git merge fix    
git log --all --decorate --oneline --graph  # Display repo commit history.

# 7. Delete the "fix" branch:
git branch -d fix
git branch                                  # Verify that the "fix" branch is gone.
git log --all --decorate --oneline --graph  # Show repo history again.
```

<br>

### B) Add an image and new links to the HTML page
Add new links to the web page:
```yaml
# 1. Create and switch to a new "dev" branch.
git switch -c dev

# 2. Add the new references to the web page.
vim references.html     # Edit HTML page to add the new links...
git diff
git diff --cached
# After having checked that the two new links are working, stage and commit the changes.
git add references.html
git diff
git diff --cached
git commit -m "Add two new books to Git reference page"
```

:question:
**Question answer:** the difference between `git diff` and `git diff --cached`
is that the former will display the difference between the working tree
(files on disk) and the git index, while the later shows the difference
between the git index and the latest commit.

Add a Git logo to web page:
```yaml
# 3. Edit the HTML page to add logo.
vim references.html
git commit -m "Add git logo" references.html
```

Merge changes into `main`, delete branch `dev`:
```yaml
# 4. Merge "dev" into "main"
git switch main  # To merge "dev" into "main", we must be on the "main" branch.
git merge dev

# 5. Verify that both "dev" and "main" now point to the same commit.
git log --all --decorate --oneline --graph

# 6. Delete the "dev" branch.                             
git branch -d dev
```


<br>
<br>
<br>


## Exercise 3 - The crazy peak sorter script
Clone the peak sorter project from GitHub, test run the peak sorter script, and
display the Git repository's history.
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

<br>

### A) Add a fix to the `main` branch

1. Create a new `hotfix` branch and switch to it:
    ```yaml
    git switch -c hotfix
    ```

2. Apply Jimmy's fix using cherry-picking on a temporary `hotfix` branch.
   Note: the commit ID of the commit
   `"peak_sorter: add check that input table has the ALTITUDE and PEAK columns"`
   is `8e0d4fe`.
    ```yaml
    git log --all --decorate --oneline --graph  # Search for the commit ID of Jimmy's fix: 8e0d4fe
    git cherry-pick 8e0d4fe
    git log --all --decorate --oneline --graph  # Verify the commit was properly cherry-picked.
    ```

3. Test run the script to verify nothing is broken and have a look at the
   changes introduced by the cherry-pick.
    ```yaml
    ./peak_sorter.sh
    git show HEAD
    ```

4. Merge the fix back into `main`.
    ```yaml
    git switch main
    git merge hotfix

    # Verify that "hotfix" and "main" now point to the same commit.
    git log --all --decorate --oneline --graph

    # Test-run the script and delete the temporary "hotfix" branch.
    ./peak_sorter.sh
    ```

5. Delete the `hotfix` branch.
    ```yaml
    git branch -d hotfix
    ```

<br>

### B) Add the Dahu count feature to the `main` branch

1. Checkout the `feature-dahu` branch. Run the script to test that the
   "Dahu count" feature works.
    ```yaml
    git switch feature-dahu
    ./peak_sorter.sh

    # Display the repo history, so we can then compare it before and after the merge.
    git log --all --decorate --oneline --graph
    ```

2. Rebase `feature-dahu` on `main`. As there are conflicts, we need to manually
   resolve them by opening the conflicted file in an editor, resolving the
   conflict, then running `git add peak_sorter.sh` and `git rebase --continue`.
    ```yaml
    git rebase main

    # There are 3 conflicts, so repeat the steps below 3 times.
    vim peak_sorter.sh        # Open file in editor to solve the conflict.
    git add peak_sorter.sh
    git rebase --continue

    # When the rebase is completed, test-run the script to see if everything is
    # still working after the rebase.
    ./peak_sorter.sh
    ```

3. We are now ready to merge `feature-dahu` into `main`.
    ```yaml
    git checkout main
    git merge feature-dahu  # This is now a fast-forward merge -> no conflicts.

    # Test run to see if everything is still working after the merge.
    ./peak_sorter.sh        
    ```

   :question:
   **Question answer:** since we have rebased the branch `feature-dahu` on
   `main`, the merge will be **fast-forward** and there will be no conflicts.


<br>
<br>
<br>


## Exercise 4 - The Awesome Animal Awareness Project

#### A) Organize your team

Clone the Awesome Animal Awareness Project (A3P) repo.
```yaml
cd exercise_4/
git clone https://github.com/sibgit/sibgit.github.io.git
cd sibgit.github.io
```

The team leader creates the main development branch for the team and pushes
it to the remote repository on GitHub. The example here is for the team `yeti`.
```yaml
git switch -c yeti-dev       # This is a shortcut for git branch yeti-d + git switch yeti-dev
git push -u origin yeti-dev  # Note: -u is the short option for --set-upstream
```

Other team members can now pull the new team branch `yeti-dev` and check-it out.
```yaml
git fetch
git switch yeti-dev
git log --all --decorate --oneline --graph  # Display repo history and branches.
```

<br>

### B) Add content for your awesome animal

Each team member creates his personal work branch. The example here is for
`Alice`, working in team `yeti`.  
Alice edits the `yeti.html` page in her favorite editor. When she is done, she
loads the page in her browser to make sure the rendering is looking good. Then
she commits her changes.
```yaml
git switch -c yeti-alice
vim yeti.html             # Edit the web page.
git add yeti.html
git commit -m "Yeti: add habitat and distribution info."
```

<br>

### C) Merge your branch with your team's animal-dev branch

Each member of the team must now add the changes they made on their personal
branch to the team branch. Let's assume that Alice is either the second or
third team member to add her changes, and therefore she has to rebase her
branch `yeti-alice` onto `yeti-dev`.

1. Alice updates her local version of the team branch.
    ```yaml
    git switch yeti-dev
    git pull  # Fetch updates from remote and merge them with local yeti-dev branch.
    ```

2. Alice rebases her personal branch on the team branch.
    ```yaml
    git switch yeti-alice
    git rebase yeti-dev     # Conflicts will appear, so Alice must solve them manually.
    git add yeti.html
    git rebase --continue
    ```

3. Now that the rebase is completed, Alice merges her branch into `yeti-dev`.
   Then she pushes the updated `yeti-dev` branch to the remote and lets her
   team members know that she is done.
    ```yaml
    git switch yeti-dev
    git merge alice-dev  # This is now a simple fast-forward merge.
    git push

    # Optional: Alice can now delete her personal branch, if it is no longer needed.
    git branch -d alice-dev
    ```

<br>

### D) Create a pull request for the top-level management to verify and approve your work.

* Now that the yeti page is completed, the team leader can make a pull request
  to project owner on GitHub (i.e. one of the class teachers).
* When the request is accepted, the changes introduced in `yeti-dev` will be
  merged into the `main` branch of the project.
* All members of the team can now view their work at https://sibgit.github.io,
  and update their local git repository.
    ```yaml
    git switch yeti-dev

    # This check is optional, but can be used to make sure the merge from
    # "git pull" will be fast-forward.
    git fetch
    git status

    git pull
    git switch main
    git pull
    ```

<br>

### E) branch cleanup

* Delete the remote instance of the team branch.  
  :pushpin:
  **Note:** only 1 person in the team needs to do this.
    ```yaml
    git push origin --delete yeti-dev
    ```
* Delete local instances of the team branch and the personal branch.
    ```yaml
    git branch -d yeti-alice
    git branch -d yeti-dev
    ```
* Update the local instance of the `main` branch.
    ```yaml
    git fetch --prune
    git switch main
    git pull
    ```


<br>
<br>
<br>
