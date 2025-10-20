## Exercises - first steps (day 1)

<br>

[[_TOC_]]

<br>

## Configuring Git
Before starting with the exercises, make sure that you have done the minimal Git configuration by
setting your **user name** and **email** address in Git:
```yaml
git config --global user.name "First-name Last-name"
git config --global user.email "your.email@example.com"

# Examples:
git config --global user.name "Alice Smith"
git config --global user.email alice.smith@wonder.org
```

Optionally you can also change the **default editor** used by Git, e.g. if you are more
comfortable with `nano` than `vim`:  
```yaml
git config --global core.editor nano    # Set default editor to nano.
git config --global core.editor vim     # Set default editor to vim (the default).
```

To see the config values that are currently set, the commands are the following:
```yaml
git config --get user.name
git config --get user.email
git config --get core.editor

git config --list --show-origin  # Show all config values at once.
```

<br>  
<br>


## Exercise 1 - Your first commit [30 min]
**Objectives:** learn to initialize a new Git repository, add files to it, and make commits.  

Welcome to the first exercise of this Git course! This is a warm-up, so you will be guided
step-by-step on exactly what you need to do.

### Create a new repo, add content and make commits
1. Change directory into `/exercise_1` (shell command: `cd exercise_1/`) and list its content (
   shell command: `ls -l`).
   You will see that it contains (parts of) the source code of the R package `stringr`.

   Please do the following:
    * Initialize a new empty Git repository at the root of the `exercise_1` directory,
      so that you can version control its content.  
    * Run the `git status` command.
    * **Question:** what is the status of the files in your working directory?

2. Add all files to the Git repository (i.e. stage all files), except for `test_results.out`.
   Run the `git status` command again to make sure the staged content is correct. If you have
   staged your files correctly, the output of the `git status` command should look like this:
   ```
   Changes to be committed:
        new file:   DESCRIPTION
        new file:   LICENSE
        new file:   R/sort.R
        new file:   R/split.r
        new file:   R/stringr.R
        new file:   README.md
        new file:   tests/testthat.R
        new file:   tests/testthat/test-case.R

    Untracked files:
        test_results.out
   ```

3. Make a first commit in the repo with the message `Initial commit for fake stringr package`.

   Then, do the following:  
    * Run `git log` to display the repository's history.
    * Run `git show` to have a look at your commit.  
    * Run `git status` again.
    * **Question:** are there any untracked files left?
   <br>

   *Note:* depending on how much there is to print, `git show` will displays the content of a
   commit with the GNU program `less`:
    * Use your keyboard arrow keys to move up/down.
    * Press `q` to exit and return to the shell.
   <br>

4. If you have done things correctly there should be exactly 1 untracked file at this point:
   `test_results.out`. To stop this file from appearing as untracked, add it to a Git "ignore file"
   so that it gets ignored by all copies of the repo (**hint**: use a `.gitignore` file).

   Now run the `git status` command again: you should see that you still have an untracked file!
    * **Question:** how should we deal with this untracked file? Should it be added to the repo,
      or added to the ignore list?
    * Take the appropriate action with the untracked file, and make a new commit if necessary.
    * At the end of this step, you working tree should be "clean": when you run `git status`, the
      output should be:
      ```
      On branch master
      nothing to commit, working tree clean
      ```
   <br>

5. If you look at the content of the `README.md` file, you will see that it contains the string
   `MISSING` on two different lines. Edit the `README.md` file to replace the `MISSING` values
   by their correct content:  
    * The author of the stringr package: "Hadley Wickham"  
    * The URL of the package: https://cran.r-project.org/web/packages/stringr
   <br>

   Run the `git status` command again. You should see that the `README.md` file is now marked as
   "modified". To see the changes made to the file, run `git diff`.

   You can now commit the changes you just made:
    * Add/stage the changes you just made to `README.md` the Git repo with the `git add` command.
      Remember that each time you modify a file and want to include these changes into your next
      commit, you have to `git add` that file again.
    * **Tip:** to stage all modified files at once, you can use the `git add -u` shortcut command.
    * Run `git status` again: you should see that the `README.md` file is now listed under
      `Changes to be committed:` (in green).
    * Commit your changes with the message `README: add author and URL`.
   <br>

6. Display the (modest) history of your Git repo with the following variations of the `git log`
   command. Observe how history is displayed by each command:
    * `git log`  
    * `git log --pretty=oneline`
    * `git log --oneline`
    * `git log --all --decorate --oneline --graph`
   <br>

