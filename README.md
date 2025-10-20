# Version control with Git

**Table of Content:**

[[_TOC_]]

<br>

## Course Description

### Overview

**Git** is an open source version control system for tracking changes
in code and other types of text documents. First released in 2005, Git has
become the de-facto standard for version control, and is extensively used in
both open source and commercial software development. The usage of Git is not
limited to software development, but can also be used to keep track of data
analysis scripts, pipelines, and more generally any text-based document.

This 2-day course gives a **comprehensive overview of Git** and its most useful
commands, as well as an introduction to collaborative workflows using GitHub.

The course starts with the basics of Git - it is therefore suitable for people
with no or little knowledge of Git - and will take the participants all the
way into more advanced territory such as editing the history of a Git
repository. Emphasis is also put on collaborative workflows, which are actively
practiced in some of the exercises.

Please note that this course focuses exclusively on using Git via
**command line**. This knowledge is then easily transferrable to any GUI
environment (e.g. VS Code, RStudio, etc.).

Specifically, the course covers the following topics:

#### Day 1: first steps with Git

* Brief introduction to version control systems and Git.
* Git basics: creating a git repository and making commits.
* Git concepts: commits, the HEAD pointer and the Git index.
* Git branches: introduction to branched workflows and collaborative
  workflow examples.
* Branch management: merge, rebase and cherry-pick.
* Retrieving data from the Git database: git checkout.
* Working with remotes: collaborating with Git and GitHub.
* GitHub: a brief overview.

#### Day 2: Git advanced topics

* Rewriting history: interactive rebase, git reset and commit amending.
* The detached HEAD state explained.
* The Git stash: Git’s “cut and paste” functionality.
* Git tags: label important commits.
* GitHub: creating a new project, adding new users and collaborating with them.

#### Optional modules

This material is not formally presented during the course. It covers topics
that can be useful in certain scenarios:

* **GitHub Actions** and **GitLab CI/CD**: writing automated pipelines.
* **Git submodules**: embed a Git repository as a subdirectory of another Git
  repo.
* **Git LFS**: versioning large files with Git.

### Audience

This course is aimed at people who are interested in using a version control
system for collaborative work, or simply to keep track of modifications in
their scripts, files or code base.  
This includes people working on code development, but also scientists
interested in improving the reproducibility of their data analyses by keeping
track of their scripts using version control.

### ECTS credits

Participants who wish to receive ECTS credits for the course can take an
optional exam at the end of day 2 of the course. The SIB will recommend
successful participants to receive 0.5 ECTS credits from their home
institutions.

<br>

## Course prerequisites

### Knowledge/competencies

The course is focused on using Git in command line mode (no graphical user
interface). It is therefore
**highly recommended to have some basic knowledge of the UNIX command line**:
e.g. how to change directory or how to edit a file in a command line editor
such as vim/nano.

