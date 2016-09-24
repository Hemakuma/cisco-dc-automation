NXToolKit
---
**Table of Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [NXToolKit](#nxtoolkit)   
   - [Introduction](#introduction)   
      - [Exercise 1](#exercise-1)   
         - [Creating  login credential file.](#creating-login-credential-file)   
      - [Exercise 2](#exercise-2)   
         - [Logging in your nxtoolkit container](#logging-in-your-nxtoolkit-container)   
      - [Exercise 3](#exercise-3)   
         - [Running nxos show commands using nxtoolkit](#running-nxos-show-commands-using-nxtoolkit)   
      - [Exercise 4](#exercise-4)   
         - [Showing all vlans configured on the switch](#showing-all-vlans-configured-on-the-switch)   
      - [Exercise 5](#exercise-5)   
         - [Create a Script that displays all the interfaces that have "operational state" as up.](#create-a-script-that-displays-all-the-interfaces-that-have-operational-state-as-up)   
      - [Exercise 6](#exercise-6)   
         - [Configuring VLAN using nxtoolkit](#configuring-vlan-using-nxtoolkit)   
      - [Exercise 7](#exercise-7)   
         - [Configuring Physical interface](#configuring-physical-interface)   
      - [Exercise 8](#exercise-8)   
         - [Configuring L3 Interface](#configuring-l3-interface)   
      - [Exercise 9](#exercise-9)   
         - [Configuring SVI (optional)](#configuring-svi-optional)   
      - [Exercise 10](#exercise-10)   
         - [Deleting SVI (optional)](#deleting-svi-optional)   

<!-- /MDTOC -->

Introduction
---
NXtoolkit is a object based programmability in the Open NX-OS.Nxtoolkit is a set of python libraries that allow basic configuration of the Cisco Nexus 9000/3000 Series Switch. It is intended to allow users to quickly begin using the REST API and accelerate the process of programming and automating a Cisco Nexus 9000/3000 based network.

It abstracts away the complexity of the API.  
https://github.com/datacenter/nxtoolkit

### Exercise 1
#### Creating  login credential file.
All the python scripts requires switch login information (Credentials).  You can either manually type it each time you run these sample scripts or you can put it in file called `credentials.py` file and the scripts will automatically read the credentials from this file.

1. Switch to  `ATOM` editor window
2. Close all the files that are open in ATOM to make space for nxtoolkit programming files.
3. Make sure you see the `sample` folder under `nxtoolkit` folder. Expand this folder to see all the files. All your object based scripts are located in the this folder.
4. In your ATOM editor, create a new file called `credentials.py`.  
5. Right click on the `samples` folder under `nxtoolkit` folder and select `NEW File`
5. Copy and paste the following content in this file. Make sure to change the `URL` to point to your  switch ip.
https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/credentials.py
5. Save the file. `CMD+S`
6. close the file


### Exercise 2
#### Logging in your nxtoolkit container
1. Close any terminal window you have already open.
2. Open a new terminal window.  For exercises in this section, we will use this terminal.  (windows users, open up `git bash` terminal)
3. Login to the container
   ```
   cd ~
   cd training
   source source.docker
   docker attach nxtoolkit

   ```

### Exercise 3
#### Running nxos show commands using nxtoolkit
1. Inside nxtoolkit container, type the following.
2. `cd myscripts`
3. `cd samples`
4. `ls  | grep show ` <-- to see all the show scripts
5. Lets run the few of show interfaces scripts.
6. Since all the files are executable, we can just run it without typing python. It will automatically associate it with python program.
7. Type `./nx-show-interfaces.py`  to run  the script or you can type `python nx-show-interfaces.py`
you don't need to pass any credentials as it is automatically read from the `credentials.py` file that you created.

   ![nxtoolkit](/images/nxtoolkit-30.png)
8. Examine the output
9. Switch to ATOM window
10. Navigate to the samples folder and located the `nx-show-interfaces.py` file
11. Look at the code and try to make sense of what is doing.  Open up the git hub documentation and read the `nxtoolkit.py`

Basically the script logins into the switches, uses `interface.get` function to get all the interfaces.  Then it puts the data inside a python list. This list contains tuples that contains interface attributes such as interface id, admin state, operation state et.   Then it displays the data from the dictionary using jinja2 template.
If you have time, get a look at the what "interface class" module does..GitHub - datacenter/nxtoolkit
https://github.com/datacenter/nxtoolkit/blob/master/nxtoolkit/nxphysobject.py

### Exercise 4
#### Showing all vlans configured on the switch

1. Switch to  your nxostoolkit terminal
2. type `./nx-show-l2bds.py`
3. note down the vlans you see.
4. Open up another Terminal Window
5. ssh to your switch
   6. `ssh admin@10.114.217.xx`
   7. configure t
   8. vlan 10
9. Switch to  your nxostoolkit terminal
10. `./nx-show-l2bds.py`
11. compare the results with step 1.

**TIP:**  Going forward, keep both of the terminal window open; use nxtoolkit terminal to run the python scripts and use the ssh session to switch to verify the configurations.

### Exercise 5
#### Create a Script that displays all the interfaces that have "operational state" as up.

1. Lets make copy of `nx-show-interfaces.py` and modify the copy  (u can name it anything you like but i usually put my initials as prefix to the original script so that i can easily find my scripts later )
2. In your nxtoolkit terminal
3. `pwd`
4. you should be inside the
5. `/opt/nxtoolkit/myscripts/samples`
6. `cp nx-show-interfaces.py hk-show-interfaces.py`
7. Go to `ATOM` editor
8. open up this file by double clicking it.
9. Modify `hk-show-interfaces.py` script to achieve the above goal. You want to see all the interfaces whose `operational state` is `up`
10. HELP : https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/hk-show-interfaces.py
11. Switch to your nxtoolkit terminal  and run the script
12. `./hk-show-interfaces.py`
13. Switch to  switch CLI terminal , shut some of the interfaces eg
   ```
   config t
   int eth 1/4
   shut
   ```
14. Repeat step 4
15. compare the results.

   ![nxtoolkit](/images/nxtoolkit-31.png)

**Note:**
All interfaces are store in a python list called `data`.  This list contains tuples that has attribute of each interface.
Here is a sample list with  tuples looks like
[('eth1/33', 'leaf', 'up', u'down', 'auto', '1500', 'discovery'), ('eth1/34', 'leaf', 'up', u'down', 'auto', '1500', 'discovery'), ('eth1/36', 'leaf', 'up', u'down', 'auto', '1500', 'discovery'), ('eth1/37', 'leaf', 'up', u'down', 'auto', '1500', 'discovery'), ('eth1/38', 'leaf', 'up', u'down', 'auto', '1500', 'discovery'),.....]

Now we want to display only the interfaces that have "operational state = up".   This means we need to do some list comprehension.  Start learning Python now . We have lots of references on the  jive site.

![nxtoolkit](/images/nxtoolkit-32.png)

Comprehensions — Python 3 Patterns, Recipes and Idioms
```
   data_up = [x for x in data if x[3] == "up"]
   for d in  data_up:
   print(template.format(*d))
```
data_up = [x for x in data if x[3] == "up"]
x in data = tuple in the list data eg  x = ('eth1/34', 'leaf', 'up', u'down', 'auto', '1500', 'discovery').  if you want to look at the  operational state, you need to compare the 3rd item in the tuple like this x[3] <-- this is one of the tuple in the data list.

x[3] = refers to 3rd  column in the tuple, which is the operational state.
Now this will give you another list in which all the interfaces have operational state up.
You can print this list.
Your output should look similar to this

List Comprehensions in Python


### Exercise 6
#### Configuring VLAN using nxtoolkit
**Goal:** Configure Vlan 300 and 301 using nxtoolkit

**Solution:** Modify `nx-create-l2bds.py`

1. Switch to nxtoolkit window
   1. make sure you are in samples directory
   2. `cp nx-create-l2bds.py hk-create-l2dbs.py`
2. Switch to `ATOM `window
   1. Modify the file  `hk-create-l2bds.py` as per the requirement.
   2. HELP: https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/hk-create-l2bds.py
   3. Save the file `CMD+S`
3. Switch to nxtoolkit terminal window.
   1. Before you run your script, verify that vlan does not exist on the switch using your script.
   2. `./nx-show-l2bds.py`
   3. run the script  `./hk-create-l2bds.py`
   4. Verify on the switch that both of the vlans are now configured using the script
   5. `./nx-show-l2bds.py`


**References**
https://github.com/datacenter/nxtoolkit/blob/master/nxtoolkit/nxtoolkit.py

By now you should have got the hang of how to run scripts. How to switch between terminals , how to get in container , how to edit file etc.  Therefore  going forward, we will not show you all the details.  This will help you with your learning.  Cut /paste does not help much.  If you get stuck, check out my files on the gitllab

http://gitlab.cisco.com/hemakuma/se-training/tree/master/nxtoolkit/samples


### Exercise 7
#### Configuring Physical interface
In this exercise, we will configure interface eth1/2, eth1/3 as follows:
```
access vlan: 300
description: configured using nxtoolkit
adminstate : down
description: "Configured by nxtoolkit"
```

1. Take a look at sample scripts to see if any of the script can help with your requirement.
2. Looks like `nx-config-interface.py`  is close to our requirement.
3. Lets make a copy of it and modify it.
4. Switch to your nxtoolkit terminal
5. `cp nx-config-interface.py hk-config-interface.py`
5. Switch to `ATOM` window.  Double click `hk-config-interface.py` to edit it.
6. Change the code to reflect your requirement.

   HELP:  https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/hk-config-interface.py
7. Save the file  `CMD+S`
8. Switch to switch CLI terminal and to verify the current configuration before modifying it.
`show run interface`
9. Switch to nxtoolkit window
10. Run the script from the nxtookit container.
`./hk-config-interface.py`
11. Repeat step 8 and compare the result.

**Reference**
Take a look at all the libraries that nxtoolkit uses at git hub.  Try to make sense of it.  https://github.com/datacenter/nxtoolkit/blob/master/nxtoolkit/nxphysobject.py

### Exercise 8
#### Configuring L3 Interface

**Goal:** Configure interface eth1/2 with ip address of 192.168.60.1/24

**Solution:**  modify `nx-config-ipv4.py`

1. Switch to your nxtoolkit container terminal
2.    `cp nx-config-ipv4.py hk-config-ipv4.py`
3. Switch to ATOM
   1. modify the file as per the requirement.
   2. Hint: you will need to change the interface to layer 3 mode first before configuring the ip.
   2. HELP: https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/hk-config-ipv4.py
   3. Save the file
4. Switch to nxtoolkit container
   1. run the file the script
   2. `./hk-config-ipv4.py`
5. Verify the configuration on the switch CLI.

### Exercise 9
#### Configuring SVI (optional)
Optional ...do it later; not in the class

Configure following SVIs:

   1. vlan 300:  192.168.50.1/24
   2. vlan 301: 192.168.51.1/24

Note: vlans should be already created by the script in exercise 6.

**Solution:**  Modify `nx-config-svi.py`

1. Login to your nxtoolkit container
2. `cp nx-config-svi.py hk-config-svi.py`
3. Switch to  ATOM.
3. Modify the file as per the requirement.  Hint: the code is at the end of the file. You have to uncomment it.
4. Save the file
5. Inside the nxtoolkit container, run this script.
6. Verify on the switch that both of the vlans are configured.

   HELP  https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/hk-config-svi.py

### Exercise 10
#### Deleting SVI (optional)
Delete following SVI
vlan 301

**Solution:** Modify `nx-config-svi.py`

1. Login to your nxtoolkit container
2. `cp hk-config-svi.py hk-delete-svi.py`
3. Open the file using ATOM.
4. Modify the file as per the requirement.
5. Save the file
6. On the nxtoolkit container, run the file.
7. Verify on the switch that both of the vlans are configured.
   1. `show run int vlan300`
