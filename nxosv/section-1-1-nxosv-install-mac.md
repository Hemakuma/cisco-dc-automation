


#Introduction
You can deploy a virtual nexus 9K switch on your laptop.  This way , you  have access to N9K all the time to learn programmability at your own pace.

![nxosv](/images/nxosv-v-10.png)
Following NXOS features are available on the NXOSv switch.


#Prerequisite
###VM Fusion
Make sure you have VM fusion  installed on your laptop.  You can download 30day trial version from here
http://www.vmware.com/products/fusion.html

If your laptop is running Windows 7, refer to the NX-OSv in VMWare Player on Windows 7/10 document for setup instructions.
Download OVA file from the box link provided.

#NXOSv on Vmware Fusion
If you installed VM Fusion with default settings, all your VMs will be located under Documents/Virtual Machines  folder.
![nxosv](/images/nxosv-v-1.png)  
##Steps To install NXOSv
1. Open up your VM fusion  (`Command+space` and type `Vmware Fusion`)
2. Create a New VM; Click on `Add`  button and select More Options...
![nxosv](/images/nxosv-v-2.png)  
3. Select `Import an existing virtual machine`
![nxosv](/images/nxosv-v-3.png)  
4. Click on `Choose File` and choose the file you downloaded from box  "nxosv-final.7.0.3.I2.2e.ova"
![nxosv](/images/nxosv-v-4.png)  
5. Select the location  of your VM.  You can leave it to the default settings.
![nxosv](/images/nxosv-v-5.png)  
6. It should install this VM now.  Click on `Customize Setting`.
![nxosv](/images/nxosv-v-6.png)   
7. Select the first network adaptor for management interface connectivity.
![nxosv](/images/nxosv-v-7.png)
8. Enable NAT on this interface  
![nxosv](/images/nxosv-v-8.png)
9. Disable all the other network adaptors (adaptor 2-6)
![nxosv](/images/nxosv-v-9.png)   
10. Close the window.  Do not start the VM as yet.
11. Shutdown VM Fusion
12. Go to the dir or location where your virtual machine is created  `cd  /Users/<userid>/Documents/Virtual Machines.localized/nxosv-final.7.0.3.I2.2e.vmwarevm`
13. Type `ls`
    if you see locked vmx file like this nxosv-final.7.0.3.I2.2.vmx.lck  ; Make sure you have closed the Vmware Fusion window before editing the vmx file.
14. edit the vmx file
        `vim nxosv-final.7.0.3.I2.2e.vmx`
15. go to the end of the file..uses arrow keys
16. press "i" to insert
17. Add the following lines at the end of this file:

    ```
    serial0.present = "TRUE"
    serial0.fileType = "network"
    serial0.fileName = "telnet://127.0.0.1:9002"
    serial0.startConnected = "TRUE"
    serial0.yieldOnMsrRead = "TRUE"
    ```
    
18. Start VMware Fusion
19. Start NXOSv Machine.
    After few sec, you should see the following screen.  Wait until you see "Leaving grub land".  Your NXOS virtual switch is now up and running. To access it, you need to do initial configuration via serial connection.  (Not through the VM Console). ***You will not be able type anything in the KVM console.***
    ![nxosv](/images/nxosv-v-11.png)  


## Connecting to you NXOSv switch and configuring it.
1. open up a terminal window
2. type `telnet 127.0.0.1 9002`
3. This gives you console access to your nxosv switch.
4. You should see the initial POAP boot screen. Set you admin password and do the initial configuration. (admin/cisco)  
 ![nxosv](/images/nxosv-v-11.png)  
5. Provision the management interface.
    ```
    config t
    inter mgmt0
    ip addr dhcp
    ```
5. Set the boot variable
    `boot nxos bootflash:xosv-final.7.0.3.I2.2e.bin`

6. Enable feature features
    ```
    feature nxapi
    feature scp-server
    ```
6. Save the configuraiton
    `copy run start`
7. checkpoint your configuration
    `checkpoint file bootflash:startconfig`

7. Find out the ip address of the mgmt interfaces.
    `show ip int brief vrf mgmt`
    note down this ip.  now you can ssh to the box via ssh

*Tip*
If you want to rollback your configuration, use
`rollback running-config file bootflash:startconfig`

##Connect to the switch using ssh and http
From a new terminal window, ssh to the ip address of your NXOSv switch.  There is no port forwarding configured but the switch ip address is visible on the host.
`ssh  admin@172.16.123.134`
Set the default gateway. (You can get the default gateway ip from your mac. type ifconfig and look for the ip address of vmnet8.)
```
switch(config)# vrf context management
switch(config-vrf)# ip route 0/0 172.16.123.1
```
You can access the sandbox by going to the  following address:    http://172.16.123.134

##Troubleshooting
If you get into loader mode, use these commands to recover.

On the VMware Fusion Console window
type `dir`
note down the image name

`loader> boot <put the image name here>`