7. As you can see, with the current history of our git repo, there is not much difference between
   the outputs of `git log --oneline` and `git log --all --decorate --oneline --graph`. This will
   however change later when we start working with **branches**, and you will soon notice that the
   `git log --all --decorate --oneline --graph` command is very handy.

   So let's create a Git shortcut for it:  
    * `git config --global alias.adog "log --all --decorate --oneline --graph"`  
    * Test your new shortcut by typing: `git adog`
    * Your commit history should look like this (note: commit ID values will differ):
      ```
      * 8b14ba7 (HEAD -> master) README: add author and URL
      * b48e2c1 Add gitignore file
      * cd56e1e Initial commit for fake stringr package
      ```
    * **Tip:** to list your defined git aliases, you can use: `git config --list | grep ^alias`

<br>

### Additional tasks (if you have time)

8. Let's create some additional files at the root of the `exercise_1` directory: simply copy-paste
   the following 6 lines in your terminal (with the working dir set to `exercise_1`):  
   ```yaml
   echo "Let's keep this local" > personal_notes.txt
   mkdir large_data
   echo "A large file in the making..." > large_data/large_1.csv
   cp large_data/large_1.csv large_data/large_2.csv
   echo "adding a line" >> DESCRIPTION
   echo "adding a line" >> README.md
   ```
   This should create a new file named `personal_notes.txt`, as well as a directory `large_data`
   with 2 files in it: `large_1.csv` and `large_2.csv`.  
   It will also modify the existing files `DESCRIPTION` and `README.md`.

9. We now want to experiment the difference between the `git add --all` and `git add -u` commands
   (`-u` is a shortcut for `--update`).  
   Let's proceed as follows:
    * Run `git status`, and note the state of each file. You should have both modified and
      untracked changes.
    * Run `git add -u` followed by `git status`: note which files have been added.
    * Run `git reset HEAD` (we will see this command later in the course) followed `git status`:
      you should see that this command has removed all newly added content from the staging area.
    * Now run `git add --all` followed by `git status`: again, note which files have been added.
      You should now be able to answer the question below.
    <br>

   **Question:** what is the difference between `git add --all` and `git add -u`?

10. Unstage the following 3 files: `personal_notes.txt`, `large_data/large_1.csv` and
    `large_data/large_2.csv`.  
    *Note:* "unstaging" is a synonym for "removing from the git index".

    To unstage a file, you can use either of these commands:
     * `git restore --staged <file>`
     * `git reset HEAD <file>`
     * `git rm --cached <file>`
     <br>

    **Questions:**
     * The 2 first commands are identical, but what is the difference with `git rm --cached <file>`?
     * Why does `git rm --cached <file>` give the same result as the first two commands in this
       particular case?  
    <br>

11. Run `git status` to verify that you have successfully unstaged the 3 files (see point 10).
    They should now appear as "untracked".
    ```
    On branch master
    Changes to be committed:
    	modified:   DESCRIPTION
    	modified:   README.md

    Untracked files:
    	large_data/
    	personal_notes.txt
    ```
    To stop these files from appearing as untracked, add them to the correct ignore files so that:  
     * Files in `large_data` are ignored by all copies of the repo.
     * `personal_notes.txt` is ignored *only* by your local copy of the repository.
     * **Hint**: use the `.gitignore` and `.git/info/exclude` files. Note that `.git/info/exclude`
       is a file, not a directory.
    <br>

12. Commit the changes to the `DESCRIPTION`, `README.md` and `.gitignore` files with the message
    "Update DESCRIPTION and README".  
    Your working tree should now be clean - verify it with `git status`.  

    The output of `git status` should be the following:
    ```
    On branch master
    nothing to commit, working tree clean
    ```
    The output of `git adog` should be the following (note: commit ID values will differ):
    ```
    * 30e657b (HEAD -> master) Update DESCRIPTION and README
    * b6d778e README: add author and URL
    * fd570c5 Add .gitignore file
    * e50b5cc Initial commit for fake stringr package
    ```


<br>
<br>


## Exercise 2 - The Git reference web page  [30 min].
**Objectives:** learn to use branches when adding new features and bug fixes to a code base.  

To start the exercise, change directory into `/exercise_2`. You will see that it already contains
a Git repository, as well as an HTML page named `references.html`.  

