# Version control with Git: setting-up your environment

Please complete the following setup **before the start of the course**. In particular:
* Make sure to provide us with your GitHub user name (see point 2. below) at the
  **latest 3 days before** the start of the course.
* For online classes, you should have a computer with a **working microphone** as some exercises
  involve collaboration/communication with other participants.

<br/>

## 1. Installing Git on your computer
To install Git, please refer to the relevant section below depending on your operating system.
Ideally, a relatively recent version of Git (>= 2.23.0) should be installed, although this is not
strictly necessary.

To test whether your installation was successful, try to run `git help` and `git --version`.

* **Linux:** install Git using your distribution's official package manager.
    * Ubuntu/Debian: `apt-get install git`
    * Fedora/CentOS: `dnf install git`  

* **Windows:** download Git from [this link](https://git-scm.com/download/win) and follow the
  installation instructions.  
  When the installation is complete, open the `Git Bash` application from the Windows start menu.
  This application acts as a small Linux shell, with Git and a few other basic GNU-Linux tools
  installed (e.g. the `vim` and `nano` editors).  

* **MacOS:** Download Git from [this link](https://git-scm.com/download/mac) and follow the
  installation instructions.  


<br/>  
<br/>


## 2. Create a GitHub account
[GitHub](https://www.github.com) is a commercial hosting platform for Git repositories, currently
owned by Microsoft.  

We will use GitHub for some of the course exercises, and you will therefore need to have a (free)
account on GitHub to use during the course. If you do not already have a GitHub account, go to
[www.github.com](https://www.github.com) and sign-up for a free account.

**IMPORTANT:** once you have created your account, add your name and GitHub user name to the
"GitHub user names" table in
[this google doc document](https://docs.google.com/document/d/1EX72NInz-eA2d2GOa5aTB8D88GWb91Sk-sCNHwQYXqE).
This is needed so that we can add you to projects we will use during the exercises. Please do this
**at least 3 days before the start of the course** as it sometimes takes a bit of time before the
invitations to join a new project on GitHub are sent-out.

The "GitHub user names" table will also be used to define teams for the collaborative exercises
that we will do during the course (see the "Team" column of the table). If you are participating in
the course with colleagues, you can register in the same team.


<br/>  
<br/>


## 3. Install Git-LFS on your computer
**If you plan to attend the optional Git LFS (Large File Storage) module of the course on day 3**,
please install the Git-LFS extension on your machine.

* **Linux:** install Git LFS using your distribution's official package manager:
    * Ubuntu/Debian: `apt-get install git-lfs`
    * Fedora/CentOS: `dnf install git-lfs`

* **Windows:** Git LFS is packaged directly with [Git for Windows](https://git-scm.com/download/win),
  therefore no extra installation is needed.

* **MacOS:** Git LFS can be installed by either:
    * Downloading it from [https://github.com/git-lfs/git-lfs/releases](https://github.com/git-lfs/git-lfs/releases)
    * Installing via Homebrew: `brew install git-lfs`
    * Installing via MacPorts: `port install git-lfs`
