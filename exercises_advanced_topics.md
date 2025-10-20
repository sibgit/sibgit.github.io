# Exercises - Git advanced topics

<br>

[[_TOC_]]

<br>

## Exercise 1 - The vim cheat-sheet rebase  [40 min]
**Objective:** learn to use **interactive rebase**.

In this exercise, your task is to perform a cleanup on an existing repository that contains
instructions on how to use the `vim` editor.

**Note:** in this exercise we will be **rewriting history** on the `master` branch of our
repository. In a *production* environment, this **should ideally be avoided**.

1. Enter the `exercise_1/vim_cheatsheet.git` repository and have a look at it's history.  
   You can also display the content of the files - if you are using vim as an editor in Git,
   it might even be of interest to you.

   Checkout the `dev` branch, and look at which files it contains. Then go back to the `master`
   branch.

2. Looking at the **commit history** on the `master` branch, you will see that edits to the
   different files are all over the place. To make things more tidy, we first would like to
   group (in the history's chronology) the edits to each of the 3 files:
   `README.md`, `normal_mode.md` and `command_mode.md`.

   Your first task is thus to re-order the commits on the `master` branch in the following order
   (here listed from oldest to newest):
   ```
    1. First commit of the vim cheat-sheet project. Add README file
    2. Edit README file
    3. Edit README file: add open file example
    4. Add 'normal_mode' file: commands for vim's normal mode
    5. Edit 'normal_mode' file: add 'dd' and 'yy' commands
    6. Edit 'normal_mode' file: add more commands
    7. Add 'command_mode' file: commands for vim's command mode
    8. Edit 'command_mode' file: add more commands
   ```

3. After you are done with the re-ordering, display the history of the repo using the
   `git log --all --decorate --oneline --graph` command (or `git adog`, if you have created this
   alias).

   You should see that branch `dev` still contains the two "old" commits (`8ec5c65` and `eb8d137`)
   from the shared history it had with  the `master` branch before the rebase. These two commits
   are now duplicated between the two branches.  
   To get rid of them, please rebase `dev` on `master`, then return to `master`.

4. All things considered, our repo history would look cleaner if, for each file, we had a single
   commit rather than having multiple small and incremental commits.  
   For each file, merge the different commits so that your history looks like this (the oldest
   commit is here listed at the bottom of the list):
   ```
    * Add 'command_mode' file: commands for vim's command mode
    * Add 'normal_mode' file: commands for vim's normal mode
    * Edit README file: add vim modes and open file example
    * First commit of the vim cheat-sheet project. Add README file
   ```

   **Please note:**  
    * The commit message for the second commit is a new message. You will need to
      **edit the commit message** during the rebase.
    * The `README.md` file still has 2 commits - because it's first commit is also the first
      commit of the repo.  
   <br>

5. Display the history of the repo using `git log --all --decorate --oneline --graph`. You will
   see that, again, `dev` and `master` have diverged with duplicated commits. Rebase `dev` on
   `master` to get rid of the duplicated commits.
   When you are done, switch back to branch `master`.  

   **Hint:** use interactive rebase to easily get rid of the duplicated commits.  

6. As a final step, we want to fix a small bug in the `README.md` and `normal_mode.md` files:
    * In the `README.md` file, remove the line starting with "TODO:".
    * In the `normal_mode.md` file, remove the two last lines which contain duplicated commands.
      (the "dd" and "yy" commands are listed twice).
   <br>

   For *each file* to correct, create a `--fixup` commit (so you create 2 fixup commits). You
   should carefully select to which commit the fixup applies. Then rebase using `--autosquash`.
   When you are done, rebase `dev` on `master` again (**Hint:** to easily skip a commit, you can
   either use interactive rebase as before, or use `git rebase --skip` when a conflict arises -
   see also the message that git is printing when a conflict is detected).

   After the rebase, switch back to the `master` branch and display the history of your repository
   with `git log --all --decorate --oneline --graph`.  
   It should look like this (note: commit ID values will differ):
    ```
    * 062a1e2 (dev) Maybe we should all switch to emacs...
    * 07e7d86 Add image file showing a visual overview of vim modes
    * 9a0188b (HEAD -> master) Add 'command_mode' file: commands for vim's command mode
    * 4c17a23 Add 'normal_mode' file: commands for vim's normal mode
    * 3becef8 Edit README file: add vim modes and open file example
    * 0fbf901 First commit of the vim cheat-sheet project. Add README file
    ```

<br>

### Additional tasks (if you have time)
7. There are 2 more things you can try:
    * Merge the first 2 commits of the repo into a single one: for this you will need to use
      the **`--root`** option of the `git rebase` command (for details, see the course slides).
    * Change the commit message of the latest commit of the `dev` branch from
      "Maybe we should all switch to emacs..." to "Add xkcd comic image file".


<br>
<br>



## Exercise 2 - The big reset [40 min]
**Objective:** get familiar with `git reset` and its `--mixed`, `--soft` and `--hard` options.

In this exercise, you will build a new Git repo to keep track of your favorite Chuck Norris
quotes. Please note that, in order show you some use cases of `git reset`, we will
**intentionally make some mistakes**, so that we can then correct them. So don't be unsettled by
the fact that this exercise keeps asking you to change things: it's by design!

Let's get started:

1. Change into the `exercise_2/the_big_reset.git` directory, where you will find a number of files.
   Your first task is to turn this directory into a Git repository.

2. Add the `README.md` file to the Git repo, then make a first commit with the commit message
   "First commit" (or any other message you like).

3. Now add the `my_quotes.md` file to the git repo, and make a new commit with the message
   "Add a file to keep track of quotes".

4. Wait, that commit message is not nearly dramatic enough for what is about to unfold in this
   exercise. Let's change it to: "Starting The Big Reset".  
   **Note:** while the optimal way to do this is using `git commit --amend`, we here suggest that
   you try to use `git reset` (with either `--soft` or `--mixed` options).

5. Actually, there's still a thing we forgot: there is a `chuck_norris_quotes.txt` file in the
   repo - but we will not track it. So put it on the list of files to be ignored by Git and make
   this change part of the very *first* commit of the history (i.e. the file you added to ignore
   `chuck_norris_quotes.txt` should be part of the first commit).  
   **Hint:** you will need to use a combination of reset and amending.

6. Alright, fixing stuff is fun, but let's try to get some real work done here. Open the
   `chuck_norris_quotes.txt` file and select your 3 favorite quotes. Add each of your 3 quotes
   successively to the `my_quotes.md` file, making a new commit **each time** you add a quote (so
   after that, you should have 3 additional commits in your history, and a total of 5 commits).

7. With so many epic quotes to chose from it was hard to select only 3! But wait - Git has a
   solution for us: **branches**! Let's create an alternate branch, where we can have 3 other
   favorite quotes.  
   Create a new branch - let's name it `second_choice` - at the point in history where the
   `my_quotes.md` file was still empty. Then checkout/switch to your new `second_choice` branch
   and verify that `my_quotes.md` does not contain any quotes.

8. Add 3 new quotes to the file, and commit. Make a single commit with all quotes this time.  
   Then, have a look at your repo's history with `git log --all --decorate --oneline --graph`.

9. Upon reflection, why not put all of our favorite quotes into the `my_quotes.md` file: who said
   we could have only 3! To do this, merge branch `second_choice` into `master`. This will be a
   **3-way merge** (i.e. non-fast-forward) - so get ready for some **conflict resolution** action.

10. After the merge is done, let's look at the history of our Git repo again with
    `git log --all --decorate --oneline --graph`. You can see that, as expected, the 3-way merge
    introduced an additional **merge commit**, so our history is not as clean as could be.

    Can we fix it? *Yes, we can!*
     * Revert the repository to its state before the merge (**hint:** time to put that `--hard`
       option to good use!).
     * Rebase `second_choice` on `master`, before merging `second_choice` into `master`.
    <br>

    When you are done, run `git log --all --decorate --oneline --graph` and marvel at your
    perfectly clean history.


<br>
<br>


## Exercise 3 - The backport [30 min]
**Objective:** learn to use `git stash` and `git tag`, understand the **detached HEAD mode**.

You are busy working on the `dev` branch of a data analysis pipeline, when you get a message from
a user: they want you to backport the support for a new type of input data onto an old version
of the pipeline (version 1.0.1). You try to argue that this is an old version with security
vulnerabilities, and that they should not use it anymore... but they *just don't care*. As a
problem never comes alone, they need this *right now*.

1. Change into `exercise_3/backport.git` and have a look at the Git repository's history.
   Note the different branches and tags.  
   **Reminder:** you can use `git log --all --decorate --oneline --graph` to display the entire
   repo's history (or `git adog`, if you have created this alias).

2. Implementing the backport is urgent, so you will finish your work on the `dev` branch later.
   Stash away any uncommitted change, so you don't lose your hard work, then go back to version
   `1.0.1` of the project.
    * **Question:** where is the `HEAD` pointer now?
    * **Question:** has anything else changed in the repository's history?
   <br>

3. Implement the backport fix:
    * In the file `functions.py`, change the line `if extension in [".img", ".txt", ".csv"]:` to
      `if extension in [".img", ".txt", ".csv", ".json"]:`.
    * Then commit your change.
   <br>

4. Go back to the `dev` branch. As you leave, you should see a warning message from Git.
    * **Question:** what would happen to our latest commit if we "leave it behind"?
    * Take Git's advice and create a new branch named `backport` that points to the commit we
      just left behind.
    * While we are at it, let's also add a **tag** `v1.0.1b` to our new commit.
   <br>

5. Technically, after tagging our new commit with `v1.0.1b`, the `backport` branch is not really
   needed anymore, since a tagged commit will not be garbage collected. So let's delete this
   branch.

6. That's it. Now we can finally get back to work on `dev`.
    * Restore the stashed content on `dev`
    * Commit it and you're done (you can use commit message "Improve file import tests").
   <br>

   At this point, your git history should look like this:
    ```
    * 0bc9428 (HEAD -> dev) Improve file import tests
    * a9096d0 Add tests for import module
    * fe0cdf5 Rename import module file
    * 08470b9 Add new module for file import
    | * 1a2fa0c (tag: v1.0.1b) Backport: add support for 'json' file type
    | | * b7d3e86 (master) Improve code documentation
    | | * 5efa0e0 (tag: v1.0.2) Improve tests
    | |/  
    |/|   
    * | 81fcc20 Add new module for file export
    |/  
    * d2c319e (tag: v1.0.1) Add tests
    * 8f93dc0 Add import_file() function
    * a67a81f Add random class
    * 7332db6 (tag: v0.0.1) Add first version of code
    * e3c1543 First commit. Add README
    ```

<br>

### Additional tasks (if you have time)
7. Update the `master` branch with the new commits from `dev`:
    * Rebase your `dev` branch onto `master`, before merging it into `master`.
    * Add a new tag `v2.0.0` to the latest commit on `master`.
   <br>

   Your git history should now look like this:
   ```
   * 7f37744 (HEAD -> master, tag: v2.0.0, dev) Improve file import tests
   * 9065d06 Add tests for import module
   * cd61372 Rename import module file
   * c6859d4 Add new module for file import
   * b7d3e86 Improve code documentation
   * 5efa0e0 (tag: v1.0.2) Improve tests
   * 81fcc20 Add new module for file export
   | * 1a2fa0c (tag: v1.0.1b) Backport: add support for 'json' file type
   |/  
   * d2c319e (tag: v1.0.1) Add tests
   * 8f93dc0 Add import_file() function
   * a67a81f Add random class
   * 7332db6 (tag: v0.0.1) Add first version of code
   * e3c1543 First commit. Add README
   ```

<br>
<br>


## Exercise 4 - The treasure hunt [120 min]
**Objective:** collaborative exercise to review the Git commands and concepts seen during the
course.  
For participants who wish to receive **ECTS credits** for this course, this exercise also serves
as exam - please see the instructions at the end of the exercise.

We have saved the best for last! In this collaborative exercise, you will
**team-up with two fellow pirates** in order to retrieve the lost treasure of the feared pirate
John Longsilver.  
Get ready for a quest that will be *legend* ... wait for it ... *dairy !!*

**Introductory notes:**  
While this exercise is somewhat *gameified*, it nevertheless covers many of the important
operations and collaborative workflows you would encounter while doing real work. For instance:
* Each of the *quests* you will complete in this exercise can be seen as the equivalent of
  adding a **new feature** to a software or data analysis pipeline.
* Completing a quest, merging your work into the `main` branch and adding a tag, would be the
  equivalent of making a **new release** of your work/software.

At least during the first 2 quests (sections C and D), you will be asked to follow a
**collaborative workflow** that uses the following branches:
* **`main`** is the *production* branch, i.e. the branch on which only final, production ready,
  material is published. Do *not* work directly on the `main` branch.
* **`devel`** (for "develop") is the branch where the team will consolidate each "feature" (i.e.
  each quest of the treasure hunt) before merging it to `main` when a quest is completed.
* Short-lived **personal branches** (feature branches) will be created by each team member to add
  their work, before merging it into `devel`. As this is an exercise and we do not have much time,
  the personal branches will only contain 1 (or sometimes 2) commits before they get merged into
  `devel`, but you can imagine that in a real application more commits would be added.

:fire: **Important** - please pay particular attention to:
* Keep a **clean history** by avoiding unnecessary merges, and rebase branches to be merged before
  merging them whenever possible.
* Use **clear and meaningful commit messages** for your commits.
* **Delete temporary branches** that are no longer needed (e.g. a personal feature branch).
* Please pay particular attention to these points if you are taking this exercise as **exam**.
* Make sure to **carefully read** the instructions and **hints** given for each section, before
  starting the work.

<br>

### A) Organize your crew
1. Get together: go to the [course's google doc page](https://docs.google.com/document/d/1EX72NInz-eA2d2GOa5aTB8D88GWb91Sk-sCNHwQYXqE)
   and check which team you belong to by looking at the **Team** column in the GitHub user names
   table.
    * **On-site course:** locate your other team members in the classroom and sit together so you
      can communicate.
    * **Online course:** you will be placed in a breakout-room with your teammates. Remember that
      you can use the "share screen" function to help each other and show to other team members
      what you are doing on your machine.
   <br>

2. Organize your crew: within your pirate team, distribute the following roles:
   **Captain**, **Quartermaster** and **First-mate**. If your crew is less than 3 people, assign
   one person to multiple roles (e.g. one person is Captain and the other person is both
   Quartermaster and First-mate).

<br>

### B) Create a shared Git repo on GitHub/GitLab
1. Go to `exercise_4/treasure_hunt.git`. As you will see, this is a Git repository that already
   contains some files and commits.

   :warning:
   **IMPORTANT note for Windows users:** please run the following command while *inside* the git
   repository: `git config core.filemode false` (do *not* add the `--global` flag, so that this
   change only affects the current repo). This will instruct Git to ignore differences in
   "execution permissions" in files, and is needed because Windows does not use the same permission
   system as Linux/MacOS.

2. The first thing you need to do before embarking on your quest is to **setup a remote repository**
   on [GitHub](https://github.com) or [GitLab](https://about.gitlab.com), so that you can use this
   as a shared repository to synchronize your local Git repos within the team.
    * As you only need one remote for the entire team, select one person of your crew who will
      create a new project/repo under their GitHub/GitLab user account.
    * The repo should be **public**.
    * Once the repo is created, the owner must **give access to the repo** to the other members
      of the crew.
    * For instructions about how to create a new repo on GitHub and give access to the repo to
      other people, please see the course slides (Exercise 4 help slides).
   <br>

3. All members of the crew should add the newly created remote repository as a **remote** to their
   local copy of `treasure_hunt.git`.  
   In addition:
    * One pirate should push the `main` branch to the remote.
    * The other pirates, should set `origin/main` as the **upstream** for their `main` branch.
      This is done with the command: `git branch --set-upstream-to=origin/main`.

<br>

### C) Treasure hunt part one: sign the pirate code
Even pirates have rules! To avoid disputes at sea, you and your fellow pirates should start your
journey by carrying-out a little paperwork and signing the
[pirate code](https://en.wikipedia.org/wiki/Pirate_code).

Signing the **pirate code**, and thereby officializing your participation and rank in the treasure
hunt, is done by adding your name next to your role at the bottom of the `pirate_code.md` file
located at the root of the git repo.
Please **pay particular attention to articles IX, X and XI of the contract!**

To properly edit this file and share your edits with your fellow pirates, proceed as follows:

1. Enter the `treasure_hunt.git` git repo. You should be on a branch named `main`, the main branch
   of the quest.
2. The **Captain** should create a new branch named `devel` (for "development"), and push it to the
   remote. This is the branch on which the team will consolidate their work before merging it
   into `main`.
3. The other crew members can now update their local repos with the new `devel` branch.
   For the time being, this branch is pointing at the latest commit of the `main` branch.  
   **Important:** other team members should *not* create the `devel` branch locally themselves.
     They should check it out from the remote.
4. Each pirate must now sign the `pirate_code.md` and share the changes with the other members.  
   Proceed as follows:
    * Each pirate creates a new local personal branch (pointing at the latest commit of `devel`):
      the personal branches should be named `add-crew-cp` (Captain), `add-crew-qm` (Quartermaster)
      and `add-crew-fm` (First-mate) respectively.
    * On your personal branch, sign the pirate code by replacing `NAME_HERE` with your name on
      the line that is matching your rank. **Important:** only sign for yourself, not for others!
    * Make a commit with a **meaningful commit message**.
    * In addition, the **First-mate** should make some changes to article I of the pirate code in
      order to get an equal share of the treasure! (this change should be added as a new commit to
      the first-mate's personal branch, `add-crew-fm`).
5. In turn, each pirate can now add their commit to the `devel` branch by:
    * Pulling new changes on `devel` from the remote (the first person to add their commit does not
      need to do this in principle).
    * Rebasing their personal branch on `devel`.
    * Merging their personal branch into `devel` - this should now be a **fast-forward** merge.
    * Pushing their changes on `devel` back to the remote.
    * Inform other team members that they can now pull the changes on `devel` (so that everyone
      stays in sync).
6. When everyone has added their changes to `devel`, each pirate should update their local `devel`
   branch to the latest version.
7. At this point, the `pirate_code.md` file on the `devel` branch should now contain the names of
   the entire team.  
   To verify that things were done properly, run `./verify_contract.sh` in your terminal: the names
   and ranks of all crew-members should be proudly displayed.
8. If the step above is working as expected for the entire team, it's time to make a **release** to
   the `main` branch:
    * The **Quartermaster** should merge `devel` into `main`.
    * The other members of the crew should then update their `main` branch.
    * The **First-mate** should add a **tag** named `contract-signed` to label this new "release"
      and push it to the remote.
    * The other members of the crew should then fetch the tag from the remote.
9. Each pirate can now delete their personal branch (`add-crew-cp`, `add-crew-qm`, `add-crew-fm`).
10. That's it, the paperwork is done! Let's get on that ship and sail!

<br>

### D) Treasure hunt part two: gear-up!
Wait... did I just say "get on that ship"? Well, you see, the problem with that is... there is no
ship... yet! And we will need a flag too! And rhum of course, lots of rhum for this long journey.  
The second part of this quest is therefore to find those 3 items: **a ship**, **a flag** and
(lots of) **rhum**.

You are 3 in the team, so split the task: the Captain will search for the ship, the Quartermaster
for the flag and the First-mate for rhum.  
This is how you should proceed:

1. As you can see, the `treasure_hunt.git` repo has several branches that correspond to different
   locations: `Port-royal`, `Oyster-bay` and `Blackbeard-castle`.
   Visit these different locations and ask local pirates for a hint by executing
   `./ask_a_pirate.sh` in your shell/terminal. When executed, these files will give you hints of
   where you can find the items you are looking for (**hint:** look at the history of the branch
   to find the commit where an item is hidden).  
   :fire:
   **Important**, please note the following:
     * Each team member should search for 1 item (as specified above).
     * Each item to find corresponds to a file: `ship.ascii`, `flag.ascii` and `rhum.ascii`.
     * To visit a location, switch/check-out the corresponding branch or commit.
     * As you have done earlier, each pirate should create a personal branch to work on:
       `add-supplies-cp` (Captain), `add-supplies-qm` (Quartermaster) and `add-supplies-fm`
       (First-mate).
2. Once you have located the commit with the item you are looking for, you should add it to your
   personal branch by **cherry-picking** the commit onto your personal branch
   (`add-supplies-cp`/`add-supplies-qm`/`add-supplies-fm`).
3. In turn, each pirate can now add their commit to the `devel` branch using the same procedure as
   in the first part of the quest - see **point 5** of "sign the pirate code" if you need a
   refresher.
4. When everyone has added their changes to `devel`, each pirate should update their local `devel`
   branch to the latest version. The `devel` branch should have 3 new commits (one made by each
   team member), and for every pirate the 3 files `ship.ascii`, `flag.ascii` and `rhum.ascii`
   should be present in the `supplies/` directory.
5. To verify whether things were done properly, run `./verify_supplies.sh` in your terminal. If
   your ship is loaded with the correct supplies, a success message will be displayed.
6. If everyone got a success message when running `./verify_supplies.sh`, then it's time to make a
   **release** to the `main` branch:
   * The **Captain** should merge `devel` into `main`.
   * The other members of the crew should then update their `main` branch.
   * The **Quartermaster** should add a **tag** named `read-to-sail` to label this new "release"
     and push it to the remote.
   * The other members of the crew should then fetch the tag from the remote.
7. Each pirate can now delete their personal branch (`add-supplies-cp`, `add-supplies-qm`,
   `add-supplies-fm`).

<br>

### E) Treasure hunt part three: find the treasure
The ship is loaded and the flag is flying, you're ready for the final step:
finding **the treasure!**

#### Visit Skull-rock-island to find the treasure
Start by setting sail on `Skull-rock-island` (i.e. switch to the branch). The island was home to
**John Longsilver** - a fearsome pirate with a healthy appetite for treasures. Unfortunately,
pirate lives do sometimes take a turn for the shorter, and so did Longsilver's.

While Longsilver is no more, his **trusty parrot** - a talkative bird if there ever was one! - is
still very much alive. Maybe that bird has some recollection of where Longsilver has been hiding
his treasure? To hear what the parrot has to say, run `./listen_to_parrot.sh` in your shell.

As you can see for yourself, the parrot isn't being helpful... but we can change this, with a
little... rebasing!  
Proceed as follows:
 1. **Switch** to the branch `work-in-progress`.
 2. On that branch, **merge the 2 commits** `make the parrot more cooperative` and
    `update the parrot's talking` together using **interactive rebase**.
 3. **Rebase** the `work-in-progress` branch on the `Skull-rock-island` branch. If any conflict
    arises during the rebase, keep the version of the file(s) that is(are) coming from the
    `work-in-progress` branch.
 4. After the rebase is completed, run `./listen_to_parrot.sh` again: the bird should now be more
    cooperative and give you some hints on where to look for **the keys** to unlock the
    **treasure chest**.

You should now also see that you have a new directory named `treasure_chest` at the root of your
Git repo. To bring the treasure chest aboard your ship, the **First-mate** should do the following:
 1. Merge the `work-in-progress` branch into the `Skull-rock-island` branch (at this point this
    should be a fast-forward merge).
 2. Merge the `Skull-rock-island` branch into the `main` branch. This will add the treasure chest
    to the `main` branch - along with your new friend the parrot!
 3. Push the changes to the `main` and `Skull-rock-island` branches to the remote.
 4. The **Captain** and **Quartermaster** can now update their local repos with the changes to
    `main` and `Skull-rock-island` that were pushed by the **First-mate**.
 5. At this point everyone should be synchronized, be on their `main` branch, and have the treasure
    onboard.
 6. Congratulations, you're almost there! Let's celebrate this with a new **release**: the
    **Captain** should add a tag named `treasure-found` and push it to the remote. Everyone else
    can then fetch it.

Alright, let's see what John Longsilver has been hiding in his treasure. To open it, execute
`./open_chest.sh` and see what happens...  
*Shiver me timbers!* We are missing **the keys** to open the lock!

:pushpin:
**Note:** If you are running out of time, you can finish this last part of the exercise on your
own (without collaboration). You simply have to find the 3 keys for the treasure on your own.

<br>

#### Opening the treasure
We are now looking for the **3 keys to open the 3 locks of the chest**. To find where they are,
listen to the parrot's hints (run `./listen_to_parrot.sh`) and then go back to `Port-royal`,
`Blackbeard-castle` and `Oyster-bay` to find the keys.

:fire:
**Hints - please read carefully:**
* Unlike in the previous quest, the locations that the parrot gives in its hints correspond to
  **directories** on disk - *not* to commits!
* The **key files** are files that have the name of the object that contains them or is near them.
  E.g. a file named `candle` would be the "key" file if you think the key is near a candle.
* Each key file must be placed in the correct lock directory, and renamed to the correct name:
  * **Key 1** must be placed into into `treasure_chest/lock_1` and renamed `key_1`.
  * **Key 2** must be placed into into `treasure_chest/lock_2` and renamed `key_2`.
  * **Key 3** must be placed into into `treasure_chest/lock_3` and renamed `key_3`.
* Once you have located the correct file (based on the hints given by the parrot), **checkout** the
  file to the `main` branch, rename and move it to the correct location (see above), and make a
  new commit.
  * To **checkout a single file** from a given branch/commit, remember that you can use:
    `git checkout <commit ref> <file>`. For this exercise, run the command while located at the
    root of working tree.
  * To move, rename, or move+rename a file, you can use: `git mv <file> <new_location/new_name>`
* **Do not modify the content of the file in any way, this would destroy the key**.

As before, this is teamwork and each pirate should retrieve one of the keys. This time however,
**to save time** (and because we believe that by now you have understood the principle), you can
**add your commit directly to `main`** (*crazy!*, I know, :tada:).
Make sure to actively coordinate with your team mates so that your `main` branch is up-to-date
before you add a commit to it: you do not want any divergence in the `main` branch among the team!
If divergence occurs, you can:
* **Rebase** your local changes in `main` onto `origin/main`: `git rebase origin/main`
  or `git pull --rebase` (both will give the same result).
* **Merge** your local changes in `main` with `origin/main`: `git pull` (on older Git versions) or
  `git pull --no-rebase` (on recent Git versions).

When each pirate has added his key to `main` and everyone has updated their local Git repo copies,
you can try to run `./open_chest.sh` again. If the keys are the correct files, have been placed at
the correct location, and have been correctly renamed, the chest should open!

<br>

### For people doing this exercise as an exam :memo:
If you would like to receive credits for the course, please do the following within 3 days of the
end of the course:
1. Take a screenshot of the output of your Git repo's full history (use
   `git log --all --decorate --oneline --graph`) at the end of your quest.
2. Create a new branch named `exam-YOUR_FULL_NAME` rooted on the latest commit of `main` (replace
   `YOUR_FULL_NAME` with your actual name). Add a commit with your screenshot to this new branch
   and push it to the remote.
3. Send an email with subject "Git course exam - your name" to `robin.engler@sib.swiss` with the
   following information:
    * Your **full name**.
    * The **URL of the remote repo** that you have used for this exercise (repo must be public).
    * The **secret code** you found when opening the chest.
4. Make sure you have signed-up for the exam on the Google doc of the course.


<br>
<br>
<br>
