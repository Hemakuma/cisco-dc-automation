NXOS Onbox Programmability Guide
----
**Table of Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [NXOS Onbox Programmability Guide](#nxos-onbox-programmability-guide)   
   - [** In progress ..do not do this section**](#in-progress-do-not-do-this-section)   
   - [Guestshell (optional)](#guestshell-optional)   
      - [Exercise 1 - Playing with guestshell](#exercise-1-playing-with-guestshell)   
   - [Scheduler](#scheduler)   
   - [Python](#python)   
   - [Bash Shell (optional)](#bash-shell-optional)   
      - [Exercise 1:  Enable bash shell](#exercise-1-enable-bash-shell)   
      - [Exercise 2:  Using with bash shell](#exercise-2-using-with-bash-shell)   

<!-- /MDTOC -->



** In progress ..do not do this section**
----




## Guestshell (optional)

Refer to the Cisco live lab guide here
http://ngne.ciscolive.com/



### Exercise 1 - Playing with guestshell
1. Run command in guest shell
2. Make sure your NXOSv switch is running on VMware fusion or VMware Player. For more details see  NXOSv on VM Fusion or  NX-OSv in VMWare Player on Windows 7
3. Connect to the switch using serial port and get the ip address of the switch.

Open a terminal window (or putty) and Login to the switch using ssh
ssh admin@192.168.102.128   (password = cisco123)
guestshell
This will get you inside the guestshell
Try your favorite linux commands in guest shell
pwd
ip add
ip link
ethtool -S Eth1-1  <--shows the statistics of the port
Type "exit" to exit the guestshell
## Scheduler
<skip this>
This is covered in Cisco live labs.  See the jive link for more details

#EEM

See cisco EEM


## Python

## Bash Shell (optional)
Bash Shell
### Exercise 1:  Enable bash shell
1. ssh to your switch   ssh admin@<switchip>
2. config t
3. feature bash
4. run bash to get in the bash terminal.

### Exercise 2:  Using with bash shell
1. Run some linux networking commands.
2. ifconfig
3. ip link
4. yum info bgp
5. rpm -qa | grep bgp
6. Set the mtu of interface Eth1/1 to 9000
check the current mtu
ifconfig Eth1-1 | grep MTU
change the mtu
sudo ifconfig Eth1-1 mtu 9216
verify
ifconfig Eth1-1 | grep MTU
Exercise 3:  Practical Example
using bash shell, print all the interface that admin up
vsh -c "show interface brief" | grep up | awk '{print $1}'
Exit bash shell by typing  "exit"
