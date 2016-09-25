
#Ansible - Using Ansible to manage NXOS devices
Ansible is an open-source software platform for configuring ,managing and orchestrating compute and switching infrastructure. Ansible features a state-driven resource model that describes the desired state of computer systems and services.  Ansible is agentless and uses push model while puppet and chef are pull model using agents on the host.  Ansible uses YAML to configure playbooks.
Playbooks contains one or multiple plays and plays contains one or multiple tasks.  Tasks talks to ansible module. Ansible module are python scripts that configures the devices in the inventory  list.   Playbook is applied to host inventory file.  Inventory file contains list of all the devices that needs to be automated. Within a single play, you can specify which host this play applies to  from the list of all available host defined in the inventory file.



**Hosts:** Remote machines Ansible manages.  
**Groups:** Group of hosts assigned to a pool that can be conveniently targeted and managed together.  
**Inventory:** File describing the Hosts and Groups in Ansible.  
**Modules:** Modules (also referred to as “task plugins” or “library plugins”) are the components that do the actual work in Ansible. They are what gets executed in each playbook task.  
**Playbooks:** A collection of plays which the Ansible Engine orchestrates, configures, administers, or deploys. These playbooks describe the policy to be executed to the host(s).  People refer to these playbooks as "design plans" which are designed to be human- readable and are developed in a basic text language called **YAML**.


#Exercise 1: Creating Ansible Container using Docker
1. In the terminal window.
2. go to your se-training folder.
3. source source.docker
4. Create a directory to store ansible scripts
5. mkdir ansible
6. cd ansible
7. Spin up a nxos ansible container
     a. docker run --name ansible -h ansible -it --restart=always -v ~/se-training/ansible:/nxos-ansible/myscripts --workdir /nxos-ansible/myscripts jedelman8/nxos-ansible
     b. winpty docker run --name ansible -h ansible -it --restart=always -v ~/se-training/ansible:/nxos-ansible/myscripts --workdir /nxos-ansible/myscripts jedelman8/nxos-ansible (for window users)

