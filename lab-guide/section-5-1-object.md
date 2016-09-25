
NXAPI (object-based REST Client)
---
**Table of Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [NXAPI (object-based REST Client)](#nxapi-object-based-rest-client)   
   - [Introduction](#introduction)   
      - [Exercise 1](#exercise-1)   
         - [Browsing the object store](#browsing-the-object-store)   

<!-- /MDTOC -->

Introduction
---
Camden release incorporated a RESTful API, with an underlaying object based data structure. It is organized in the Parent/Child hierarchy  MIT.
The DME (data management engine) holds the repository for the state of the managed system in the Management Information Tree (MIT). The MIT manages and maintains the whole hierarchical tree of objects on the switch, with each object representing the configuration, operational status, accompanying statistics and associated faults for a switch function. The MIT is the single source of truth for the configuration and operational status of NX-OS features and elements. Object instances, also referred to as Managed Object (MOs), are stored in the MIT in a hierarchical tree.  The MO database is organized in such a way that there is a parent-child hierarchy, meaning there is a root node followed by a hierarchical group of children.
Every object in the object store will have a DN (distinguished name).

The **distinguished** name enables you to unambiguously identify the target object. The DN specifies the exact managed object on which the API call is operating.
The **relative** name identifies an object within the context of its parent object. The distinguished name is composed of a sequence of relative names

![nxrest-1](/images/nxrest-1.png)

Take a look at this document if you unfamiliar with object model.  https://opennxos.cisco.com/public/api/nxapi-rest/

Following mind map shows some of the objects in the MIT. Use visore to find out more. Here is very high level of the tree structure. Keep this as for reference when u start programing using object model.

![nxrest-1](/images/nxrest-2.png)

Object Store

![nxrest-1](/images/nxrest-3.png)

### Exercise 1
#### Browsing the object store
*Make sure your switch has following feature enabled. `feature nxapi`.*  

Objects are stored in object store inside the switch. Data management engine (DME) is used to manipulate object inside this object store. There is a northbound API that exposures switch features externally and also there is a southbound API that interacts with the physical hardware.  Everything inside the switch is modeled inside this object store in a MIT. You can look at the objects stored in the data store and get their attributes using a inbuilt app on the switch called visore.

1. Open up Chrome browser and  in the address field, point it to the url of your NXOSv switch.
    1. `http://<switchip>/visore.html`
    2. Authenticate  (admin/cisco123)
2. You will see screen like below. Everything in the `green column` are the `attributes` of this object.  Anything in the `yellow` are the `values of the those attributes` and the `pink` row is the `class of this object`.  

    ![nxrest-1](/images/nxrest-4.png)  

4. Object are in a Tree called MIT. The top level (Root of MIT) is called System class (topSystem) and the dn for this Class (dn=sys).
5. Click on the `green right arrow`, in the dn row to see the child object of this class.  (basically you are navigating the MIT to see varies object in the class)
6. Navigate to the interfaceEntity class (you can search for it ..use cmd + F).  This class holds all the objects related to the interfaces , both physical and virtual interfaces.
![nxrest-1](/images/nxrest-5.png)   
7. Click on the `right green arrow` in the dn row.  This will take you all the interface objects (physical and virtual)
8. Navigate to eth1/1 (again within the page, you can use cmd+F to search).  Note, anything in [xxx] is the instance of the class , therefore you can modify its attributes.  
![nxrest-1](/images/nxrest-6.png)

9. Note down the dn for this object.  We will use this dn to configure port eth1/1 from the REST Client. This will be the full url  http://<switch ip>/api/mo/<dn>.json that you need to point to in order to modify any attributes of this object.  
![nxrest-1](/images/nxrest-100.png)  
10. Save this URL for the next exercise.
11. Once you get the feel of the classes, know the names, you can query the classes directly.  In the Query field type `l1PhysIf` and press `search`. You will see all the object related to this class. In this case, you will see all the interfaces.
12. (optional) Now Repeat all of the above steps to find the dn to configure a new vlan.  Hint, vlans are also called `l2bd`.

----
**Common classes**  

```
InterfaceEntity <-- to manupliate all the physical interfaces  
l1PhysIf  
BDEntity <-- for the vlans  
BGPEntity <-- to configure bgp  
ospfEntity  
aclEntity  
CdpEntity  
dhcpEntity  
dnsEntity  
L3Inst  

```