1. Open the `references.html` page in your web browser.
2. Explore the content of the Git repo using the `git log`, `git status` and `git branch`
   commands to answer the following **questions:**  
    * How many commits have already been made in the repo?  
    * How many branches are present in the repo? How are they named?  
    * Are there any uncommitted changes?  

<br>

### A) Fix the broken "ProGit" link.
Your first task is to fix the broken link to the "ProGit" book in the HTML page. You can try to
click on the "ProGit" link in the page open in your browser, it should return an error.

Since **we want to follow good practices**, we will *not* work on this fix in the `master` branch.
Instead we will create a new branch, fix the link problem on that branch, test our fix, and then
merge it into `master` once we are confident the problem is solved.

1. Before working on our fix in a new branch, we need to make sure that our working tree is clean:
    * Check the git repo to see if there are any uncommitted changes.
    * If there are, display the uncommitted changes to see what they do. Even if you are not
      familiar with HTML, it should be easy enough to figure-out what the changes do.
   <br>

2. Now that you have figured-out what the uncommitted changes do, stage the changes and make a
   commit with a meaningful message. Verify that your working tree is now clean.

3. Create a new branch named `fix`.  

4. In the new branch, edit the `references.html` file to fix the link to the "ProGit" book.  
   **Hint:** add `https://` in front of the URL.  

5. Verify in your browser that the link is now working properly (note: you might have to reload the
   page). If it is the case, you can commit your changes.  
   Please **use a meaningful commit message**.

6. Merge your `fix` branch into `master`, then run `git log --all --decorate --oneline --graph`:
   at this point, the output should look like this:  
   (note: commit ID values will differ and your commit messages might be somewhat different)
   ```
    * 50a2e7f (HEAD -> master, fix) Fix broken ProGit link
    * e6a6176 Add Git logo placeholder to the Git reference webpage
    * 8a7444c Add Git logo file
    * c99fb57 Add links to Git resources
    * 5b54605 First commit. Add template for the Git reference page
   ```

7. Delete your `fix` branch as it is no longer needed.  
   Run `git branch` and/or `git log --all --decorate --oneline --graph` to make sure the `fix`
   branch was deleted.  

Enjoy your Git reference page. You can have a look at the different links if you want to learn
everything about Git!

<br>

### B) Additional tasks (if you have time): add an image and new links to the HTML page.

In this second part of the exercise, you are tasked to add a couple of links to new book on the
Git reference page.  
Here is what you should do:

1. Work in a new branch named `dev`.  

2. Make a commit that adds the following 2 references at the end of the list in the HTML page:  
   * `<li><a href="https://www.manning.com/books/learn-git-in-a-month-of-lunches">Learn git in a month of lunches</a></li>`
   * `<li><a href="https://www.amazon.com/Git-Porch-Willie-Crawford-2006-02-01/dp/B01K95YGYG">Git Porch</a></li>`  
   <br>

   **Notes:**
    * Use `git diff` and `git diff --cached` to look at your edits in the HTML file, before
      and after staging them.
    * **Question:** what is the difference between `git diff` and `git diff --cached`?
    * Check whether you did a proper job by refreshing the HTML page in your browser *before* you
      commit your changes.  
   <br>

3. Make a commit that adds the Git logo to the webpage: replace the placeholder line that starts  
   with `<!-- Add Git logo placeholder` by `<img src="git_logo.png">` in the HTML file.

   **Note:** check whether you did a proper job by refreshing the HTML page in your browser
   *before* you commit your changes.

4. When you have added these new features - and tested that they actually work by reloading the
   `references.html` page in your browser (new links are working, logo was added) - merge your
   `dev` branch into `master`.

5. Run `git log --all --decorate --oneline --graph` to display your entire repository's history.
   It should look like this:  
   (note: commit ID values will differ and commit message my be somewhat different)
   ```
    * ba4687a (HEAD -> master, dev) Add git logo
    * 30b149a Add two new books to Git reference page
    * 50a2e7f Fix broken ProGit link
    * e6a6176 Add Git logo placeholder to the Git reference webpage
    * 8a7444c Add Git logo file
    * c99fb57 Add links to Git resources
    * 5b54605 First commit. Add template for the Git reference page
   ```

6. Delete the `dev` branch, you no longer need it. Verify that is was deleted by running
   `git branch` and/or `git log --all --decorate --oneline --graph`.


