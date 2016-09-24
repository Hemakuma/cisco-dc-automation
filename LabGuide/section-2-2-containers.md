Docker Containers
---
**Table of Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [Docker Containers](#docker-containers)   
   - [Gitlab and Jenkins Container](#gitlab-and-jenkins-container)   
      - [Exercise-1](#exercise-1)   
         - [Create docker compose file to install gitlab and jenkins](#create-docker-compose-file-to-install-gitlab-and-jenkins)   
      - [Exercise-2](#exercise-2)   
         - [Spin up gitlab and jenkins container](#spin-up-gitlab-and-jenkins-container)   
   - [Ansible Docker Container](#ansible-docker-container)   
      - [Exercise-1](#exercise-1)   
         - [Run the Ansible container](#run-the-ansible-container)   
   - [Nxtoolkit Container](#nxtoolkit-container)   
      - [Exercise-1](#exercise-1)   
         - [Run the Nxtoolkit container](#run-the-nxtoolkit-container)   
      - [Exercise 2](#exercise-2)   
         - [Spin up a nxtoolkit Container using this image](#spin-up-a-nxtoolkit-container-using-this-image)   
   - [Docker Tips](#docker-tips)   

<!-- /MDTOC -->


------

Lets build some containers on our docker engine.

## Gitlab and Jenkins Container
We will use opensource gitlab server for distributed source control of our files in this lab.  We could have used github but we wanted to see full integration with Jenkins and other tools, therefore we installed all the tools locally.  With dockers, installing these opensource software is pretty easy.

### Exercise-1
#### Create docker compose file to install gitlab and jenkins
Docker compose is one way of spinning up docker containers. Basically you create a docker-compose.yml file and run docker-compose up command.

1. Switch to the terminal window
  2. type `docker-machine ip default`
  3. write this ip somewhere. You will need this through out the lab.
2. Switch to `ATOM` editor.
  1. Right click on the `training` Folder and select `New file`.
  2. name it `docker-compose.yml`
  3. Copy and Paste content into the file and save it.  Use the contents from github url. Those are more upto date content.
  4. Make sure to change the `external_url` to reflect the ip address of you docker machine.
  https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/Docker-compose.yml

  ```
      ---
      version: '2'
      services:
        gitlab:
          image: 'gitlab/gitlab-ce:latest'
          container_name: gitlab
          restart: always
          hostname: 'gitlab.cisco.com'
          environment:
            GITLAB_OMNIBUS_CONFIG: |
              external_url 'http://192.168.99.100'
              #  change the ip to match your docker host
              # Add any other gitlab.rb configuration here, each on its own line
          ports:
            - '80:80'
            - '443:443'
            - '2222:22'

          volumes:
            - '/srv/gitlab/config:/etc/gitlab'
            - '/srv/gitlab/logs:/var/log/gitlab'
            - '/srv/gitlab/data:/var/opt/gitlab'

        jenkins:
            image: hemakuma/jenkins-ansible
            container_name: jenkins-ansible
            restart: always
            hostname: jenkins-ansible
            ports:
              - '8080:8080'
              - '50000:50000'

  ```

3. Save the file `CMD + S`

### Exercise-2
#### Spin up gitlab and jenkins container

1. Switch to the terminal window
2. cd `training`
3. type ` docker-compose up -d`
4. this should start up the gitlab and jenkins container.  It will take couple of mins to download the images.

  ![docker-comp](/images/docker-comp-1.png)

5. Wait for this process to finish.  It should take couple of mins.
6. Verify that your gitlab and jenkins container is running. Type  `docker ps`
7. type `docker-machine ip default`  Note down this ip somewhere, you will need it throughout this lab.

**We will configure gitlab and jenkins in later section**


## Ansible Docker Container
Lets install Ansible on  a docker container.

### Exercise-1
#### Run the Ansible container
1. Switch to `ATOM` editor
  1. Right click on the `training` folder and select `New Folder`.  Name it `ansible`.
  2. Right click on the `ansible` folder and select `New File`. Name it `README.md`
  3. Type something eg `This is my first edit` in it and save.
2. Switch to the terminal window
  2. Spin up a nxos ansible container.  Remember to replace my userid `hemakuma` with your.  Not do not replace it for the image name. Leave the image name to `hemakuma/ansible`
      1. Mac Users  
      `docker run --name ansible -h ansible -it --restart=always -v ~/training/ansible:/nxos-ansible/myscripts --workdir /nxos-ansible/myscripts hemakuma/ansible`
      2. Windows Users  
      ` docker run --name ansible -h ansible -it --restart=always -v ~/training/ansible:/nxos-ansible/myscripts --workdir /nxos-ansible/myscripts hemakuma/ansible`

        **Windows users if you having issues, try running the command directly with winpty added to the start of the docker command`**

    This will start the container and log you in. From inside the container.

  2. Exit the container by typing `exit`.  We will use this container later in the ansible exercises.


## Nxtoolkit Container
Nxtoolkit container is prebuilt container that has Cisco nxtoolkit installed.  nxtoolkit provides python libraries and examples on how to interact with Cisco's Nexus switches using NXAPI.
### Exercise-1
#### Run the Nxtoolkit container
1. Switch to `ATOM`
2. Right click on the `training` folder and select `New Folder`. Name it `nxtoolkit`

### Exercise 2
#### Spin up a nxtoolkit Container using this image
1. Switchback to the terminal window.
2. type `pwd` to verify your working directory. You should be in the `training/nxtoolkit` directory
3. Mac Users    
    `docker run --name nxtoolkit -h nxtoolkit -it --restart=always -v ~/training/nxtoolkit:/opt/nxtoolkit/myscripts  hemakuma/nxtoolkit`

4. Windows Users  
    Launch Docker Quickstart Terminal and run the following command:  
    `winpty docker run --name nxtoolkit -h nxtoolkit -it --restart=always -v ~/training/nxtoolkit:/opt/nxtoolkit/myscripts  hemakuma/nxtoolkit`

    -v option basically lets you mount a volume inside your container so that you can access it from your laptop HDD.  This way you can edit your code on your laptop but it will be available inside the container.

    **You should now be inside your nxtoolkit container.**
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



## Docker Tips
If your boot2docker can not resolve dns entry?
`docker-machine default ssh`

```
echo "nameserver 8.8.8.8" > /etc/resolv.conf && sudo /etc/init.d/docker restart

```

Sometimes the images gets large and u do not have enough space on the VM
` df -h `

to check the disk usage.

you can use --disksize option of docker-machine create to create a bigger size disk.  By default its only 2G.

**Some common docker commands**
```

docker ps
docker run
docker attach <container name>
docker info
docker inspect <container name>

```
