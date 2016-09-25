
DevOps Tools
---
**Table of Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [DevOps Tools](#devops-tools)   
   - [Introduction](#introduction)   
   - [Code Editor](#code-editor)   
      - [Exercise-1](#exercise-1)   
         - [Installing Atom IDE](#installing-atom-ide)   
      - [Exercise-2](#exercise-2)   
         - [Add git-plus plugins](#add-git-plus-plugins)   
   - [Virtual Box](#virtual-box)   
      - [Exercise-1](#exercise-1)   
         - [Installing Virtual box](#installing-virtual-box)   
      - [Exercise 2](#exercise-2)   
         - [Verify Virtual box installation](#verify-virtual-box-installation)   
   - [GIT](#git)   
      - [Exercise-1](#exercise-1)   
         - [Installing git](#installing-git)   
            - [Mac Users](#mac-users)   
            - [Windows Users](#windows-users)   
   - [Do not do the sections below. These tool are not needed for the lab.](#do-not-do-the-sections-below-these-tool-are-not-needed-for-the-lab)   
   - [Vagrant Tool](#vagrant-tool)   
      - [Exercise 1](#exercise-1)   
         - [Installing Vagrant Tool](#installing-vagrant-tool)   
      - [Exercise 2](#exercise-2)   
         - [Verify installation of vagrant tool](#verify-installation-of-vagrant-tool)   

<!-- /MDTOC -->




Introduction
---

Most of the developers develop applications on their laptop.  They have most of the application development tools installed on their laptop. We will try to setup similar environment for you on your laptop. This way you can have tools on your laptop to continue your learning after this class.  Lets install some of the tools:

## Code Editor
To write codes, you will need a good text editor that does syntax highlighting. Atom is excellent editor for editing  codes. It is opensource so there is no fee for it.  Any text editor with syntax highlight will do.

### Exercise-1
#### Installing Atom IDE

1. Download and install atom editor for your OS.
http://atom.io

### Exercise-2
#### Add git-plus plugins
1. Click on Atom
2. click on Preference.
3. click on `Install`
4. search for `git-plus`
5. Click install to install the package.

**Tips on using git-plus**
cmd + shift + H  <-- to get the command window

## Virtual Box
Virtual box is a x86 virtualization product.  Basically it lets you run virutal machines on your laptop.

### Exercise-1
#### Installing Virtual box

1. Download and install Virtualbox for your OS.
    https://www.virtualbox.org/

### Exercise 2
#### Verify Virtual box installation
1. On the terminal window type `vboxmanage -v`


## GIT
GIT is a very popular and efficient open source Version Control System. It tracks content such as files and directories.

GIT stores the file content in BLOBs (binary large objects). The folders are represented as trees. Each tree contains other trees (subfolders) and BLOBs along with a simple text file which consists of the mode, type, name and SHA (Secure Hash Algorithm) of each blob and subtree entry. During repository transfers, even if there are several files with the same content and different names, the GIT software will transfer the BLOB once and then expand it to the different files.

The history of your project is stored in a commit object. Every time you make a modification you have to commit it. The commit file keeps the author, committer, comment and any parent commits that directly precede it.

### Exercise-1
#### Installing git
##### Mac Users
1. **Xcode** is a large suite of software development tools and libraries from Apple.  This includes git package.  Lets install xcode-select, command line utility for xcode.
2. Open up a terminal window.  `Cmd+space`  and type `terminal`
3. on the terminal type  `xcode-select --install`
4. A software update popup window will appear.
5. click on `Install`.
6. Wait for it to install the Command line tools.
7. type  `git --help`
8. you see all the options for git command.

##### Windows Users
1. Download  and install `mysGit` package. Use all the default setting for installation. https://git-for-windows.github.io/
2. Use all the default during installation.
2. You should see Git Bash icon on your desktop.  

## Do not do the sections below. These tool are not needed for the lab.
## Vagrant Tool
### Exercise 1
#### Installing Vagrant Tool
1. Download and install vagrant from here
    https://www.vagrantup.com/downloads.html

For more information see vagrant documentation
https://www.vagrantup.com/docs/installation/

### Exercise 2
#### Verify installation of vagrant tool
Verify that it is installed correctly

1. Open a terminal window
2. Type `vagrant -v` and `vagrant -h`
3. make sure you see help options
