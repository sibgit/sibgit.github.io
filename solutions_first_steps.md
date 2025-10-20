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

### Part A: create a new repo from scratch and make a first commit

1. To initialize a new Git repository, we must first **enter the directory** in
   which we want to track changes.
    ```sh
    cd exercise_1/test-project  # Enter the directory.
    ls -l                       # List files present the directory.
    ```

2. We can now **initialize a new Git repository** with `git init`.
    ```sh
    git init  # Create a new Git repository.
    ls -la    # We can observe that a new ".git" hidden directory was created.
    ```

3. **Running `git status`**, we see that (as expected at this point) all
   files are untracked.
    ```sh
    git status  # At this point, all files are untracked.
    ```

   Output of `git status`:
    ```txt
      On branch main
      No commits yet

      Untracked files:
        (use "git add <file>..." to include in what will be committed)
      	README.md
      	doc/
      	script.py
      	script.pyc
      	tests/
    ```

4. **Stage all files** except the `*.pyc` files. There are several ways we can
   do this:
   * By staging individual files:
      ```sh
      git add README.md
      git add script.py
      git add doc/
      git add tests/tests.py
      git add tests/output.csv
      # Note that we can also stage all 3 files in a single command:
      git add README.md script.py doc/ tests/tests.py tests/output.csv
      ```
   * By staging all files in the directory, then removing the `*.pyc` files
     from the staging area (Git index):
      ```sh
      git add --all  # Or we can also use `git add .`
      git rm --cached script.pyc tests/tests.pyc # Do not forget the --cached option,
                                                 # otherwise files are deleted on disk !
      ```
     Note that in this specific case we can not use `git restore --staged` to
     remove files from the Git index because we do not have any commits yet
     in the repository (and `git restore --staged` needs a commit to restore
     from).

   We then display the status the files in the working tree, we should see that
   all our files (except the `.pyc` files) are displayed as "new file" and are
   now ready to be committed.
    ```sh
    git status
    ```
    ```txt
    Changes to be committed:
         new file:   README.md
         new file:   doc/user-guide.pdf
         new file:   script.py
         new file:   tests/output.csv
         new file:   tests/tests.py

    Untracked files:
         script.pyc
         tests/tests.pyc
    ```

5. **Make a first commit**, then display the history of the repo and the status
   of the files in the working tree.
    ```sh
    git commit -m "Initial commit for test project"
    git log     # At this point the commit history contains a single commit.
    git show    # Show content of the last commit.
    ```

   :question:
   **Question answer:**
   Looking at the output of `git show`, we can see that the newly added content
   for the file `doc/user-guide.pdf` is not displayed (unlike for `README.md`
   for instance, where the content of the file is shown). The reason for
   this is that `user-guide.pdf` is a **binary file** (and not a plain text
   file), and Git does not display details for binary files (it simply lists
   the file has having been modified).

<br>

### Part B: commit an update to a tracked file

1. **Edit the file** `README.md` as instructed (nothing to correct).

2. **Show the differences in tracked files** between the working tree and the
   Git index (staging area) with `git diff`.
   ```sh
   # In this case, since only the README.md file was modified, both of the
   # following command produce the same result.
   git diff
   git diff README.md
   ```

3. **Stage the changes** to `README.md` and **make a new commit**.
   ```sh
   git add README.md
   git commit -m "Make README file more cheerful"

   # Alternative options: staging all modified files with "git add -u" (since
   # only README.md was modified, this results in the same as staging the
   # README.md file).
   git add -u
   git commit -m "Make README file more cheerful"

   # Alternative using "git commit" shortcuts to stage + commit in a single
   # command.
   git commit -m "Make README file more cheerful" README.md
   git commit --all -m "Make README file more cheerful"
   ```

<br>

### Part C: adding files to the `.gitignore` list

1. **Add `*.pyc` files to `.gitignore`**:
   After having create a `.gitignore` file and ignored the `*.pyc` pattern,
   we see that the two `.pyc` files are not longer listed as *untracked*.
    ```sh
    echo "*.pyc" > .gitignore
    git status
    ```

  However, we now have a new untracked file: the `.gitignore` file itself.
  Since the rules defined in the `.gitignore` file are useful to all users of
  the repository, this file should be added to the repo.

2. **Add a new commit with the `.gitignore`** file:
    ```sh
    git add .gitignore
    git commit -m "Add *.pyc to ignore list"
    git status  # -> nothing to commit, working tree clean
    ```

3. **Display the (modest) history of your Git repo**.
    ```sh
    git log                                     # Prints the full commit message along with author and date.
    git log --pretty=oneline                    # 1 commit per line. Full commit hash/ID (checksum).
    git log --oneline                           # 1 commit per line. Abbreviated commit hash/ID.
    git log --all --decorate --oneline --graph  # Shows commits for all branches.
    ```

   Create an `adog` alias Git command and test it:
    ```sh
    git config --global alias.adog "log --all --decorate --oneline --graph"
    git adog

    # List your defined Git aliases.
    git config --list | grep ^alias
    ```

<br>

### Additional Tasks

1. **Removing content from the Git index (unstaging)**.
    ```sh
    git add --all
    git restore --staged script.py          # Unstage changes to script.py
    git restore --staged personal_notes.md  # Unstage personal_notes.md

    # To unstage personal_notes.md, we can also use:
    git rm --cached personal_notes.md
    ```

2. **File staging shortcuts:** `git add --update` vs. `git add --all`.
   The difference between `git add -u/--update` and `git add --all` is that
   using `--update` will only add files that are already tracked in Git, while
   `--all` will add all files (except ignored files), whether they are already
   tracked (modified) or not (untracked).
   In a sense, `--update` is safer because it prevents you from adding
   completely new files to the Git repo by mistake.
    ```sh
    git add -u                      # Stage all modified files.
    git status                      # We can see that modifications in script.py are now staged.
    git restore --staged script.py  # Unstage the modifications.
    ```


3. **Ignore a file using `.git/info/exclude`**.  
    ```sh
    # Add "personal_notes.md" to the "exclude"  file:
    echo "personal_notes.md" >> .git/info/exclude
    cat .git/info/exclude  # Display the content of the file

    # personal_notes.md is no longer listed as untracked.
    git status
    ```

4. **Undo a change in your working tree**.
   `git restore script.py` overwrites the version of `script.py` present in
   the working tree with the version from the Git index.
    ```sh
    git restore script.py
    cat script.py           # The line "adding a bad line..." is gone.
    git diff                # Empty output, which is what we expect.
    git status              # No more uncommitted changes.
    ```


5. **Remove a file from the repository**.  
   We use `git rm <file>` to remove the file from both the Git index and the
   working tree. To delete the file only from the index we would use
   `git rm --cached <file>`.
    ```sh
    git rm tests/output.csv
    git status
    git commit -m "Remove test output"
    ```

6. **Retrieve `output.csv` from an older commit**.  
   The `--source` argument is used to indicate from which commit the file
   should be restored. You can pass a commit ID (hash), or use a reference to
   a commit such as `HEAD~1` in the solution below. `HEAD~1` refers to the
   parent of the current `HEAD`.
   ```sh
   git restore --source HEAD~1 tests/output.csv
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