<br>
<br>


## Exercise 3 - The crazy peak sorter script [30 min]
**Objectives:** get familiar with rebasing branches, solving conflicts and cherry-picking.

The stakes are raising! You are now taking the lead developer position of the "peak sorter"
project - a mesmerizing bash script that sorts all of the Alp's 4000 m summits in order of
decreasing elevation (i.e. highest to lowest). It also displays the name and elevation of the
highest summit in the Alps when completed.

* Start the exercise by changing your work directory to `exercise_3/` - it should be empty.
* Clone the data for exercise 3 with the command: `git clone https://github.com/sibgit/peak_sorter.git`.
* You should now have a new directory named "peak_sorter". Enter it with the command
  `cd peak_sorter`.
* To see the peak sorter script in action, run `./peak_sorter.sh` on the command line.  
* Have a look at the history of the git repo with `git log --all --decorate --oneline --graph`
  (or you can use the alias `git adog`, if you have created it).

<br>

### A) Add a fix to the master branch
As the new lead developer of the "peak sorter" project, your first task is to add an input data
integrity check to the `peak_sorter.sh` script. Luckily a former developer of the project, Jimmy,
has already worked on this issue and made a commit with the fix in his own branch named
`dev-jimmy`.

The message of the commit containing the fix is:
`"peak_sorter: add check that input table has the ALTITUDE and PEAK columns"`.  

Your first task is to take this commit and apply it to the `peak_sorter.sh` script. Proceed as
follows:  

1. Create a new `hotfix` branch (we wouldn't want to work directly on `master`, wouldn't we?).

2. **Cherry-pick** the commit that contains Jimmy's fix onto your `hotfix` branch.

3. When the cherry-pick is completed, test run the `./peak_sorter.sh` script to make sure nothing
   was broken. You can also have a look at the changes introduced by the cherry-pick with
   `git show HEAD` to review the latest commit.

4. When you are confident things are working as expected, **merge** your `hotfix` branch
   into `master`.

5. Delete the `hotfix` branch.

<br>

