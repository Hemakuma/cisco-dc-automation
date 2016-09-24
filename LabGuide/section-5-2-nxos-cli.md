Off Box Programming
----
**Table of Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [Off Box Programming](#off-box-programming)   
   - [Introduction](#introduction)   
   - [NXAPI CLI Based](#nxapi-cli-based)   
      - [Exercise-1](#exercise-1)   
         - [Creating VLAN using python code.](#creating-vlan-using-python-code)   
      - [Exercise-2](#exercise-2)   
         - [VLAN Consistency Checker](#vlan-consistency-checker)   
      - [Exercise-3](#exercise-3)   
         - [Write a python script to show the version of the switch.](#write-a-python-script-to-show-the-version-of-the-switch)   
      - [Exercise-4](#exercise-4)   
         - [Downloading Sample Codes](#downloading-sample-codes)   
      - [Exercise-5](#exercise-5)   
         - [Doing more complex scripting](#doing-more-complex-scripting)   

<!-- /MDTOC -->

Introduction
---
There are different ways that you can modify the attributes of the managed objects (MO). See picture below.  In this section will be using NXAPI-CLI based and NXAPI-REST based to manipulate these objects.
Both methods are modifying the underlaying MOs.  NXAPI-CLI is sending CLI commands over HTTP to the switch.  The webserver in the switch strips off the http and sends the command to the CLI API which then modifies the MO.  NXAPI-REST directly modifying the MO.

![ob-1](/images/ob-2.png)  

## NXAPI CLI Based
NX-API CLI based, is a easy way to programmatically configure and extract information  from Nexus switches. However it is not a REST API,   basically it send cli command wrapped around  HTTP in json or xml format . The response you get are also in json or xml which is very easy to manipulate.
It uses pycsco library.
https://github.com/jedelman8/pycsco: python modules to simplify the use of working with cisco nexus switches

### Exercise-1
#### Creating VLAN using python code.
The best way to get started is to use the sandbox to generate your base code and then modify it to meet your requirement.

1. Create vlan 900 using python code generated from sandbox.
    1. Switch to `ATOM` text editor
    2. Open up the `create-vlan.py` file.  It should be already open. Remember we created this file  in the Sandbox section. If you skipped that part, go back and redo it.
    3. Lets modify the content of `create-vlan.py` file.  Change `YOURIP = <switchip>`, USER=admin, PASSWORD=cisco123
    4. change the vlan to `900` and name it `db`

        ![nxcli-1](/images/nxcli-1.png)

    5.  `CMD+S` to save the file

2. Attach to your nxtoolkit container
    1. Open up a terminal and go to your `training`  directory
    2. type `cd training`
    3. type `source source.docker`
    4. `docker attach nxtoolkit`  (for windows users  make sure you are using `docker quick terminal`)
    5. Press enter twice to see the bash shell of the container
    6. This will log you into your nxtoolkit container.  You should see `root@nxtoolkit` prompt

    ![nxcli-2](/images/nxcli-2.png)

3. Verify that your create-vlan.py exists in the container.
    1. `cd myscripts`
    2. `ls` <-- you should see "create-vlan.py" file
4. Run the python script to create the vlan
    1. From inside the nxtoolkit container. Type
    2. `python create-vlan.py`
5. Verify that vlan 900 is now configured on the switch
    1. Switch  to your `ssh session to the switch` terminal
    2. `show run vlan`  <-- verify that vlan got created

### Exercise-2
#### VLAN Consistency Checker
Its no fun just creating one vlan. We could do this faster with CLI.   Lets write up a little complex script.  This script uses `python lists` and `for` loops.  Google it up and learn little bit about lists and loops.  The goal should be to start learning from others code.  See how they do it and they practice with your own. Google if it doesn't make sense.

**Goal:** Create a script to ensure the required vlans are present all the switches you manage in your network.  This is a very common requirement for lots of customers.  They want to make sure all the switches have same vlans configured.

1. In `ATOM` , right click on `nxtoolkit` and create `New File`. Name it `vlan-consist-check.py`  
2. Get the code from git hub here

    https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/vlan-consist-check.py


3. copy and  paste the code in new file.  
4. Modify the file to reflect your switch ip, username + password

    ![nxcli-1](/images/nxcli-4.png)

5. Save the file `CMD + S` as `vlan-consist-check.py`  
6. Read the code and try to make sense of it.  Keep doing this with all the scripts we write.  You will see a pattern on how the codes are written.  
7. Switch to nxtoolkit container terminal window and run this script.
   1. This script should be in your `myscript` folder.  
   2. run the script `python vlan-consist-check.py`  
9. Switch to your switch console window
   10. verify that those vlans are created.  `Show run vlan`
11. Re-run the script. This time it should not make any changes because  all the required vlans are already on the switch.  Go ahead and delete one vlan and try to re-run the script.


### Exercise-3
#### Write a python script to show the version of the switch.
**Homework.  Do not do it in the class.**

Use the sandbox to generate the python code to show the version os running on your switch. Make sure to use `json-rpc`.  It is easier to read json rpc format.

![nxcli-1](/images/nx-cli-1.png)

My script is found here

 https://github.com/Hemakuma/networkautomation/blob/master/configs/hk-show-version.py

You can modify this script so that it can get the current version of all the switches in your network. You will need created a file called `switch_ips` that lists all the switch.  The script reads this file to get which switch it needs to contact to get the version information.  My switch ip file looks like this

https://github.com/Hemakuma/networkautomation/blob/master/configs/switch_IPs

You need to put the switch_ip file in the same directory as your script.
Your output should look like this  

![nxcli-1](/images/nxcli-3.png)

*Keep the nxtoolkit container, ATOM text editor and ssh  session  to switch  open all the time. We will be using all of them frequently.  Learn how to switch between them quickly to check and verify your configurations.*

### Exercise-4
#### Downloading Sample Codes
We do not have to reinvent the wheel.  Most of the time, people have already written the script to meet some of your requirement.  All you need to do is to take that script and modify it to meet your requirement.  Some sources of script are git hub,  devnet community etc.
https://github.com/datacenter/who-moved-my-cli  
Lets  download some sample codes from git.

1. Learn how to clone someone's repo using `git clone` command.  
2. Switch to our nxtoolkit terminal  
    1. `cd /opt/nxtoolkit/myscripts/`
    2. `git clone https://github.com/datacenter/who-moved-my-cli.git`
    3. `cd who-moved-my-cli`   <-- this is the directory where u will find all the sample scripts that you just downloaded.  
3. Go to `ATOM` editor and you should see all the codes that you just downloaded.

### Exercise-5
#### Doing more complex scripting
*You will not be able to try this script in the lab as you only have one switch.  You need at least 2 switch for this script to work. Try this with real switches.*  
The beauty of scripting comes, when u start manipulating the data and creating loops to do more complex task.

1. using ATOM, open `cdp2descv2.py` file located in the `who-moved-my-cli` folder.  
2. Examine the code and try to understand what it is doing.  Google, python dictionary and learn what it does.
3. switch to your ssh session to the switch
4. Enable scp server on your switch. `config t`,   `feature scp-server`
5. Switch back to your nxtoolkit terminal
6. Under nxtoolkit directory
    1. `cd who-moved-my-cli`
    2. copy cdp2descv2.py file to the bootflash of your switch.
    3. `scp cdp2descv2.py admin@<yourswitch ip>:`
    4. don't forget the : at the end.

    ![nxcli-1](/images/nxcli-5.png)

7. Switch to the switch CLI terminal.
    1. `dir`
    2. make sure you see the file you just copied.
    3. `show interface description`
    4. make sure no description is configured on any of the interface.
    5. This script configures interfaces description based on the cdp neighbor information.
    6. Run the script on the switch.
    7. `python bootflash:/cdp2descv2.py`
    8. `show interface description`
    9. check the mgmt interface.
8. Since you don't have other switches connected to your virtual switch, you will get some errors
9. Here is the output of from my lab.  

    ![nxcli-1](/images/nxcli-6.png)  

*With Camden release, we have introduced object based restful NX API.  I prefer using objects rather than using CLI wrapped into http. Next section, we will programming using REST API*