If you are not familiar with these UNIX fundamentals, we strongly recommend
that you either follow an introductory UNIX course (e.g. the SIB "First Steps
with UNIX" course - see
[upcoming training courses](https://www.sib.swiss/training/upcoming-training-courses),
or take the an online tutorial such as
[this one](https://edu.sib.swiss/pluginfile.php/2878/mod_resource/content/4/couselab-html/content.html)
before the start of the course.

### Technical

* A Wi-Fi enabled laptop with a recent version of Git installed. Git is
  available on all major platforms and can be
  [downloaded here](https://git-scm.com/download).
* For online classes, you should also have a working microphone as some
  exercises involve collaboration/communication with other participants.

### Setting-up your environment

* Please complete the [environment setup instructions](Setting_up_your_environment.md)
  **before** the start of the course.  
* In particular, make sure to communicate us your GitHub user name
  **at least 2 days before** the start of the course via the google doc -
  see the [environment setup instructions](Setting_up_your_environment.md).

<br>

## Course links, exercises and slides

:fire:
**Please note:** the definitive course material (slides and exercises) will be
made available on the first day of the course. Until then files do not
necessarily correspond to the final version.

Useful links during the course:

* [Google doc](https://docs.google.com/document/d/1EX72NInz-eA2d2GOa5aTB8D88GWb91Sk-sCNHwQYXqE):
  for asking questions, finding your team (exercise 4), and see answers to
  previously asked questions.

### Course slides

* [First steps with Git](slides_git_first_steps.pdf)
* [Git advanced topics](slides_git_advanced_topics.pdf)
* Git commands summary:
  [PDF document summarizing all commands seen during the course](git_command_summary.pdf)
* [Git optional modules](slides_git_optional_modules.pdf)

### Exercises

**Exercise instructions:** these instructions are best viewed in a web browser.
Simply click on the link below to display them.

* Day 1: [exercises first steps](exercises_first_steps.md)
* Day 2: [exercises advanced topics](exercises_advanced_topics.md)
* Optional modules:
  * [optional exercise CI/CD](exercises_optional_cicd.md)
  * [optional exercise submodules](exercises_optional_submodules.md)
  * [optional exercise Git LFS](exercises_optional_LFS.md)

**Exercise material:** please download the following `.zip` files and
  decompress them on your local machine:

* Day 1: [`exercises_first_steps.zip`](exercises_first_steps.zip)
* Day 2: [`exercises_advanced.zip`](exercises_advanced.zip)
* Optional modules:
  * [`exercises_optional_modules.zip`](exercises_optional_modules.zip)

**Exercise solutions**:

* Day 1: [solutions first steps](solutions_first_steps.md)
* Day 2: [solutions advanced topics](solutions_advanced_topics.md)
* Optional modules:
  * Solutions CI/CD - solutions are integrated in exercise instructions.
  * [Solutions submodules](solutions_optional_submodules.md)
  * [Solutions Git LFS](solutions_optional_LFS.md)

<br>

## Course schedule

Please note that this schedule is approximate.

* **Day 1 and 2:** 9h00 - 17h30

### First steps with Git [day 1]

* 09h00: Welcome and intro to Git
* 09h10: Git basics
* 10h00: Exercise 1
* 10h30: BREAK - 15 min
* 10h45: Exercise 1 - corrections
* 11h00: Git concepts: commits, the HEAD pointer and the git index
* 11h30: Git branches, git merge
* 12h15: LUNCH BREAK - 1h
* 13h15: Exercise 2
* 13h45: Exercise 2 - corrections
* 14h00: Git rebase, git cherry-pick
* 14h45: Git commit --amend, git checkout
* 15h00: BREAK - 15 min
* 15h15: Exercise 3
* 15h45: Exercise 3 - corrections
* 16h00: Working with remotes, GitHub
* 16h15: Exercise 4
* 17h30: End of day 1

### Git advanced topics [day 2]

* 09h00: Intro + review/reminder
* 09h20: Interactive rebase
* 10h00: Exercise 1
* 10h40: BREAK - 15 min
* 10h55: Exercise 1 - Corrections
* 11h15: Git reset
* 11h35: Exercise 2
* 12h15: LUNCH BREAK - 1h
* 13h15: Exercise 2 - corrections
* 13h30: Detached HEAD mode, git stash, tags.
* 14h00: Exercise 3
* 14h30: Exercise 3 - corrections
* 14h45: BREAK - 15 min
* 15h00: Exercise 4
* 17h30: End of day 2

### Breaks

* 1 hour break for lunch.
* 15 minutes breaks in each half-day sessions.

<br>

## Useful references and tutorials

* [Pro Git - free Git book](https://git-scm.com/book/en/v2)
* [Atlassian Git tutorials](https://www.atlassian.com/git/tutorials)

<br>

## Acknowledgments and funding

### Funding

* The development of this course has been funded by the Swiss State Secretariat
  for Education, Research and Innovation
  ([SERI](https://www.sbfi.admin.ch/sbfi/en/home.html)) trough the
  [SIB Swiss Institute of Bioinformatics](https://www.sib.swiss).
* Other funding sources include:
  * European Union's Horizon Europe Framework Program (grant number 101080997).