### B) Add the "Dahu count" feature to the master branch
Your next task is to add a new feature to the `peak_sorter.sh` script. This new feature, which will
take the peak sorter script to new heights, should add the number of
[dahus](https://en.wikipedia.org/wiki/Dahu) observations made on each alpine peak to the output
of the script.  

Again, you're in luck, because this feature has already been developed in a branch of the Git repo.
That branch is aptly named `feature-dahu`.  
Proceed as follows:  

1. Checkout the `feature-dahu` branch and test whether the new feature has been implemented
   properly - i.e. run the peak sorter script and check its output for Dahu counts. A
   column named `DAHU_POPULATION` should also be added to the output file `sorted_peaks.txt`,
   you can have a look with the command `head sorted_peaks.txt`.

2. Look at the **history of the git repo** with: `git log --all --decorate --oneline --graph`
   (i.e., `git adog`), so you can compare it before and after the rebase.

3. Rebase the `feature-dahu` branch on `master`. This will result in conflicts that you will have
   to manually resolve. In all 3 conflicts, always keep the version that is coming from the
   feature branch. It's the second version when you open the file to manually solve the conflicts
   (the version that is *not* coming from `HEAD`).

4. When you completed the rebase, try to run `./peak_sorter.sh` again to make sure it works: it
   should still display the number of Dahus observed on the Alps' highest peak as it did before
   the rebase.

   Look again at your repo's history (`git adog`). Compare it to what you had before the merge
   (scroll up in your terminal), to visually see how the rebase operations changed the history.

5. When you are confident that the new Dahu count feature is correctly implemented, you can merge
   `feature-dahu` into the `master` branch.

   **Question:** do you expect any conflicts to occur during this merge?

At the end of the exercise, your history (`git log --all --decorate --oneline --graph`) should
look like this:  
(note: some commit ID values will differ)
```
* 953de22 (HEAD -> master, feature-dahu) peak_sorter: display highest peak at end of script
* 337fac8 peak_sorter: added authors as comment to script
* 98bf029 peak_sorter: improved code commenting
* 825dccc peak_sorter: add Dahu observation counts to output table
* 6989be6 README: add more explanation about the added Dahu counts
* 348e3b6 Add Dahu count table
* aa3e112 peak_sorter: add check that input table has the ALTITUDE and PEAK columns
* f6ceaac (origin/master, origin/HEAD) peak_sorter: added authors to script
* f3d8e22 peak_sorter: display name of highest peak when script completes
| * fc0b016 (origin/feature-dahu) peak_sorter: display highest peak at end of script
| * d29958d peak_sorter: added authors as comment to script
| * 6c0d087 peak_sorter: improved code commenting
| * 89d201f peak_sorter: add Dahu observation counts to output table
| * 9da30be README: add more explanation about the added Dahu counts
| * 58e6152 Add Dahu count table
|/  
* cfd30ce Add gitignore file to ignore script output
* f8231ce Add README file to project
| * 1c695d9 (origin/dev-jimmy) peak_sorter: add check that input table has the ALTITUDE and PEAK columns
| * ff85686 Ran script and added output
|/  
* 821bcf5 peak_sorter: add +x permission
* 40d5ad5 Add input table of peaks above 4000m in the Alps
* a3e9ea6 peak_sorter: add first version of peak sorter script
```

**Notes:**
 * In a real application, you would now push your changes on `master` to the remote repository on
   GitHub with the `git push` command. In this exercise however, you do not have the permissions
   on the remote repository to perform this action.
 * Trying to delete the local branch `feature-dahu` with the safe `-d` option will not work,
   because changes on the branch have not been pushed to the upstream branch `origin/feature-dahu`.
   If you wanted to delete the `feature-dahu` branch, you would need to use the `-D` option to
   **force-delete** the branch: `git branch -D feature-dahu`.
 * In the situation where you would want to push changes in `feature-dahu` to the remote, you
   would need to use `git push --force` to overwrite the branch's history on the remote. This is
   because **rebasing a branch modifies its history**: the local and remote instance of the branch
   have now **diverged**.
 * To see this divergence in history, you can run `git log --all --decorate --oneline --graph` and
   look at the `feature-dahu` and `origin/feature-dahu` branches.


<br>
<br>


## Exercise 4 - The Awesome Animal Awareness Project [60 min]
**Objectives:** learn to collaborate with others on a project hosted on GitHub.

Congratulations! Your improving Git skills have not gone unnoticed, and you are now hired
by our agile "A3P" startup - the [Awesome Animal Awareness Project](https://github.com/sibgit/sibgit.github.io).

Your mission - should you choose to accept it - is to help us build new website. This is a
**collaborative effort**, and you will be working in teams of 3 people. Each team will contribute
a page of the website about a specific - and awesome - animal. At the end of the exercise, the page
created by your team will be integrated into the [Awesome Animal Awareness website](https://sibgit.github.io).

**Important:** please follow the **naming convention for branches** tightly, as our company's
internal Git policies are **very strict**.

**Before you start:** create a GitHub **personal access token** on GitHub. This will be needed in
order to allow you to push changes to GitHub. Please refer to the course slides for instructions
on how to create the token (a demo will also be made in the class before the exercise).

### A) Organize your team
1. Get together: go to the [course's google doc page](https://docs.google.com/document/d/1EX72NInz-eA2d2GOa5aTB8D88GWb91Sk-sCNHwQYXqE)
   and check which team you belong to by looking at the **Team** column in the GitHub user names
   table.
    * **On-site course:** locate your other team members in the classroom and sit together so you
      can communicate.
    * **Online course:** you will be placed in a breakout-room with your teammates. Remember that
      you can use the "share screen" function to help each other and show to other team members
      what you are doing on your machine.
   <br>

2. Change into `exercise_4/` and clone the project from our
   [GitHub repository](https://github.com/sibgit/sibgit.github.io).

3. Among your group, chose a "team leader". The team leader should create a new **team branch**
   named after your animal's name followed by `-dev`.  
   Examples: `tiger-dev`, `yeti-dev`, `sunfish-dev`, ...  
   After the branch is created, the team leader should push it to the repo on GitHub.

   **Important:** when you push a local branch to a remote *for the first time*, you have to
   indicate an "upstream" branch for the branch you are pushing.  
   This is done by using the `-u / --set-upstream` option of `git push`:
   ```yaml
   git push --set-upstream origin <branch you want to push>

   # Examples:
   git push --set-upstream origin sunfish-dev
   git push -u origin sunfish-dev                 # -u is the shortcut for --set-upstream
   ```

4. Other team members can now **fetch changes** from the repo and **checkout the new team branch**
   branch just pushed by the team leader.
    * **Important:** other team members should *not* create the team branch locally themselves.
      They should check it out from the remote.
    * For other team members, the `-u / --set-upstream` option is *not* be needed when they
      push changes to the team branch for the first time, because the upstream was automatically
      set when they checked-out the team branch from the remote.
   <br>

5. Each team member should now create their **personal branch**, using the following naming scheme:
   `<animal>-<your name>`, e.g. `tiger-sandra` or `pallas_cat-tom`. Note that this branch does
   not necessarily have to be pushed to the remote: it's your personal work branch, so it can stay
   local.

   *Note:* in a real application, you will probably want to push your personal branch to the remote
   as a backup (depending on how long-lived your branch is), but this is not necessary for this
   exercise.

<br>


### B) Add content for your awesome animal
Each team member will now contribute something to the web page of the team's animal.  

1. Switch to your **personal branch**.

2. On your personal branch, edit the HTML file corresponding to your animal. The topics that each
   member of the team has to work on are listed in the Google doc - see the **Task** column of the
   GitHub user name table. In the HTML file, the `??` mark the positions where you have to add your
   content.  

3. For the team members with the **task "Picture"**: find and download a picture of your animal
   from the world wide web. Add the image to project repo (at the root of the project directory)
   and copy the file name into `<img src='?? image-filename'>`, e.g. `<img src='tiger_image.png'>`.  
   **Important:** make sure to add the image file to your commit.

4. Check the rendering of your HTML file by opening it with your browser.

5. Commit your changes with a meaningful message.  

<br>


### C) Merge your branch with your team's animal-dev branch
Each team member should now merge the edits made on their personal branch (e.g. `tiger-sandra`)
into the team's main development branch (e.g. `tiger-dev`), and then push the changes back to the
remote repo on GitHub.

This is **best done as a coordinated, iterative, process**, where each member will in turn:

1. Do a `git pull` on the team's main development branch to make sure their local copy is
   up-to-date.  
   **Important:** verify that you are on the correct branch before doing `git pull`.  
   **Note:** in situations where you are not sure whether a remote branch has diverged from your
   local copy of it, you can do a `git fetch` + `git status` before running `git pull`. In this
   way you will see whether the merge operated by `git pull` will be fast-forward or not.
   Reminder: `git pull` is a shortcut for `git fetch` + `git merge <upstream>`.

2. Rebase their personal branch on the team's main development branch. Note that the first
   person to add their changes will in principle *not* need to rebase, since their personal branch
   will already be rooted at the latest commit of the team's main development branch.

3. Merge the changes from their personal branch into the team's main development branch.

4. Push the changes back to the remote repo on GitHub. Then let the next person know they can
   proceed.

<br>

After all team members have gone through the above steps, the team's main development branch
should contain the work of all 3 team members.

<br>

### D) Create a pull request for the top-level management to verify and approve your work.
Now that your team's main branch is all merged and clean, the team leader should
**create a pull request** for the team branch to be merged into the project's `master` branch.

1. Creating such a pull request is done online, in the project's [GitHub repository](https://github.com/sibgit/sibgit.github.io).
   In the **Pull requests** tab, click on **New pull request** (green button). For more details
   on how to create a pull request, please refer to the course slides.

2. Once the management approved your work and merged it into `master`, you should be able to see
   your animal's page on the [Awesome Animal Awareness website](https://sibgit.github.io)!

Well done! Enjoy your success by reading about your awesome animal and looking-up some of the
other teams' pages.

<br>

### E) Additional tasks (if you have time): branch cleanup
After your team branch has been merged into the `master` branch of the project, you can now
delete your team branch and personal branches (since they have been merged).

* Each team member can delete their personal branch and team branch from their local repo.
* One person in the team can delete the team branch on the remote.
  The command for this is: `git push origin --delete <branch>`.
* Each team member can update their local copy of the `master` branch.
  ```yaml
  git fetch --prune
  git switch master
  git pull
  ```
  **Note:** the **`--prune`** option **removes references to remote branches** (i.e.
  remote-tracking references) that no longer exist on the remote. For instance, if a team branch
  was deleted on the remote (after it was merged into `master`), then you might also want to
  delete your local reference to this remote branch.  
  The `--prune` option can also be passed to `git pull`: `git pull --prune`.

<br>
<br>
<br>
