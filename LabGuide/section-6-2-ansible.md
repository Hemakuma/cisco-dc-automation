 Using Ansible to automate NXOS switches
---
**Table of  Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [**Table of  Contents -- Using Ansible to automate NXOS switches**](#table-of-contents-using-ansible-to-automate-nxos-switches)   
   - [Ansible - Introduction](#ansible-introduction)   
   - [Section-6-2-1 : Ansible Setup](#section-6-2-1-ansible-setup)   
      - [Exercise 1](#exercise-1)   
         - [Setting up the directory structure to host ansible files](#setting-up-the-directory-structure-to-host-ansible-files)   
      - [Exercise 2](#exercise-2)   
         - [Creating Host Inventory File](#creating-host-inventory-file)   
      - [Exercise 3](#exercise-3)   
         - [Creating credentials file](#creating-credentials-file)   
      - [Exercise 4](#exercise-4)   
         - [Ping Test.  Make sure your Switch can reach internet.](#ping-test-make-sure-your-switch-can-reach-internet)   
         - [Login in to your Ansible Container and Run the Playbook](#login-in-to-your-ansible-container-and-run-the-playbook)   
   - [Section-6-2-2 : Simple Ansible Playbooks](#section-6-2-2-simple-ansible-playbooks)   
      - [Exercise 1](#exercise-1)   
         - [Gather facts about the switch](#gather-facts-about-the-switch)   
      - [Exercise 2](#exercise-2)   
         - [Using the nxos_config module](#using-the-nxos_config-module)   
   - [Ansible Roles - Introduction](#ansible-roles-introduction)   
   - [Section-6-2-3: Base Configuration](#section-6-2-3-base-configuration)   
      - [Exercise 1](#exercise-1)   
         - [Create ansible roles directory structure](#create-ansible-roles-directory-structure)   
      - [Exercise 2](#exercise-2)   
         - [Configuring login roles](#configuring-login-roles)   
      - [Exercise 3](#exercise-3)   
         - [Base configuration repository](#base-configuration-repository)   
      - [Exercise 4](#exercise-4)   
         - [Create jinja2 template for the base configuration.](#create-jinja2-template-for-the-base-configuration)   
      - [Exercise 5](#exercise-5)   
         - [Create handler to save the configuration](#create-handler-to-save-the-configuration)   
      - [Exercise 6](#exercise-6)   
         - [Create playbook tasks for base configuration.](#create-playbook-tasks-for-base-configuration)   
      - [Exercise 7](#exercise-7)   
         - [Create playbook to push this base configuration to the switch.](#create-playbook-to-push-this-base-configuration-to-the-switch)   
      - [Exercise 8](#exercise-8)   
         - [Lets run the playbook](#lets-run-the-playbook)   
      - [Excercise 9](#excercise-9)   
         - [Adding New NTP server](#adding-new-ntp-server)   
   - [Section-6-2-4: Automating VLAN provisioning](#section-6-2-4-automating-vlan-provisioning)   
      - [Exercise 1](#exercise-1)   
         - [vlan configuration repository](#vlan-configuration-repository)   
      - [Exercise 2](#exercise-2)   
         - [Create handler to save the configuration](#create-handler-to-save-the-configuration)   
      - [Exercise 3](#exercise-3)   
         - [Create playbook tasks to configure the switch ports.](#create-playbook-tasks-to-configure-the-switch-ports)   
      - [Exercise 4](#exercise-4)   
         - [Create playbook to push host port configuration to the switch.](#create-playbook-to-push-host-port-configuration-to-the-switch)   
      - [Exercise 5](#exercise-5)   
         - [Lets run the playbook](#lets-run-the-playbook)   
      - [Exercise 6](#exercise-6)   
         - [Adding new Vlans](#adding-new-vlans)   
      - [Exercise 7](#exercise-7)   
         - [Removing  Vlans](#removing-vlans)   
   - [Section-6-2-5: Uplink port Configuration.](#section-6-2-5-uplink-port-configuration)   
      - [Exercise 1](#exercise-1)   
         - [Uplinks configuration repository](#uplinks-configuration-repository)   
      - [Exercise 2](#exercise-2)   
         - [Create handler to save the configuration](#create-handler-to-save-the-configuration)   
      - [Exercise 3](#exercise-3)   
         - [Create playbook tasks to configure the switch ports.](#create-playbook-tasks-to-configure-the-switch-ports)   
      - [Exercise 4](#exercise-4)   
         - [Create jinja2 template for the uplink configuration.](#create-jinja2-template-for-the-uplink-configuration)   
      - [Exercise 5](#exercise-5)   
         - [Create playbook to push host port configuration to the switch.](#create-playbook-to-push-host-port-configuration-to-the-switch)   
      - [Exercise 6](#exercise-6)   
         - [Run the playbook](#run-the-playbook)   
   - [Section-6-2-6: Hostport Configuration.](#section-6-2-6-hostport-configuration)   
      - [Exercise 1](#exercise-1)   
         - [Hostport configuration repository](#hostport-configuration-repository)   
      - [Exercise 2](#exercise-2)   
         - [Create handler to save the configuration](#create-handler-to-save-the-configuration)   
      - [Exercise 3](#exercise-3)   
         - [Create playbook tasks to configure the switch ports.](#create-playbook-tasks-to-configure-the-switch-ports)   
      - [Exercise 4](#exercise-4)   
         - [Create playbook to push host port configuration to the switch.](#create-playbook-to-push-host-port-configuration-to-the-switch)   
      - [Exercise 5](#exercise-5)   
         - [Lets run the playbook](#lets-run-the-playbook)   
      - [Excercise 6](#excercise-6)   
         - [Add new server port](#add-new-server-port)   
   - [Ansible Tips](#ansible-tips)   
      - [How to see which ansible modules are installed?](#how-to-see-which-ansible-modules-are-installed)   

<!-- /MDTOC -->


Ansible - Introduction
----
-----
Ansible is an open-source software platform for configuring ,managing and orchestrating compute and switching infrastructure. Ansible features a state-driven resource model that describes the desired state of computer systems and services.  Ansible is agentless and uses push model while puppet and chef are pull model using agents on the host.  Ansible uses YAML to configure playbooks.
Playbooks contains one or multiple plays and plays contains one or multiple tasks.  Tasks talks to ansible module. Ansible module are python scripts that configures the devices in the inventory  list.   Playbook is applied to host inventory file.  Inventory file contains list of all the devices that needs to be automated. Within a single play, you can specify which host this play applies to  from the list of all available host defined in the inventory file.

![ansible-0](/images/ansible-0.png)

![ansibel-1](/images/ansible-1.png)

**Hosts:** Remote machines Ansible manages.  
**Groups:** Group of hosts assigned to a pool that can be conveniently targeted and managed together.  
**Inventory:** File describing the Hosts and Groups in Ansible. what groups a host belongs to, the
properties those groups and hosts have.
**Modules:** Modules (also referred to as “task plugins” or “library plugins”) are the components that do the actual work in Ansible. They are what gets executed in each playbook task.  
**Playbooks:** A collection of plays which the Ansible Engine orchestrates, configures, administers, or deploys. These playbooks describe the policy to be executed to the host(s).  People refer to these playbooks as "design plans" which are designed to be human- readable and are developed in a basic text language called **YAML**.
**Play**: specify a list of tasks that are run in sequence across one or more hosts. Each task
can also run multiple times with a variable taking a different value. Playbooks are expressed in
YAML format
**Roles**: are a way to encapsulate common tasks and properties for reuse, if you find yourself writing
the same tasks in multiple playbooks, turn them into roles.

http://docs.ansible.com/ansible/list_of_network_modules.html


## Section-6-2-1 : Ansible Setup

### Exercise 1
#### Setting up the directory structure to host ansible files
1. Switch to `ATOM` Editor.
   1. Close all the files that are open in Atom.
   2. Right click on the `ansible` folder
   2. Select new folder
   3. name it `roles`
2. Repeat above and create following folders:
   1. templates
   2. group_vars
   3. host_vars
   4. files

![roles](/images/roles-801.png)
### Exercise 2
#### Creating Host Inventory File
Inventory file contains list of hosts that you want to manage from Ansible.  In our case, it will list of switches that we want to manage by ansible. These host/swiches can be organized in groups.

2. In the `ATOM` Editor
3. Right click on `ansible` folder and select `New File`.
3. Name it `hosts`
4. Add the the following lines to the file   

    ```
    [all:vars]
    ansible_connection = local

    [leafs]
    n9k-1

    ```

    ![ansibel-1](/images/hosts-11.png)

5. We are using  ansible connection as local rather than ssh, because the nxos ansible modules use nxapi to configure the switches rather than ssh.
6. Save the file `CMD + S`

You can read more about Inventory file here:  Inventory http://docs.ansible.com/ansible/intro_inventory.html

### Exercise 3
#### Creating credentials file
1. Under `ansible` folder , create a new file
2. name it `credentials.yml`
3. copy and paste the following.  Change the `username` and `password` to match your environment.

    ```
    ---
    creds:
        host: "{{ inventory_hostname }}"
        username:   admin
        password:   cisco123
        transport:  nxapi
    ```
4. Save the file `cmd + S`

To login into the switches, we need the `username` and `password`.  This file with combination with `provider` keyword in ansbile playbook will let ansible login into the devices.  

*What is provider keyword used for?*
**All core networking modules implement a provider argument, which is a collection of arguments used to define the characteristics of how to connect to the device.
The provider argument accepts keyword arguments and passes them through to the task to assign connection and authentication parameters.**

https://docs.ansible.com/ansible/intro_networking.html

### Exercise 4
#### Ping Test.  Make sure your Switch can reach internet.
You need to get yourself familiarize with nxos ansible modules.  Take a look at the ping module.  
http://docs.ansible.com/ansible/nxos_ping_module.html

1. Under `ansible` folder , create a new file
3. name it `ping.yml`
4. copy and past the following :

    ```
    ---
    - name: ping testing
      hosts: n9k-1
      connection: local
      gather_facts: no
      tasks:
        - name: obtain login credentials
          include_vars: credentials.yml

        # test reachability to 8.8.8.8 using mgmt vrf
        - nxos_ping: dest=8.8.8.8 vrf=management host={{ inventory_hostname }} provider="{{ creds }}"

        # Test reachability to a few different public IPs using mgmt vrf
        # if device has name lookups turned on, you can use names
        - nxos_ping: dest={{ item }} vrf=management  provider="{{ creds }}" host={{ inventory_hostname }}
          with_items:
            - 8.8.8.8
            - 4.4.4.4

    ```

9. `CMD+S` to save it

Note:  Environment variables can be set at the play or task level.
http://docs.ansible.com/ansible/faq.html  

##Exercise 5
#### Login in to your Ansible Container and Run the Playbook
1. Go to your terminal window and type the following:

    ```
    cd training
    source source.docker
    docker attach ansible
    ```
2. you should be inside your ansible docker container.
3. Since we not using dns for host resolution, we need to update the hosts file on the container.
    1. Inside the ansible container
       1. change the ip to match your environment. type
       2. `echo "172.16.123.135  n9k-1" >> /etc/hosts`
       3. cat /etc/hosts

        ![hosts](/images/ansible-300.png)

      4. `ping n9k-1`  make sure the host is able to resolve it.
      5. `ctrl + c` to break the ping.


3. Run Ping playbook.
    1. `ansible-playbook -i hosts ping.yml`

#####Ansible Help
To see all the nexus modules:
`ansible-doc -l | grep nxos`

To get snippet
`ansible-doc -s nxos_config`



----

## Section-6-2-2 : Simple Ansible Playbooks

### Exercise 1
#### Gather facts about the switch
Use nxos_facts module . Read more about it here  https://docs.ansible.com/ansible/nxos_facts_module.html

1. Right click on `ansible` folder and select `New File`
2. Name it `ex-show-facts.yml`
3. Copy and past the following :
   https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/ex-show-facts.yml

5. Save the file `CMD + S`
6. Create jinja2 template to store the data
   1. Right click on the `templates` folder and select `new file`. Name it `facts.j2`
   2. Copy and paste the following content to this file:
   3. https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/facts.j2

       ```
       DEVICE: {{ inventory_hostname }}

       Version:
       {{ kickstart | to_nice_json }}

       Platform:
       {{ platform | to_nice_json }}

       vlan Info:
       {{ vlan_list | to_nice_json }}

       Interfaces:
       {{ interfaces_list | to_nice_json }}

       Power Supply Info:
       {{ power_supply_info | to_nice_json }}

       Fan Info:
       {{ fan_info | to_nice_json }}
       ```

5. switch to ansible container and run the playbook
    1. `ansible-playbook -i hosts ex-show-facts.yml`
6. Switch to ATOM and go to `files` directory. View the json files for each of the switch.


### Exercise 2
#### Using the nxos_config module
Cisco NXOS configurations use a simple block indent file syntax for segmenting configuration into sections. nxos_config provides a way to build this sections of configurations. Read more here.
http://docs.ansible.com/ansible/nxos_config_module.html

1. Under `ansible` folder , create a new file. Right click on `ansibe` folder and select `New File`
2. name it `ex-config-module.yml`
3. copy and past the following :

    ```
    ---
    - name:  101-internet-access list
      hosts: n9k-1
      connection: local
      gather_facts: no
      tasks:
        - name: obtain login credentials
          include_vars: credentials.yml

        - nxos_config:
            lines:
              - 5 permit ip 10.132.243.9/32 any
              - 10 permit ip 10.241.16.91/32 any
              - 20 permit ip 140.84.159.66/32 any
              - 30 permit ip 140.84.54.128/26 any
              - 240 deny ip any any
            parents: ip access-list 101-internet-access
            before: ip access-list 101-internet-access
            match: exact
            provider: "{{ creds }}"

    ```
5. Save the file `CMD + S`
6. Switch to ansible container and run the playbook
    1. `ansible-playbook -i hosts ex-config-module.yml`
6. Verify the configuration on using Switch CLI. `show access-list`

----
Ansible Roles - Introduction
-----

Simply put, roles are a further level of abstraction that can be useful for organizing playbooks. As you add more and more functionality and flexibility to your playbooks, they can become unwieldy and difficult to maintain as a single file. Roles allow you to create very minimal playbooks that then look to a directory structure to determine the actual configuration steps they need to perform.

Organizing things into roles also allows you to reuse common configuration steps between different types of servers. This is already possible by "including" other files within a playbook, but with roles, these types of links between files are automatic based on a specific directory hierarchy.

In general, the idea behind roles is to allow you to define what a server is supposed to do, instead of having to specify the exact steps needed to get a server to act a certain way.

----
## Section-6-2-3: Base Configuration
Applying the base configuration to all switches in the inventory using jinja2 template.  Templates are good for mostly static configuration.  Since base configuration does not change that often, it is good idea to put them in a template.

### Exercise 1
#### Create ansible roles directory structure
Ansible galaxy provides a very easy way to create the directory structure to host your roles.

1. Switch to 'ansible container' terminal window
   2. `cd roles`
   3. type `ansible-galaxy init login`
   4. type `ansible-galaxy init baseconfig`
   5. type `ansible-galaxy init vlans`
   6. type `ansible-galaxy init uplinks`
   7. type `ansible-galaxy init hostports`

### Exercise 2
#### Configuring login roles
This role allows us to login into the switches.

1. Switch to `Atom` Editor
2. go to `login` folder under `roles`.
3. expand it
4. navigate to `vars` folder and open `main.yml`
5. type in the following:

    ```
    ---
    creds:
        host: "{{ inventory_hostname }}"
        transport: nxapi
        username:   admin
        password:   cisco123
    ```

    ![ansiblerole](/images/ansiblerole-1.png)

6. Save the file `Cmd+S`

### Exercise 3
#### Base configuration repository
This role allows us to do base configuration on all the switches. Base configuration includes things like ntp, snmp, alias etc that needs to be  provisioned to all switches.

1. Navigate to `ansible --> roles --> baseconfig --> vars`
2. Open up the `main.yml` file
3. copy and paste the following code.
4. https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/base-vars.yml
4. Save the file `Cmd+S`
5. Your file should look like this.

    ![ansiblerole](/images/ansible-301.png)


### Exercise 4

To configure base configuration, we are going to use nxos_template module. This module allows implementers to work with the device running-config. It provides a way to push a set of commands onto a network device by evaluating the current running-config and only pushing configuration commands that are not already configured. The config source can be a set of commands or a template.  This module uses jinja2 template.  

**What is Jinja2 Template?**

Jinja2 is a very popular and powerful Python-based template engine. Templates look very similar to normal text-based files except for the occasional variables or code that surrounds the special tags. These get evaluated and are mostly replaced by values at runtime, creating a text file, which is then copied to the destination host. The following are the two types of tags that Jinja2 templates accept:

`{{ }}` embeds variables inside a template and prints its value in the resulting file. This is the most common use of a template.
For example: `{{ hostname }}`

`{% %}` embeds statements of code inside a template, for example, for a loop, it embeds the if-else statements, which are evaluated at runtime but are not printed.

So where does jinja2 gets the values for its variables?  At the runtime of the playbook, Jinja2 has access to all the variable that are referenced inside that particular playbook.  So the values for the variables inside the jinja2 template is substituted with the values from the facts and variable obtained from the playbook at the run time.

**Steps to create the templates:**

1. Always start with the actual configuration file.
2. Identify the parameters that needs to be dynamically generated
3. create a template. Add variables as you see fit.
4. create the variable file.
5. create the playbook
6. run the playbook

---
#### Create jinja2 template for the base configuration.
1. Navigate to `ansible --> roles --> baseconfig --> templates`
2. Right click on the `templates` folder and select `New File`. Name it `basetemplate.j2`
3. Copy and paste the content from this link.  This is the jinja 2 template.

    https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/basetemplate.j2

4. Save the file `CMD + S`

### Exercise 5
#### Create handler to save the configuration
1. Navigate to `ansible --> roles --> baseconfig --> handlers`
2. Open up the `main.yml` file
3. Copy and paste the following:

    ```
    ---
    # handlers file for baseconfig
    - name: Save Config
      nxos_config:
        provider: "{{ creds }}"
        lines:
          - 'copy run start'
    ```
4. Save the file `CMD + S`

### Exercise 6
#### Create playbook tasks for base configuration.
1. Navigate to `ansible --> roles --> baseconfig --> tasks`
2. Open up the `main.yml` file
3. Copy and paste the following:

    ```
    ---
    # tasks file for baseconfig
    #renders the jinja2 template

    - name: configure base template
      nxos_template:
        provider: "{{ creds }}"
        src: basetemplate.j2
        backup: yes
      notify:
        - Save Config
    ```
4. Note we are specifying the jinja2 template to be used to build this baseconfig file.  Also we are going to backup current running configuration prior to making any changes.  Finally we will save the configuration.
5. Save the file `CMD + S`


### Exercise 7
#### Create playbook to push this base configuration to the switch.
1. Navigate to `ansible` folder
2. Right click and select `New File`. Name it `deploy-baseconfig.yml`
3. Copy and paste the following:

    ```
    ---
    - hosts: all
      connection: local
      strategy: free
      roles:
        - { role: login, tags: [ 'login' ] }
        - { role: baseconfig, tags: [ 'login', 'base'] }
    ```
4. Save the file `CMD + S`

### Exercise 8
#### Lets run the playbook
1. Switch to the `ansible container` terminal window.
2. Run the playbook
    1. `ansible-playbook -i hosts deploy-baseconfig.yml`
3. Login into your switch.
4. Verify that ansible has made those configuration.

### Excercise 9
#### Adding New NTP server
Lets say Server guys added a new `NTP server` which has ip of `192.200.0.2`. You want to update all your switches in your DC to reflect this change.  Today, you might be logging into all the switches and manually typing this in.  With ansible, we to go one file (the variables file) and make this modification.  Then we run the playbook again.  Within secs , it will update your entire DC switches with the new ntp server information.  Note, ansible is idempotent, therefore it will not change anything else except that one small change.  Therefore this is not be disruptive change.

1. Switch to `ATOM` editor
    2. Navigate to `ansible --> roles --> baseconfig --> vars`
    2. Open up the `main.yml` file
    3. add this a line to your `NTP server list`
    4. `- 192.200.0.2`
2. Switch to the ansible terminal window
    1. Run the playbook again
    2. `ansible-playbook -i hosts deploy-baseconfig.yml`
3. Login into your switch.
    1. Verify that ansible has made those configuration.
4. Switch to `ATOM` editor.
    1. Navigate to `ansible --> roles --> baseconfig-->backup`
    2. you should see your backupfiles


----
## Section-6-2-4: Automating VLAN provisioning
**Problem Statement**
Users perform manual Day-2 operations to configure network elements repeatedly for the purpose of on-boarding new workloads. One common category of Day-2 configuration activity is performing routine VLAN related operations:
1. Check if VLAN exists
2. Change names and descriptions of VLANs
3. Configure a VLAN

It would be beneficial to automate this repetitive configuration with a goal of reducing time and effort needed to complete the configurations, while reducing the probability of errors.

**Solution  - Ansible automation**
Ensure following vlans are configured on all the switches.

| Vlan id |	vlan name |
|---------|-----------|
|10	| web|
| 20 |	app|
|30	|db|
|40	|misc
|99	|native-vlan



### Exercise 1
#### vlan configuration repository
This role allows us to Configure and maintain vlans on all the switches.
Lets update the vars directory with all our variables.

1. Navigate to `ansible --> roles --> vlans --> vars`
2. Open `main.yml`
3. copy and paste the following code.

    ```
    vlans:
        - { vlan_id: 10, name: web, state: present }
        - { vlan_id: 20, name: app, state: present }
        - { vlan_id: 30, name: db, state: present }
        - { vlan_id: 40, name: misc, state: present }
        - { vlan_id: 99, name: native_vlan, state: present }
    ```
4. Save the file `Cmd+S`

### Exercise 2
#### Create handler to save the configuration
1. Navigate to `ansible --> roles --> vlans --> handlers`
2. Open up the `main.yml` file
3. Copy and paste the following

    ```
    ---
    # handlers file for baseconfig
    - name: Save Config
      nxos_config:
        provider: "{{ creds }}"
        lines:
          - 'copy run start'
    ```
4. Save the file `CMD + S`

### Exercise 3
#### Create playbook tasks to configure the switch ports.
This time, we are going to use `nxos_vlan` module to make these changes.  Read more about it here
https://docs.ansible.com/ansible/nxos_vlan_module.html

1. Navigate to `ansible --> roles --> vlans --> tasks`
2. Open up the `main.yml` file
3. Copy and paste the following:

    ```
    ---
    -   name: configuring the vlans
        nxos_vlan: vlan_id={{ item.vlan_id }}  name={{ item.name }} state={{ item.state }}  provider="{{ creds }}"
        with_items: "{{vlans}}"
        notify:
          - Save Config
    ```
5. Save the file `CMD + S`


### Exercise 4
#### Create playbook to push host port configuration to the switch.
1. Navigate to `ansible` folder
2. Right click and select `New File`. Name it `deploy-vlans.yml`
3. Copy and paste the following:

    ```
    ---
    - hosts: all
      connection: local
      strategy: free
      roles:
        - { role: login, tags: [ 'login' ] }
        - { role: vlans, tags: [ 'login', 'vlans'] }
    ```
4. Save the file `CMD + S`

### Exercise 5
#### Lets run the playbook
1. Switch to the `ansible container` terminal window.
2. Run the playbook
    1. `ansible-playbook -i hosts deploy-vlans.yml`
3. Login into your switch.
4. Verify that ansible has made those configuration.



**Homework**

### Exercise 6
#### Adding new Vlans
Modify the the roles to add vlan 800 to all the switches.

### Exercise 7
#### Removing  Vlans

Modify the playbook so that vlan 40 is removed from all the switches.


----


## Section-6-2-5: Uplink port Configuration.
In this section, we will be configuring uplink ports. This will include changing the interface to L3 mode and then assigning ip to it.  We are going to use jinja2 template for this.

### Exercise 1
#### Uplinks configuration repository
This role allows us to create configuration for uplinks on the switches. Since the config is per switch basis, we need to hold the variables in the `host_vars`. We need to create a folder for each host in `host_vars` folder.  Ansible will search this folder to look for the variables.

1. Navigate to `ansible --> hosts_vars`
2. Right click and select `New Folder`. Name it `n9k-1.yml`
3. copy and paste the following code.

    ```
    ---
    uplink_interface:
      - {int: "ethernet1/1", ip: 192.168.1.2/30}
      - {int: "ethernet1/2", ip: 192.168.2.2/30}

    ```
4. Save the file `Cmd+S`

### Exercise 2
#### Create handler to save the configuration
1. Navigate to `ansible --> roles --> uplinks --> handlers`
2. Open up the `main.yml` file
3. Copy and paste the following

    ```
    ---
    # handlers file for baseconfig
    - name: Save Config
      nxos_config:
        provider: "{{ creds }}"
        lines:
          - 'copy run start'
    ```
4. Save the file `CMD + S`

### Exercise 3
#### Create playbook tasks to configure the switch ports.
This time, we are going to use `nxos_template`  module to make these changes.  Read more about it here

https://docs.ansible.com/ansible/nxos_template_module.html

1. Navigate to `ansible --> roles --> uplinks --> tasks`
2. Open up the `main.yml` file
3. Copy and paste the following:

    ```
    ---
    # tasks file for uplinks
    - name: configure uplinks template
      nxos_template:
        provider: "{{ creds }}"
        src: interface.j2
        backup: yes
      notify:
        - Save Config

    ```
5. Save the file `CMD + S`

### Exercise 4
#### Create jinja2 template for the uplink configuration.
1. Navigate to `ansible --> roles --> uplinks --> templates`
2. Right click on the `templates` folder and select `New File`. Name it `interface.j2`
3. Copy and paste the content from this link.  This is the jinja 2 template.

    https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/basetemplate.j2

4. Save the file `CMD + S`


### Exercise 5
#### Create playbook to push host port configuration to the switch.
1. Navigate to `ansible` folder
2. Right click and select `New File`. Name it `deploy-uplinks.yml`
3. Copy and paste the following:

    ```
    ---
    - hosts: all
      connection: local
      strategy: free
      roles:
        - { role: login, tags: [ 'login' ] }
        - { role: uplinks, tags: [ 'login', 'uplinks'] }
    ```
4. Save the file `CMD + S`

### Exercise 6
#### Run the playbook
1. Switch to the `ansible container` terminal window.
2. Run the playbook
    1. `ansible-playbook -i hosts deploy-uplinks.yml`
3. Login into your switch.
4. Verify that ansible has made those configuration.


## Section-6-2-6: Hostport Configuration.
Another common Day 2 operations tasks is to configure hosts/server ports.  We want to have consistent configuration on all the server facing ports.

### Exercise 1
#### Hostport configuration repository
We have created a role to hold all the hostport configuration data.  Host port configuration includes things like `switch port access mode`,  etc that needs to be  provisioned to per switches basis.

Since the config is per switch basis, we need to hold the variables in the `host_vars`. We need create folder for each host in this folder.  Ansible will search this folder to look for the variables.

1. Navigate to `ansible --> hosts_vars`
2. Right click and select `New Folder`. Name it `n9k-1.yml`
3. copy and paste the following code.

    ```
    ---
    hostports:
       - {int: "ethernet1/5", des: ESXI-1}
       - {int: "ethernet1/6", des: ESXI-2}
       - {int: "ethernet1/7", des: Openstack-nova-server-1}
       - {int: "ethernet1/8", des: Openstack-nova-server-2}
    ```
4. Save the file `Cmd+S`

### Exercise 2
#### Create handler to save the configuration
1. Navigate to `ansible --> roles --> hostports --> handlers`
2. Open up the `main.yml` file
3. Copy and paste the following

    ```
    ---
    # handlers file for baseconfig
    - name: Save Config
      nxos_config:
        provider: "{{ creds }}"
        lines:
          - 'copy run start'
    ```
4. Save the file `CMD + S`

### Exercise 3
#### Create playbook tasks to configure the switch ports.
This time, we are going to use `nxos_interface`  and `nxos_switchport` module to make these changes.  Read more about it here

https://docs.ansible.com/ansible/nxos_interface_module.html
https://docs.ansible.com/ansible/nxos_switchport_module.html

1. Navigate to `ansible --> roles --> hostports --> tasks`
2. Open up the `main.yml` file
3. Copy and paste the following:

    ```
    ---

    # tasks file for hostports
    - name: Ensure that port is in layer 2 mode
      nxos_interface:
        provider: "{{ creds }}"
        interface: Ethernet1/5
        description: 'Configured by Ansible'
        mode: layer2

    - nxos_switchport:
        provider: "{{ creds }}"
        interface: eth1/5
        mode: access
        access_vlan: 20
      notify:
        - Save Config
    ```
5. Save the file `CMD + S`


### Exercise 4
#### Create playbook to push host port configuration to the switch.
1. Navigate to `ansible` folder
2. Right click and select `New File`. Name it `deploy-hostports.yml`
3. Copy and paste the following:

    ```
    ---
    - hosts: all
      connection: local
      strategy: free
      roles:
        - { role: login, tags: [ 'login' ] }
        - { role: hostports, tags: [ 'login', 'hostports'] }
    ```
4. Save the file `CMD + S`

### Exercise 5
#### Lets run the playbook
1. Switch to the `ansible container` terminal window.
2. Run the playbook
    1. `ansible-playbook -i hosts deploy-hostports.yml`
3. Login into your switch.
4. Verify that ansible has made those configuration.

### Excercise 6
#### Add new server port
configure a new server port on n9k-1 port eth1/8

## Ansible Tips
### How to see which ansible modules are installed?
ansible-doc --list | egrep ^'nxos'
