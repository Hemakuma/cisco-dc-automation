Gitlab
---
**Table of Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [Gitlab](#gitlab)   
   - [Introduction](#introduction)   
   - [Creating `ansible` repository on gitlab.](#creating-ansible-repository-on-gitlab)   
      - [Exercise-1](#exercise-1)   
         - [Configure gitlab](#configure-gitlab)   
      - [Exercise-2](#exercise-2)   
         - [Putting `ansible` directory under git control.](#putting-ansible-directory-under-git-control)   
      - [Exercise-3](#exercise-3)   
         - [Verify that file got uploaded to gitlab](#verify-that-file-got-uploaded-to-gitlab)   
      - [Exercise-4](#exercise-4)   
         - [Scripting the git push](#scripting-the-git-push)   
      - [Exercise-5](#exercise-5)   
         - [Modifying the README file.](#modifying-the-readme-file)   
      - [Exercise-6](#exercise-6)   
         - [Push the Updated file to gitlab using script](#push-the-updated-file-to-gitlab-using-script)   
      - [Exercise-7](#exercise-7)   
         - [Verify the file has changed on gitlab.](#verify-the-file-has-changed-on-gitlab)   
      - [Exercise-8](#exercise-8)   
         - [gitpush.sh script for nxtoolkit repository.](#gitpushsh-script-for-nxtoolkit-repository)   
   - [Creating `nxtoolkit` repository on gitlab.](#creating-nxtoolkit-repository-on-gitlab)   
      - [Exercise-1](#exercise-1)   
         - [Create nxtoolkit project](#create-nxtoolkit-project)   
      - [Exercise-2](#exercise-2)   
         - [Putting nxtoolkit directory under git Repository](#putting-nxtoolkit-directory-under-git-repository)   
   - [Creating gitlab users.](#creating-gitlab-users)   
      - [Exercise 1](#exercise-1)   
      - [Create another gitlab account (`developer`)](#create-another-gitlab-account-developer)   
   - [Working with Git Branch](#working-with-git-branch)   
      - [Exercise 1](#exercise-1)   
         - [Creating a git branch](#creating-a-git-branch)   
      - [Exercise 2](#exercise-2)   
         - [Editing README.md file](#editing-readmemd-file)   
      - [Exercise 3](#exercise-3)   
         - [Push the branch to remote repository (gitlab)](#push-the-branch-to-remote-repository-gitlab)   
      - [Exercise 4](#exercise-4)   
         - [Create `Merge Request` on gitlab](#create-merge-request-on-gitlab)   
      - [Exercise 5](#exercise-5)   
         - [Merge the branch with master branch](#merge-the-branch-with-master-branch)   
      - [Exercise 6](#exercise-6)   
      - [Exercise 7](#exercise-7)   
         - [Remove the branch](#remove-the-branch)   
      - [Summary Steps for creating a branch](#summary-steps-for-creating-a-branch)   
      - [Some useful git command](#some-useful-git-command)   

<!-- /MDTOC -->


## Introduction
GIT is a very popular and efficient open source Version Control System. It tracks content such as files and directories for changes. Files transition between 3 states, modified, staged and committed file.  Repository are local but if you want to share your codes with your team, you can also push it to remote/central repository.

Note, Git doesn’t store data as a series of changesets or deltas, but instead as a series of snapshots.

  ![GitHub](/images/git-intro-1.png)

**Make sure you have git installed on your system.  See devops tools section.**

For this lab, we have installed our own gitlab server on the docker container. We will use this server for all our exercises. At the end, we will push our configuration files to github.

## Creating `ansible` repository on gitlab.
### Exercise-1
#### Configure gitlab

1. Open up chrome browser and point it to:
2. `http://<ip of the docker node>`
3. change the admin password to `cisco123`

  ![gitlab](/images/gitlab-550.png)

4. create a new user

  ![gitlab](/images/gitlab-100.png)

	```
	<yourid>/cisco123

	```

4. Click on create `New project` and name it `ansible`

  ![gitlab](/images/gitlab-551.png)

  ![gitlab](/images/gitlab-300.png)

5. Click on `Create Project`.
6. Do not close this window. We will use the information on this page for the next exercise.

### Exercise-2
#### Putting `ansible` directory under git control.
1. Switch to the terminal window (docker quick start)
2. `cd training`
3. `cd ansible`
4. Follow the instructions from the gitlab site to create a new repository.

  ![gitlab](/images/gitlab20.png)

5. You going to put your existing folder under git control.
5. Change the git commit command to use -m flag.  `git commit -m 'your message'`
5. Modify the first line as follows. Add your username and password to the url ..hemakuma:cisco123@
6. Do not copy and paste everything at once.  Do line by line.

  `git remote add origin http://hemakuma:cisco123@192.168.99.100/hemakuma/ansible.git`

  ![gitlab](/images/gitlab-505.png)
7. type ` git config -l`  and verify all the information you typed in is in the configuration file.

### Exercise-3
#### Verify that file got uploaded to gitlab
1. Switch back to you browser
2. Refresh it
3. Click on the `file tab`

  ![gitlab](/images/gitlab-506.png)

4. Make sure you see "README.md".

  ![gitlab](/images/gitlab-303.png)

### Exercise-4
#### Automating git push (gitpush.sh)
For the lab environment, we want to quickly push the changes to the remote repo on the gitlab. In this exercise we going to create a script to automate this process.

1. Switch to `ATOM` Editor
2. Go to the `ansible` folder.
3. Right click and select `New File`
4. name it `gitpush.sh`
5. add the following content to the file

    ```
    #!/bin/bash
    echo 'updating git'
    git add .
    git commit -m 'updated'
    git push
    ```
6. Save and close the file `cmd + S`

### Exercise-5
#### Modifying the README file.
1. Open up the `README.md` file from the `ansible` folder using `ATOM`
2. Add some contents to it eg `this is my second gitlab edit`
3. Save it `CMD + S`.

### Exercise-6
#### Push the Updated file to gitlab using script
1. Switch back to the terminal window.
2. you should be in the `ansible` directory  (`training/ansible`)
3. make the gitpush.sh script executable.
4. `chmod +x gitpush.sh`
6. type
7. `./gitpush.sh`

### Exercise-7
#### Verify the file has changed on gitlab.
1. Switch to browser
2. Take a look at the README.md
3. you should see the new contents.

  ![gitlab](/images/gitlab-302.png)

4. You can compare what changed by clicking on the `README.md` and then on the `History` tab. Select the `update` you want to compare.

  ![gitlab](/images/gitlab-304.png)


### Exercise-8
#### gitpush.sh script for nxtoolkit repository.
1. Using `ATOM` copy the `gitpush.sh` file from `ansible` folder.  
  1. Right click on the `gitpush.sh` and select copy.
  2. Paste it in the `nxtoolkit` folder. Right click on the `nxtoolkit` folder and select paste.
3. Switch back to the terminal window.
  2. you should be in the `nxtoolkit` directory  (`cd ~/training/nxtoolkit`)
  3. make the gitpush.sh script executable.
  4. `chmod +x gitpush.sh`
  7. We will use this script in the next exercise

----

## Creating `nxtoolkit` repository on gitlab.
### Exercise-1
#### Create nxtoolkit project
1. Switch back to your `Chrome browser`
2. login to the gitlab using your own account.
2. Create a new `project`, name it `nxtoolkit`.  See screen capture below.

    ![gitlab](/images/gitlab-507.png)

### Exercise-2
#### Putting nxtoolkit directory under git Repository
1. Switch to the terminal window
2. `pwd`  <-- make sure you are in the nxtoolkit directory
  1. ``cd ~/training/nxtoolkit``
  2. `touch README.md`
3. Follow the instructions from the gitlab site to put your `nxtoolkit` directory under git version control.
4. Do not copy and paste everything at once.  Do line by line.
5. For git commit command, use -m flag.  `git commit -m 'your message'`
5. Modify the first line as follows. Add your username and password to the url ..hemakuma:cisco123@

  `git remote add origin http://hemakuma:cisco123@192.168.99.100/hemakuma/nxtoolkit.git `

  ![gitlab](/images/gitlab-508.png)
6. Switch to chrome browser and verify that files have been uploaded.
  7. Refresh the browser page
  8. Click on `Files`

---
## Creating gitlab users.
### Exercise 1
### Create another gitlab account (`developer`)
You will use the 2nd account to review the changes. You will use your first account to request the merge and using 2nd account to review , approve and merge the changes to the trunk.

1. sign out from gitlab.  Sign out button is at the right top corner.
2. Fill in the form for `New User` account

  ![gitlab](/images/gitlab-400.png)


###Exercise 2
####Giving access to your projects to this user.
1. Sign out as developer account and log back in using your regular account
2. Give access to both `ansible` and `nxtoolkit` to the `developer` user.
3. Select the `ansible` project and the click on `setting`.

  ![gitlab](/images/gitlab-510.png)
4. Click on `Members`

  ![gitlab](/images/gitlab-511.png)

5. Repeat the process for the `nxtoolkit` repository

  ![gitlab](/images/gitlab-512.png)

----

##Adding project to ATOM
With git-plus plugin, you can manage git updates directly from ATOM.  In order to do this, you need to open each git repository as ATOM project.

###Exercise 1
####Adding git Projects in ATOM
1. Switch to `ATOM` Editor
2. Close all the open files.
2. Right click on the `training folder`
3. Click on `Remove Project Folder`

  ![gitlab](/images/gitlab-509.png)

4. Go to `File`  and `Add project Folder`
5. Navigate to `ansible`  folder under `training` and open it.
6. Repeat the same process to add `nxtoolkit` folder as a `project folder` in ATOM.
7. click on any file inside nxtoolkit folder and take a look at the bottom right corner. It should tell you will git branch.
8. Now u can run git add / commit / push directly from ATOM.  Go to `Package` menu and select `git-plus`.
9. For windows users, you might have to add the `git` PATH to your Environment variable.

  ```
  right-click "My Computer",
  select "Properties",
  open "Advanced",
  click "Environment Variables",
  highlight the "Path" variable,
  click "Edit"
  Add the path to git.  eg C:\Program Files\Git\bin
  ```


----

## Working with Git Branch
A branch in Git is simply a lightweight movable pointer to one of these commits. The default branch name in Git is master. As you initially make commits, you're given a master branch that points to the last commit you made. Every time you commit, it moves forward automatically.  Creating a new branch just adds a new point to the commit.

![branch](/images/branch-20.png)

### Exercise 1
#### Creating a git branch
1. Switch to terminal window
2. Navigate to `ansible` folder. `cd ~/training/ansible`
4. Type `git branch`  to see what branches you currently have and where the `HEAD` is pointing to.  You should only see the `master` branch and the `HEAD` should be pointing to it.
5. Create a new branch called `readmeupdate`
5. `git branch readmeupdate`  to create a new branch.
6. `git branch` ; notice how you now have 2 branches. HEAD is still pointing to the master branch.
6. `git checkout readmeupdate` to switch to new branch.
7. `git branch`  now the HEAD should be pointing to the new branch.

  ![branch](/images/branch-1.png)

8. Now lets make some changes to the `README.md` file.

### Exercise 2
#### Editing README.md file
1. Switch to `ATOM` Editor.
2. Go to `ansible` folder and open up `README.md` file.
3. Look at the right hand bottom corner to see which branch you are on. (this only works if you have git-plus plugin installed)
3. Edit the file. Put some text in it eg . `This is my 3rd edit using branch feature`.
4. `CMD+S` to save the file

  ![branch](/images/branch-1.png)

### Exercise 3
#### Push the branch to remote repository (gitlab)
1. Switch back to terminal window.
2. you should be in the `ansible` directory
3. push your changes to gitlab.  you want to push your branch to the gitlab.
4. you should still be in your `readmeupdate` branch.  Verify `git branch`
5. `git add .`  to add the changes
6. `git commit -m  'updated readme file' ` to commit the file
7. `git push -u origin readmeupdate` push the branch to gitlab


### Exercise 4
#### Create `Merge Request` on gitlab
1. Switch to the chrome browser.
2. Login to gitlab using your regular account.
3. Go to your `ansible` repository.

  ![branch](/images/gitlab-513.png)

4. you should see `Create Merge Request` button. Click on it
5. fill in the form as below and click on the `submit` button at the bottom.

  ![branch](/images/gitlab-514.png)


### Exercise 5
#### Merge the branch with master branch
1. Log out of gitlab accounts
2. log back in using `developer`  account.
3. Select `ansible` repository.
4. Click on the menu button

  ![branch](/images/gitlab-515.png)

5. Click on th `Merge Request`
6. Then select the `readmeupdate` merge request.
7. Click on the `Changes`  tab.  Take a look at all the changes that happen to the file line by line.
8. Add your comment to line 2.
8. Click on the `Discussion` tab.  
9. Write your review in the section below.
7. Then on the top, click `Accept Merge Request`

  ![branch](/images/gitlab-516.png)

### Exercise 6
####Verify the merge
1. Switch back to your terminal window
  2. make sure you are in the master branch
  3. `git branch`
  3. `git checkout master`
4. Switch to `ATOM` Editor.
  5. Take a look at the `README.md` file.  Notice that you do not see the changes that you made.
6. Switch back to terminal window.
  7. pull down the changes from your central repository
  7. `git pull`
8. Switch back to `ATOM`
  9. view the `README.md` file.
  10. Do you see the changes that got merged?

**This is how you can do simple peer reviews on your configuration files.**

### Exercise 7
#### Remove the branch
1. switch to the terminal window.
2. `git branch`
2. `git branch -d readmeupdate`
3. `git branch`
3. this should remove the branch.  We don't need it as we have merged our changes to the master (trunk)


### Summary Steps for creating a branch

1. Clone project
2. git clone git@example.com:project-name.git
3. Create branch with your feature
4. git checkout -b $feature_name
5. Write code. Commit changes
6. git commit -am "My feature is ready"
7. Push your branch to GitLab
8. git push origin $feature_name
9. Review your code on commits page
10. Create a merge request
11. Your team lead will review the code & merge it to the main branch

### Some useful git command

```
git branch
git log
git config -l
git status
git show

```
