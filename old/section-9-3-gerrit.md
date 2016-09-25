#Gerrit
Gerrit is a free, web-based team code collaboration tool. Software developers in a team can review each other's modifications on their source code using a Web browser and approve or reject those changes. It integrates closely with Git, a distributed version control system

![gerrit](/images/gerrit-101.png)

![gerrit](/images/gerrit-2.png)


##Installing Gerrit
Use docker container
####Use docker compose files

1. Switch to `Atoms` Editor
2. Create a folder called `gerrit` under `containers` folder
3. Right click on `gerrit` folder and select `New File`
4. copy and paste this content:

```
---
version: '2'
services:
  gerrit:
    image: 'gerritforge/gerrit-ubuntu15.04'
    container_name: gerrit
    restart: always
    hostname: 'gerrit.cisco.com'
    ports:
      - '8081:8080'
      - '29418:29418'

```

#### Run the docker compose
1. Switch to the terminal window
2. navigate to the `gerrit` folder
3. type  `docker-compose up -d`
4. it will take couple of mins and then your gerrit environment is ready to use.


#### Configure gerrit
1. Switch to Chrome browser.
2. got to
3. `http://<ip of your docker host>:8081`
4. Click on `Become`  (on the right side of the screen)
5. Click on the `New Account`
6. Select a username
7. update your laptops ssh key . your public ssh key is located in `~/.ssh/id_rsa.pub`
8. click on become and type your username

## Configuring
