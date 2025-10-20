# Exercises - Git optional modules

<br>

[[_TOC_]]

<br>

:fire:
**About "Additional Tasks"**  
As you will see, some of the exercises contain an **Additional Tasks** section.
These are tasks that you can do if you have completed the regular part of the
exercise but there is still time in the exercise session. Additional Tasks will
not be corrected in class, but you can find corrections for them in the
exercise solutions.

<br>
<br>


## Exercise 5 - The Git reference web page gets better with submodules [40 min]
**Objective:** introduction to git submodule.

In this exercise, we will implement some improvements to two web pages using
**submodules**.  
The projects for these web pages are the following:
* The [git resource webpage](https://github.com/sibgit/git_resources_webpage),
  which you might remember from the *Version Control with Git – First Steps*
  exercises.
* The [useful tools webpage](https://github.com/sibgit/useful-tools), a page
  that shows a list of useful tools for the part of our lives that does not
  involve typing Git commands in a shell (so most of our lives, hopefully!).

<br>

### A) Clone the projects - including their submodule

1. Change into the `exercise_5` directory - it should be empty.
2. Clone the repositories for the 2 web pages (links above). These repositories
   contain submodules, so **make sure to also download the submodule content**.
3. Display the two web-pages `references.html` and `useful-tools.html` in your
   local browser (one is in each of the repo you just cloned). They should both
   show a *glitter effect* when moving the mouse pointer.
4. Have a look at `.gitmodules` to see what submodules are in these projects.

:question:
**Questions**:
* Which command would you use to update the content of the
  `git_resources_webpage` and `useful-tools` repositories (so that changes
  also affect submodules)?
* Where would you need to run this command? In the main project (e.g.
  `git_resources_webpage`) or inside the `glitter-cursor` submodule directory?

<br>

### B) Change the style of the web sites by adding a submodule

1. Go to [the great-style GitHub page](https://github.com/sibgit/great-style)
   in your browser and create a **Fork** of the project. To create a fork,
   click on the **Fork** button, near the top right of the project's GitHub
   web page.

   This will create **a copy** of the original project under your GitHub/GitLab
   account. You will be the **owner** of this forked project, and therefore you
   will be able to push commits to it (which you don't have permission to do on
   the original project).

2. Add the **forked** *great-style* repo as a new submodule to both
   `git_resources_webpage` and `useful-tools`.  
  :fire:
  **Important:** make sure to use the **forked** project, and *not* the
  original project!

3. Refresh the two web pages in your browser, they should now have another
   style. If the new style is working as expected, **commit the changes**
   (i.e. the addition of the new submodule) in your two local repos
   (`git_resources_webpage` and `useful-tools`) with the commit message:
   `"Add great-style submodule"`.

4. Have again a look at the `.gitmodules` files to see how they changed.

<br>

### C) Improve the great-style submodule
If you look at your web pages, you can notice that the colors of the hyperlinks
change rather quickly, to the point where we cannot enjoy this fabulous great
style to its full extent. So let's slow that down a bit.

We will first do the changes from within the project `git_resources_webpage`
and then pull the latest version of the submodule in `useful-tools`.

1. Open the file `git_resources_webpage/great-style/great_style.css` in a text
   editor, and change the `--anim-cicle-time` value to 10s (line 4 of file).  
   Reload the web page `references.html` in your browser, and verify that the
   colors now change more slowly.

   Wait... did we just make a great style even greater? *Yes, we did !*

2. Let's keep these changes for posterity by committing them to the
   `great_style` submodule and then pushing them to the remote (your forked
   *great-style* remote).  

   :pushpin:
   **Note:** if you get a message telling you that you are not allowed to push,
   then it's probably because you have added the original *great-style* project
   as a remote, and not your forked version.

3. Almost done! All that is left to is to update the *git_resources_webpage*
   repository (the super-project) so that it points at the new version of
   *great-style* that we just committed.

   Go back to the root of `git_resources_webpage`, and commit the changes (the
   update in the version of the *great-style* submodule) with the message
   `"Update great-style submodule with slowed-down color changes"`  

   :question:
   **Question**: in the current setup, you cannot push the changes in
   `git_resources_webpage` to the remote repo because you do not have write
   permission on it. However, if you did, what would be the command to push
   your changes in both *git-resources* and its submodules all at once?

<br>

### Additional tasks (if you have time)

4. Finally, you should now update the *great-style* submodule also in your
   local `useful-tools` repository:
    * Enter the `useful-tools` repo and get the new version of the
      *great-style* submodule.
    * Reload the page *useful-tools.html* in your browser and verify that the
      colors now also change more slowly.
    * Commit your changes.


<br>
<br>
<br>


## Exercise 6 - Git LFS (Large File Storage)
**Objective:** learn to use Git LFS in two different use cases: starting with a Git repo from
scratch, and starting with an already existing Git repo.

### A) Create a new Git repository from scratch with Git LFS file tracking [20 min]
In this first part of the exercise, you are asked to create a new Git repository from scratch,
and have certain types of files tracked by Git LFS.

1. Change directory to `exercise_6/test_lfs.git`. Initialize a new empty Git repository, and make
   a first commit that only adds the `README.md` file.

   :fire:
   **Important:** if you have not run the `git lfs install` command yet (at
   least once) on your system, please do it now.

2. Make a second commit that adds all files present in the directory. However, the following files
   should be tracked by Git LFS (and not by Git directly):
    * all `.fasta` files located in the directory.
    * the `logo.png` file.
    * the `db_file.txt` located in the `db` sub-directory.
   <br>

3. Display the list of files tracked by Git LFS. Verify that the correct files are being tracked
   (you should have a total of 4 LFS-tracked files).

4. Display the content of your latest commit with `git show HEAD`.  
   :question:
   **Question:** why is the content of the files tracked by Git LFS not displayed?

5. Make some edits to both `README.md` (e.g add a line with some text) and
   `sequences/sample_A.fasta` (e.g. delete a few lines from the file). Then
   make a new commit with these modifications, and display the difference
   between your last commit and the one just before using the `git diff`
   command.

   :question:
   **Question:** what is the difference in the way that `git diff` displays the
   differences between regular files and files tracked by Git LFS?

<br>

### B) Migrate an existing directory to Git LFS [40 min]
In this part of the exercise, you will start with an existing Git repo named
`arctic_animals.git`. As its name suggests, this repo contains images of arctic
animals in different formats.

1. **Explore the history** of the Git repository located at
   `exercise_6/arctic_animals.git` and check the following:
    * How many branches are there in the repository?
    * Are there any files that are not currently tracked by Git in the
      working tree?
    * Run the following commands:
        ```yaml
        du -sh .git/objects  # Size of Git object store.
        du -sh .git          # Size of entire Git database for the current repo.
        ```
      :question:
      **Question:** what is the size of the Git "object store" at this point?
      And the size of the entire git database?
      :pushpin:
      **Note:** `.git/objects` is Git's "object store", the part of its
      database where it stores the actual data contained in the files it keeps
      track of (in terms of space usage, the other parts of the git database
      are small, as they essentially contain pointers to the objects kept in
      the object store).
   <br>

2. **We now want to use the `git lfs migrate` command** on the
   `arctic_animals.git` Git repo so that all image files (`.tif` and `.jpg`)
   become tracked by Git LFS rather than by Git itself.
    * Before starting the migration, run `git log --all --decorate --oneline --graph`
      (so we can later compare the commit ID values before and after the migration).
    * Re-write the repo's history with `git lfs migrate`, move all `.tif` and
      `.jpg` files to Git LFS.
    * Run `git log --all --decorate --oneline --graph`. Compare the commit ID
      values with what you had before the migration. You should see that all
      commits in the history of the repo are now different.

3. **Try to open the image files** located in `mammals` and `birds`: can you
   see the images? If you cannot, that is normal, because files are replaced by
   their pointers during the migration.
    * Run the correct `git lfs` command to restore the files in the working
      directory.
    * List all files that are tracked by Git LFS. Make sure they are all `.tif`
      and `.jpg` files. If the migration went correctly, you should be tracking
      the following files with Git LFS:
        ```
        ea67d4f8a5 * birds/animal_atlantic_puffin.tif
        8c92a37ac7 * birds/animal_snowy_owl.jpg
        6b58d6ac09 - fish/animal_pink_salmon.tif
        510d5b477b * logo.jpg
        1aa9297470 * mammals/animal_arctic_fox.jpg
        2519e9a194 - mammals/animal_blue_whale.tif
        47f090bd68 - mammals/animal_muskox.jpg
        3e45f949e2 * mammals/animal_walrus.tif
        4cf1383168 - birds/animal_emperor_penguin.tif
        642c1360fe - birds/animal_gentoo_penguin.jpg
        ```
    * Let's check the size of the Git object store again, as well as the size
      of the LFS object cache (i.e. the directory that contains a cached copy
      of the files tracked by LFS).  
      You can get the respective sizes of these directories with:
      ```yaml
      du -sh .git/objects
      du -sh .git/lfs
      ```
      :pushpin:
      **Note:** as you can see, the data seems now to be duplicated between the
      LFS cache and the git object store, because the size of the object store
      has not decreased compared to when we first checked-it. However, this is
      only temporary: at some point Git will do garbage-collection of all the
      commits from the old history, and the copy of the files present in the
      `.git/objects` directory will be removed.  
      To force immediate removal of the files, you can manually trigger
      garbage-collection:
       ```yaml
       git reflog expire --expire-unreachable=now --all
       git gc --prune=now
       ```
      If you check `du -sh .git/objects` again after this operation, you will
      see that the size of the Git object store is now down to < 40 KB! All the
      files now tracked by LFS have been deleted from it.

4. At the root of the `exercise_6` directory (i.e. outside of the
   `arctic_animals.git` repo), you should find an untracked file named
   `animal_humpback_whale.bmp`. Add this image to the Git repo as an
   LFS-tracked file (please place it in the correct subdirectory). Then make a
   new commit for it on the `dev` branch.  
   When the commit is made, go back to the `main` branch.  
   <br>

5. We would now like to push this repository to GitHub. For this you will first
   need to **create a new empty repo on GitHub**.

   Go to your GitHub account and create a new **private repo** named
   `arctic_animals`. If needed, you can have a look at the help slides for
   exercise 4 to get detailed instructions on how to do this. Since we will be
   importing an existing repo, don't add any file upon initialization of the
   repo on GitHub.

6. **Push your changes on both `main` and `dev` to the remote**. Remember that
   for this you will need to:
    * Add the newly created repo on GitHub as a remote to your local repo with:
      `git remote add origin https://github.com/<GitHub user name>/arctic_animals.git`
    * Use the `-u/--set-upstream` option of the push command the first time you
      push a new branch to the remote (this defines an upstream reference for
      the branch).

   After you completed the push, have a look at your repository on GitHub. You
   should now see that all your files have been pushed to GitHub, and if you
   click on a file tracked by Git LFS, its content should not be displayed -
   instead you should see a `Stored with Git LFS` message.
   <br>

7. Now that all files have been pushed to the remote on GitHub, we can safely
   **clear unnecessary files** from the local Git LFS cache (e.g. files that
   are in the history but not part of the current working tree).
    * Try to first run the relevant command in "dry run" mode.
    * How many files were deleted and how much disk space could you reclaim?  
   <br>

8. Let's now put ourselves in the shoes of another user of our `arctic_animals`
   repository:
    * **Clone the repository** in a different location of your file system.
      After you cloned your repository (you should be on branch `main` by default),
    * :question:
      **Questions:**
       * What is the size of the `.git/objects` and `.git/lfs` directories in
         your newly cloned repo?
       * Compare it to the size of these directories in your original repo.  
         Why is there this difference of size?  
   <br>


### Additional Tasks: test a "force" push to your repo

9. Go back to your original repo (not the copy) and switch/checkout the `main`
   branch. Looking at the history of the branch, you should see that its latest
   commit is a commit that removed emperor and Gentoo penguins from the repo.
   While this was a smart move - these penguins are not found in the arctic -
   the author of the commit unfortunately forgot to also delete these two
   species from the `birds/bird_list.txt` file.  

   Your task is to:
    * Fix the last commit on `main` by removing the two species from the
      `birds/bird_list.txt` file.
    * Rebase branch `dev` on `main` after the fix.  
   <br>

   When this is completed, you can push your modifications on `main` and `dev`
   to the remote:
    * Start by trying a regular push: `git push main` or `git push dev`.
    * Since we modified the history of our branches, git will not let us do a
      regular push...  
      Run the `git push` command again but adding the option to allow history
      overwriting on the remote.

<br>

### Cleanup
When you are done with your exercise, you can delete your repo on GitHub, so
you don't waste some precious storage space:
 * In your repo on GitHub, go to **Settings**.
 * Scroll to the bottom of the page. You will see a **"Danger Zone"** box.
   Click on **"Delete this repository"**.


<br>
<br>
<br>
