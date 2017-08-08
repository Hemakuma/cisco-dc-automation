# Installing DevOps tools on the switch

## Enabling Guestshell

[Guestshell doc](https://www.cisco.com/c/en/us/td/docs/switches/datacenter/nexus9000/sw/7-x/programmability/guide/b_Cisco_Nexus_9000_Series_NX-OS_Programmability_Guide_7x/Guest_Shell.html)



It is automatically enabled. `show virtual-service list`.  You can access it  by typing `guestshell` on the switch prompt.

show guestshell

### Increase the filesystem of the guestshell
By default it has 200M, we need to increase to 1000M

Exist guestshell; From the switch command, resize the filesystem
```
guestshell resize rootfs 1000  
Guestshell reboot

```

Login to the guestshell  `guestshell`
Check the size

`df -k /`

### Enable dns on the guestshell

Inside guestshell ; change vrf to management

`sudo chvrf management `

add /etc/resolv.conf file

`sudo vi /etc/resolv.conf`

Add your name servers

```
domain cisco.com
search cisco.com
nameserver 8.8.8.8

```



## Installing git on the Guestshell

In the management vrf

'yum install git'

## Installing Ansible on Guestshell

```
yum install epel-release
yum install ansible

```

## Running Ansible playbook from Guestshell

update your hosts file

`vi /etc/hosts `

to include your switch ip. you should be able to ping your leafs from your guestshell.


Create a playbook directory and put your playbooks here.

or u can use my sample playbooks

'git clone https://github.com/Hemakuma/techdaysf.git'

change the host file to match your environment

### Run your playbook


chvrf management

ansible-playbook -i dev vlan-example-1.yml

## Running Ansible Playbook automatically using EEM