8. This will start the container and log you in. From inside the container. Copy all the example scripts on your own directory so that you can edit it using sublime on your laptop. inside the container
pwd ...you should be inside your myscripts directory.  if not  then type   cd /nxos-ansible/myscripts/
9. cp -r /nxos-ansible/example-playbooks/* .
10. copy the template directory to myscripts directory too
11. cp -r /nxos-ansible/templates/ .
12. ls
13. now all these files will be available on your sublime.
14. Go to your sublime editor and you should all these files under ansible folder.


#Exercise 2: Setting up the Ansible container
New In this exercise you will use vim editor to edit the files.  Learn how to use it as it is very commonly used editor in linux.  Here is the full cheat sheet.  http://vim.rtorr.com/
1. Switch to terminal  window (ansible container window)
2. Inside your ansible container, do the following
    a. add your username/password to .netauth file.
    b. vim ~/.netauth
    c. press i to insert;
    d. change the username and password (admin/cisco123)

    e. after done editing, press ESC
    f. then type  :wq
    g. this should save the save and exited out of vim editor

### Exercise 3:  Creating Host Inventory File
    Inventory file contains list of hosts that you want to manage from Ansible.  In our case, it will list of switches that we want to manage by ansible. These host/swiches can be organized in groups.

    1. Switch to `ATOM` Editor , close all the currently opened files.
    2. Open  the `ansible` folder and then right click on `how-to` folder and select `New File`.
    3. add the the following to the file http://gitlab.cisco.com/hemakuma/se-training/blob/master/ansible/how-to/hosts
    4. Delete all the host from the above file , except for the switch that is assigned to you.  Your host file should be similar to the picture below.
    5. We are using  ansible connection as local rather than ssh, because the nxos ansible modules use nxapi to configure the switches rather than ssh.
    6. Save the file `CMD + S`
    7. Name the file as  `hosts`
    You can read more about Inventory file here:  Inventory — Ansible Documentation

### Exercise 4:  Ping Test.  Make sure your Switch can reach internet.
    Most of the scripts are already created in the how-to folder.  You will modify it to meet our requirements.
    Lets modify the examples-ping.yml file using sublime.

    1. Switch to your Ansible terminal
    2. cd how-to
    3. cp examples-ping.yml hk-ping.yml
    4. Switch to your sublime window
    5. go to Ansible folder and then to how to folder
    6. double click "hk-ping" to edit the file.
    7. change line 4 from hosts: n9k1 to hosts: all to ping all the hosts in your inventory.
    8. HELP: http://gitlab.cisco.com/hemakuma/se-training/blob/master/ansible/how-to/hk-ping.yml
    9. CMD+S to save it
    10. Switch to ansible container terminal
        1. run this playbook.
        2. ansible-playbook -i hosts hk-ping.yml

### Exercise 5 - Practical Example - Automating VLAN provisioning
        #### Problem Statement
        Users perform manual Day-1 operations to configure network elements repeatedly for the purpose of on-boarding new workloads. One common category of Day-1 configuration activity is performing routine VLAN related operations:
        1. Check if VLAN exists
        2. Change names and descriptions of VLANs
        3. Configure a VLAN

        It would be beneficial to automate this repetitive configuration with a goal of reducing time and effort needed to complete the configurations, while reducing the probability of errors.

        Today networks are in high demand and change constantly due to proliferation of customer demands. Network engineers need the ability to automate their network in a simple and manageable way.  One of the most frequent network changes is creation and removal of VLANs where network engineers are accommodating dynamic customer demands.

        #### Solution  - Ansible automation
        Ensure following vlans are configured on all the switches.

        | Vlan id |	vlan name |
        |---------|-----------|
        |10	| web
        | 20 |	app
        |30	|db
        |40	|misc
        |99	|native-vlan

        Take a look at the "how to " folder and see if you can use any of the script to meet this requirement.
        Modify examples-vlan.yml.  Study the code,  notice that as you have many different ways of configuring the vlans.  Lets comment out some of them. In ansible, if you add # at the beginning of the line, ansible treats it as if it is a comment line, therefore it will not execute it.

        1. Make a copy of the sample playbook.
            1. Switch to your ansible terminal
            2. cp examples-vlan.yml  hk-vlan.yml
        2. Modify this file to meet your requirement
        5. Switch to sublime window
        6. double click hk-vlan.yml file to edit it.
        7. Change the hosts: field to match your switch name that you put in the hosts file.  The play will only configure this particular switch.

        Let's comment out first 3 task in this file.  to comment out, just add # infront of the line.
        change the vlan id and name to match your requirement
        Help:  http://gitlab.cisco.com/hemakuma/se-training/blob/master/ansible/how-to/hk-vlan.yml
        CMD+S to save it.
        Now lets first verify that we do not have these vlans configured on our switch.
        Switch to the Switch CLI terminal
        show vlan
        Run the ansible playbook
        Switch to ansible container terminal and type
        ansible-playbook -i hosts hk-vlan.yml
        Repeat step 3 and compare the result.
        Repeat step 4.  What do you notice? do you see all green?  This the idempotent nature of ansible.  It will not change anything if there is no change.  Compare is will cli, if you repeat the command, it will reconfig the switch eventhough it is already configured.


### Exercise 6 - Configure ipv4 address to the interface
        video

        Configure interface eth1/1 with 10.1.100.2/24
        1. Switch to your ansible terminal
        2. Lets make a copy of this script.
        3. cp examples-ipv4interface.yml  hk-ipv4interface.yml
        4. Switch to sublime editor
        5. double click the file and edit it. Modified as per requirement.  you will have to delete few things and make sure
        http://gitlab.cisco.com/hemakuma/se-training/blob/master/ansible/how-to/hk-ipv4interface.yml
        6. Switch to the Switch CLI terminal
        7. Verify that no ip address is configured on eth1/1
        8. show run interface
        9. Switch to ansible terminal
        10. run the playbook
        11. ansible-playbook -i hosts hk-ipv4interface.yml
        12. Repeat 3 and compare the result.

### Exercise 7 - Show neighbors
        video

        1. Switch to your ansible terminal
        2. Lets make a copy of this script.
        3. cp get-neighbors.yml  hk-get-neigbors.yml
        4. Switch to sublime windows
        5. edit the file
        6. modify the source and destination for the template.
        http://gitlab.cisco.com/hemakuma/se-training/blob/master/ansible/how-to/hk-get-neigbors.yml

        7. This tells ansible to use the jinja2 template named neigbhors.j2 located at /nxos-ansible/templates/ folder to format the output.  It also tells ansible to  place the out in the /nxos-ansible/myscripts/ folder and name it neighbors.json.
        register saves the output in a variable called my_neighbor.   Read more about register variable here  Conditionals — Ansible Documentation
        click  ansible --> templates --> neighbors.j2
        notice how the register variable is used in the template.
        8. Switch to ansible window
        9. run the playbook
        10. ansible-playbook  -i hosts hk-get-neigbors.yml
        11. Switch to Sublime Editor
        12. Examine the output
        13. click on ansible folder.
        14. you should see neighbors.json file.  Double click it and examine the output.
