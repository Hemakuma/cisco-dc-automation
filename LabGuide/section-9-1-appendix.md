## Saving your configuration to git hub

### Exercise 8
#### Saving your Repository to github.
1. Create a account on github.com
2. create a repository called `training`.
3. we will save all configuration to github. This way even if you delete it from your laptop, it will still be available online.
4. now push your training repository to git hub.
5. switch back to terminal window
6. go to `training` folder.
7. type the following.  change the url to match your repository and username.
```
git init
git remote add origin https://hemakuma@github.com/Hemakuma/training.git
git add .
git commit -m `saving training material on github`
git push -u origin master
```
8. Now for the lab, we will be using our locally installed gitlab server.  Once we have finished with the lab, we will push everything to github.  This will be your reference for later on.




##Manual way of creating containers

###Ansible

####Build Ansible Dockerfile
1. Switch to `ATOM` editor
2. Right click on the `training` folder and select `New Folder`.  Name it `ansible`.
3. Now Right click on the `ansible` folder and select `New File`,  and name this file as `Dockerfile`
4. Copy and paste the content from here to this new file.  
https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/Dockerfile_ansible  
5. Build the image. From the terminal, type  
    `docker build -t ansible --tag hemakuma/ansible .`
6. Verify that ansible image is created.  
    1. `docker images`

###Exercise-2
####Creating Ansible Container using Docker

1. Spin up a nxos ansible container
    1. Mac Users  
    `docker run --name ansible -h ansible -it --restart=always -v ~/training/ansible:/nxos-ansible/myscripts --workdir /nxos-ansible/myscripts hemakuma/ansible`
    2. Windows Users  
    `winpty docker run --name ansible -h ansible -it --restart=always -v ~/training/ansible:/nxos-ansible/myscripts --workdir /nxos-ansible/myscripts hemakuma/ansible`

    This will start the container and log you in. From inside the container.

2. exit the container.  We will use this container later in the ansible exercises.
3. Go to your Atom editor and you should all these files under ansible folder.



##NXtoolkit
### Exercise-1
####Creating nxtoolkit image
1. Go to your `ATOM` Editor
2. Right click on the `training` folder and select `New Folder`. Name it `nxtoolkit`
3. Now Right click on the `nxtoolkit` folder and select `New File`
4. Name this file `Dockerfile`
4. Copy and paste the content from here to this new file.  
https://github.com/Hemakuma/training/blob/master/nxtoolkit/Dockerfile  
This docker file will create a docker image for nxtoolkit
5. Save the file `CMD-S`. Name the file `Dockerfile`.  Click save. (for windows users use ctrl+S)

### Exercise 2
####Creating the docker images for nxtoolkit
6. Switch back to your terminal window (Windows users use Git Bash)
go to training directory
6. `cd nxtoolkit`
7. `ls` <-- make sure your dockerfile is in this directory.
8. type `docker build -t nxtoolkit --tag hemakuma/nxtoolkit . `
9. or u can tag it too, make sure to use your id ..

    ***Don't forget the dot at the end. There is a space between nxtoolkit and dot.  For Windows users, don't forget the `winpty` in front of the docker command.
    It should build you the image for the nxtoolkit.  It should take about 5-10min to complete.***
9. Type `docker images` to verify it. Look for the nxtoolkit image.
![docker-3](/images/docker-c-3.png)
10. Now lets spin up one container using this image. This will be your nxtookkit container that have everything installed for you to get started with python coding.


###Exercise 3
####Spin up a nxtoolkit Container using this image
1. Switchback to the terminal window.
2. type `pwd` to verify your working directory. You should be in the `training/nxtoolkit` directory
3. Mac Users    
    `docker run --name nxtoolkit -h nxtoolkit -it --restart=always -v ~/training/nxtoolkit:/opt/nxtoolkit/myscripts  hemakuma/nxtoolkit`

4. Windows Users  
    Launch Docker Quickstart Terminal and run the following command:  
    `winpty docker run --name nxtoolkit -h nxtoolkit -it --restart=always -v ~/training/nxtoolkit:/opt/nxtoolkit/myscripts  hemakuma/nxtoolkit`

    -v option basically lets you mount a volume inside your container so that you can access it from your laptop HDD.  This way you can edit your code on your laptop but it will be available inside the container.
    You should now be inside your nxtoolkit container.
    ![docker-4](/images/docker-c-4.png)
4. type `ls` <-- you should see your myscripts directory
5. `cd myscripts`  
    Note `myscripts` directory inside the container is mapped to your `/training/nxtoolkit` directory on your laptop.  This means, anything you create on your laptop in the `~/training/nxtoolkit` directory will be available inside your container and vice-versa.
6. Copy the samples into your working directory. Make sure u are in your `myscripts` directory inside the container
    1. `cp -r  /opt/nxtoolkit/samples/ .`
    2. Exit the container  by typing `exit` inside the container.
9. Verify that sample folder is available on your laptop
    1. Switch to  atom editor window
    2. You should see the samples folder under nxtoolkit folder.

    ![folder](/images/docker-c-5.png)

You are now ready to do off box programming.


#Do not do this in the class
##Manual way of building images

## Jenkins  Container
### Exercise 1
####Creating Jenkins Container
1. Switch to `Atom` Editor
2. create a new folder under `containers`.
3. Right click on training and select `New Folder`
4. name it `jenkins`

### Exercise 2
###Create Dockerfile

Lets create a docker file to build a new image that has jenkins and ansible installed.  For jenkins, we will use prebuild image of Jenkins from dockerhub.  However we want to install ansible on it so that we can initiate deployment from the Jenkins server.

5. Right click on the `jenkins` folder and select `New File`
6. name it `Dockerfile`
7. paste the following

```
#VERSION 1.0
FROM jenkins
MAINTAINER Hemant Kumar, hemakuma@cisco.com
USER root
RUN apt-get update && apt-get -y upgrade && apt-get install -y  vim git python curl openssh-server  python-pip python-dev build-essential libssl-dev libffi-dev
RUN pip install --upgrade pip
RUN pip install ansible==2.1.1
RUN pip install markupsafe
RUN pip install cryptography
##if this fails, use pip install cryptography --upgrade; failed in jenkins container
## upgrade to the latest VERSION
RUN pip install ansible --upgrade
#RUN echo 172.16.123.135 leaf1 > /etc/hosts

```
7. `Cmd + S`, to save the file

### Exercise 3
####Build the new image
1. Open up a terminal window,
2. navigate to the training directory
3. type `source source.docker`
4. cd to `jenkins` folder
5. type `Docker build -t  jenkins-ansible --tag hemakuma/jenkins-ansible .`
6. This should build a new image
7. Verify that the images is in your local repo: `docker images | grep jenkins`
8. Run the jenkins-ansbile container based on this image.

	`docker run --name jenkins -h jenkins -d --restart=always -p 8080:8080 -p 50000:50000 hemakuma/jenkins-ansible`


##nxtookkit

**STOP HERE , we will do the rest of jenkins exercises in the Jenkins Section**
