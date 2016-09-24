REST Client
---
**Table of Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [REST Client](#rest-client)   
   - [Working With Rest Client (Postman)](#working-with-rest-client-postman)   
      - [Exercise-1](#exercise-1)   
         - [Installing POSTMAN](#installing-postman)   
      - [Exercise-2](#exercise-2)   
         - [Creating your First REST Request](#creating-your-first-rest-request)   
      - [Exercise-3](#exercise-3)   
         - [Viewing the current configuration of a physical interface (Interface status)](#viewing-the-current-configuration-of-a-physical-interface-interface-status)   
      - [Exercise-4](#exercise-4)   
         - [Viewing the Vlan configuration](#viewing-the-vlan-configuration)   
      - [Exercise-5](#exercise-5)   
         - [Configure Vlan 300 using REST Client](#configure-vlan-300-using-rest-client)   
      - [Exercise-6](#exercise-6)   
         - [Modifying Objects Configuring Physical interfaces](#modifying-objects-configuring-physical-interfaces)   

<!-- /MDTOC -->




## Working With Rest Client (Postman)
Postman helps you be extremely efficient while working with APIs. With Postman, you can construct requests quickly, save them for later use and analyze the responses sent by the API.

We will be using `Postman`REST client  to sent REST API calls to the nexus switch to change the MO.  For this, you will need have google chrome browser installed.  if you don't have chrome browser, please installed it.

### Exercise-1
#### Installing POSTMAN
1. Download it and install Postman REST client for chrome browser.  
https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=en
2. Open up Postman
    1. `cmd + space`
    2. type `postman`
3. For windows users , on your browser type  `chrome://apps` and then click on `Postman`
In next exercise we will create a REST request.

### Exercise-2
#### Creating your First REST Request
HTTP has 2 types of message, **Request** and **Response**. The request message has methods. Commonly used methods are **POST, GET, DELETE**

Lets create a REST request message to login into the nexus switch.  This will be a POST method.

1. Create a new Postman Collection.
2. Name it `NXOS-API`

    ![postman](/images/postman-1.png)  

3. Click on `Create`
4. Now Create a new API request to login the NXOS switch.
5. Fill the form as the follows..

    ![postman](/images/postman-2.png)

6. method = `POST`
7. url field type `http://<yourswitch ip>/api/aaaLogin.json`
8. select the `Body` tab.
7. make sure to select `JSON` for the payload. Enter the following json code. Adjust the name and password accordingly.

    ```
    {
        "aaaUser": {
            "attributes": {
                "name":"admin",
                "pwd":"cisco123"
            }
        }
    }
    ```

8. Click on `Save arrow down` and select `save as`
9. Name it `NXOS-Login`
10. Select `NXOS-API` collection.
10. Click on `Save`
10. Get you authentication token by clicking on the `Send` Button at the right side of the url. You should see your token in the RESPONSE Window

*Each time you want to configure the switch using REST client, you need to get authentication token*

### Exercise-3
#### Viewing the current configuration of a physical interface (Interface status)
Create the json rest request to view the current configuration on eth1/1.  use GET request.

1. Get the authentication token. (see exercise 2)
2. Using visore, locate the interface eth1/1 in the object store and note down the dn.  (you already done this in exercise 1)
`dn = sys/intf/phys-[eth1/1]`
3. Make a copy of the login request. From `Save` menu , select 'Save As'
4. Name it `Get-Interface-Status`
5. Select `NXOS-API` collection.
6. Change the request to be of `GET` type
7. Change the url will be `http://switch-ip/api/mo/sys/intf/phys-[eth1/1].json`
8. Click on `Save` to save the changes.
9. Click on `Send`   Send button.
10. Take note of the adminSt, accessVlan and descr from the Response window.  We are going to modify these attributes of this object in the next exercise.

    ![postman](/images/postman-3.png)  

### Exercise-4
#### Viewing the Vlan configuration
1. Get the authentication token (send login-noxs request)
3. Create a new request called `get-vlan-infor` using the same steps as you did in exercise 3 above. Basically making a copy of the `Get-Interface-Status` request using `save as` command.
4. Use `GET` request with the following url   `http://<yourswitchip>/api/mo/sys/bd/bd-[vlan-300].json`
5. You don't need anything in the body part of the request.
6. Save the Request by clicking the `Save` button again
6. Send the request to the switch by pressing `Send` button.  

You should not see vlan 300 in the RESPONSE window.  If you do see it, delete by changing the GET request to DELETE request and send it to the switch.  Then switch it back to GET request to verify that vlan 300 does not exist on the switch.


### Exercise-5
#### Configure Vlan 300 using REST Client
1. Using visore, locate the dn of the vlan. Take this time to learn how to use visore.  Hint: look for class "BDEntity". Use the mind map above to help u out too. Remember since we have not created the vlan 300 as yet, that object will not be in data store.  You will need the class name to create the object of that class. In this case, we will be creating a child object of class BDEntity.  This child class is call L2BD.
2. Switch to Postman window
3. Get the authentication token (send login-noxs request)
4. Create a new request called `config-vlan` in the `NXOS-API` folder. Use the same method. Do `save as` on a exiting request.
5. In the URL field , select `POST` and then type your object url.  It should look something like this `http://<switchip>/api/mo/sys/bd.json`
6. Change the request type to be `POST`
7. In the BODY of the POST Request type following JSON code.
https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/config-vlan.json
7. Click `Save` to save the request.
8. Send a POST request by pressing on the `Send` button.
8. Verify the vlan is configured.
    9. Select  `get-vlan-infor` request again from your collection. Then press `SEND`.  Verify that this time around you got some data about vlan 300

### Exercise-6
#### Modifying Objects Configuring Physical interfaces
Modify the physical interface eth1/1 as follows:
```
description: configured by REST Client
access vlan : vlan 300
port type = layer 2
admin state = shutdown
```
*you are going to use the same url as in example 2 but this time you going to use POST request to change the object.*

1. Using visore, locate the interface `eth1/1` in the object store and note down the dn. You have done this in exercise 5.
2. To modify this object, your url will be `http://<switch ip:/api/mo/<dn>.json`. subsitute dn from step # 1.
3. Switch to Postman window.
4. Get the authentication token (send `noxs-login` request)
5. Create a New Request. Name it `Config-Phy-Int`.  Add it to the `NXOS-API` collection.
6. In the URL field , select `POST` and then type your object url (see picture below).  It should look something like this `http://<switchip:>/api/mo/sys/intf/phys-[eth1/1].json`
7. In the BODY of the POST Request type following JSON code.
https://github.com/Hemakuma/cisco-dc-automation/blob/master/configs/config-phy-int.json
8. Save the request by click the `Save` button.

    ![postman](/images/postman-4-0.png)

9. Send a `POST` request to the switch by pressing on the `Send` button.

    ![postman](/images/postman-4.png)

10. Now click on your `Get-interface-status`  request in the Postman window.  `Send`  this GET request  to the the switch and compare the results from exercise 2.

    ![postman](/images/postman-5.png)  

11. Switch to your ssh terminal to the switch.  `do show interface ethernet 1/1`.
