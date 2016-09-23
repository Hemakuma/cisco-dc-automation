
NXOS Sandbox
---
**Table of Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [NXOS Sandbox](#nxos-sandbox)   
   - [Introduction](#introduction)   
      - [Exercise 1](#exercise-1)   
         - [Enabling NXAPI on your NXOSv Switch](#enabling-nxapi-on-your-nxosv-switch)   
      - [Exercise 2](#exercise-2)   
         - [Accessing and using the NXAPI using Sandbox built inside the switch.](#accessing-and-using-the-nxapi-using-sandbox-built-inside-the-switch)   
      - [Exercise 3](#exercise-3)   
         - [Show command from Sandbox.](#show-command-from-sandbox)   
      - [Exercise 4](#exercise-4)   
         - [Configuring VLAN 999 using Sandbox](#configuring-vlan-999-using-sandbox)   
      - [Exercise 5](#exercise-5)   
         - [Generating Python Code.](#generating-python-code)   

<!-- /MDTOC -->


Introduction
---

**NXOS provide a sandbox environment to generate your python codes.  Note, sandbox uses NXAPI-CLI, basically sending cli command wrapped in json or xml over http. It like running CLI but without logging in the switch. You can access this sandbox via web browser and pointing to the ip address of the switch.  Make sure feature nxapi is enabled.**

![sandbox](/images/sandbox-1.png)

### Exercise 1
#### Enabling NXAPI on your NXOSv Switch
1. Open up a terminal Window (or gitbash).
2. ssh to your switch (if you using nxosv, see nxov section).  use admin/cisco123
    1. `ssh admin@<ip of the switch>`    eg for mac nxosv you can also do `telnet 127.0.0.0 9002`
    2. `config t`
    3. `feature nxapi`
    4. `show nxapi`
    5. `show ip int brief vrf management`

![sandbox](/images/sandbox-2.png)

### Exercise 2
#### Accessing and using the NXAPI using Sandbox built inside the switch.
1. Open `Chrome` browser.
2. Point your browser to the switch ip address.
3. `http://<switchip>`
4. login in using your username and password.   `admin/cisco123`

### Exercise 3
#### Show command from Sandbox.
1. In the command window, type `show vlan` , and click `POST`.
2. make sure in the message format, you have selected `json` and command type = `cli_show`
3. notice the objects in both the `Request` and `Response` window.  Basically object are key value pairs. It is structured, so that you can query and key and get the response.  It becomes very easy to parse it now.
3. In the Response Window, check if  999 is present.  You should see only one VLAN in the response window.

![sandbox](/images/sandbox-13.png)

### Exercise 4
#### Configuring VLAN 999 using Sandbox
1. Now configure `VLAN 999`.  See the screen capture below.
2. In the command window, type
    ```
    config t
    vlan 999
    name web
    ```
    as shown below.

3. make sure that you have selected message format to be `json` and command type to be `cli_config`
4. Click on `POST` to send the commands via http to the switch.

    **NX-API Browser interface supports several command types. Make sure you have it set for cli_config in this case or your copied code will execute with an error.**

    ![sandbox](/images/sandbox-4.png)

5. In the `RESPONSE` window, make sure you get status `code 200`
6. Click on the `Python button` in the Request Window  and copy the content.  We will use this for the next exercise.
7. Switch to the ssh session of the switch
    1. verify that vlan 999 is configured on the switch
    2. `show vlan`

### Exercise 5
#### Generating Python Code.
You can use sandbox to generate your base python code for off box programming.

1. Save this python script that you copied from exercise 4 to a file (create-vlan.py)
1. Switch to  `ATOM` editor window
5. Right click on `nxtookit` folder and select create `New File`. name it `create-vlan.py`
6. paste the content in this file
7. save the file (`CMD + S or Ctrl+S`).


***We will use this file in the next section, when we do off-box programming using python***
