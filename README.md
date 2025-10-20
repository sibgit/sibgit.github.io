# Version control with Git

**Table of Content:**

[[_TOC_]]

<br>

**Please note:**
* The course material such as slides and exercises will be made available on the first day of
  the course. Until then files might either not be available or not correspond to the final
  version.
* Please complete the [environment setup](Setting_up_your_environment.md) **before** the start
  of the course.

<br>

## Course links and slides
Useful links during the course:
* [Google doc](https://docs.google.com/document/d/1EX72NInz-eA2d2GOa5aTB8D88GWb91Sk-sCNHwQYXqE):
  for asking questions, finding your team (exercise 4), and see answers to previously asked
  questions.
* [Zoom link](https://us02web.zoom.us/j/84868422273?pwd=YWdkT3NNVEltZCtPVy90TURSYzY1QT09): for
  online courses.
* [Feedback form](https://forms.office.com/pages/responsepage.aspx?id=KewaGuOxtEGL2iiKlpme_4W5j_y6UXxNg-bVSoI3oSFUMU81M0tEOUZBVEpWQlRIT0FXUFNZNVo3Sy4u): for giving your feedback at the
   end of the course.

**Course slides:**
* [First steps with Git](slides_git_first_steps.pdf)
* [Git advanced topics](slides_git_advanced_topics.pdf)
* [Git optional modules](slides_git_optional_modules.pdf)

**Git commands summary:**
* [PDF document summarizing all commands seen during the course](git_command_summary.pdf)

<br>


## Exercises
**Exercise instructions:**
* [Exercises day 1 - first steps](exercises_first_steps.md)
* [Exercises day 2 - advanced topics](exercises_advanced_topics.md)
* [Exercises day 3 - optional modules](exercises_optional_modules.md)

**Exercise material:** please download the following `.zip` files and decompress them on your
local machine:
* [Exercise material day 1](exercises_first_steps.zip)
* [Exercise material day 2 and 3](exercises_advanced.zip)

**Exercise solutions** will be made available at the end of each day:
* [Solutions day 1 - first steps](solutions_first_steps.md)
* [Solutions day 2 - advanced topics](solutions_advanced_topics.md)
* [Solutions day 3 - optional modules](solutions_optional_modules.md)

<br>


## Setting-up your environment
Please complete the [environment setup instructions](Setting_up_your_environment.md) **before** the
start of the course.  
In particular, make sure to send us your GitHub user name **at least 2 days before** the start of
the course.

<br>


## Course schedule
Please note that this schedule is approximate.
* **Day 1 and 2:** 9h00 - 17h30
* **Day 3:** 9h00 - 12h15

**First steps with Git [day 1]**
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
* 16h30: Exercise 4
* 17h30: End of day 1

**Advanced topics [day 2]**
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

**Advanced topics: optional modules [day 3, half day]**
* 09h00: Git submodules
* 09h30: Exercise 5
* 10h00: Exercise 5 - corrections
* 10h15: BREAK - 15 min
* 10h30: Git LFS
* 11h00: Exercise 6A
* 11h20: Exercise 6A - corrections
* 11h30: Exercise 6B
* 12h00: Exercise 6B - corrections
* 12h15: End of course

**Breaks**
* 1 hour break for lunch.
* 15 minutes breaks in each half-day sessions.

<br>


## Useful references and tutorials
* [Pro Git - free Git book](https://git-scm.com/book/en/v2)
* [Atlassian Git tutorial - become a git guru](https://www.atlassian.com/git/tutorials)

<br>


## Course Description

### Overview
Git is an open source, distributed, version control system for tracking changes in source code and
other types of text documents. Created by Linus Torvald and first released in 2005, Git has become
the de-facto standard for project source code management, and is extensively used both in open
source and commercial software development. The usage of Git is not limited to code development,
but can also be used to keep track of data analysis scripts and pipelines.

This 2.5 day course gives a very comprehensive introduction to Git and its most useful commands,
as well as an introduction to collaborative workflows and to using GitHub. The last half-day of the
course is optional.

The course covers the following topics:

**Day 1: first steps with Git**
* Introduction to version control systems and Git.
* Git basics: your first commit.
* Git concepts: commits, the HEAD pointer and the Git index.
* Git branches: introduction to branched workflows and collaborative workflow examples.
* Branch management: merge, rebase and cherry-pick.
* Retrieving data from the Git database: git checkout.
* Working with remotes: collaborating with Git.
* GitHub: a brief overview.

**Day 2: git advanced topics**
* Rewriting history: interactive rebase, git reset and commit amending.
* The detached HEAD state explained.
* The Git stash: Git’s “cut and paste” functionality.
* Git tags: label important commits.
* GitHub: creating a new project, adding new users and collaborating wit them.

**Day 3 (half-day): optional modules**  
The last half-day of the course is optional and covers two Git extensions that can be useful in
certain scenarios:
* Git LFS: versioning large files with Git.
* Git submodules: embed a Git repository as a subdirectory of another Git repo.


### Audience
This course is aimed at people with no or little knowledge of Git, who are interested in using
a version control system for collaborative work, or simply to keep track of modifications in their
scripts/code base/files.
This includes people working on code development, but also scientists interested in improving the
reproducibility of their data analyses by keeping track of their scripts using version control.


### ECTS credits
Participants who wish to receive ECTS credits for the course can take an optional exam at the end
of day 2 of the course. The SIB will recommend successful participants to receive 0.5 ECTS
credits from their home institutions.


### Prerequisites
**Knowledge/competencies**  
The course is focused on using Git in command line mode (no graphical user interface). It is
therefore necessary to have some basic knowledge of the UNIX command line: e.g. how to change
directory or how to edit a file in a command line editor such as vim/nano.

If you are not familiar with these UNIX fundamentals, we strongly recommend that you either follow
an introductory UNIX course (e.g. the SIB "First Steps with UNIX" course - see
[upcoming training courses](https://www.sib.swiss/training/upcoming-training-courses), or take
the an online tutorial such as [this one](https://edu.sib.swiss/pluginfile.php/2878/mod_resource/content/4/couselab-html/content.html)
before the start of the course.

**Technical**
* A Wi-Fi enabled laptop with a recent version of Git installed. Git is available on all major
  platforms and can be [downloaded here](https://git-scm.com/download).
* For online classes, you should also have a working microphone as some exercises involve
  collaboration/communication with other participants.
