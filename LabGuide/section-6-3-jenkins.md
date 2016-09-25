Jenkins
---
**Table of  Contents**
<!-- MDTOC maxdepth:6 firsth1:1 numbering:0 flatten:0 bullets:1 updateOnSave:1 -->

   - [Jenkins](#jenkins)   
   - [Introduction](#introduction)   
      - [Exercise-1](#exercise-1)   
         - [Update the hosts file](#update-the-hosts-file)   
      - [Exercise-2](#exercise-2)   
         - [Getting Jenkins login password](#getting-jenkins-login-password)   
      - [Exercise-3](#exercise-3)   
         - [Login into Jenkins Server](#login-into-jenkins-server)   
      - [Exercise-4](#exercise-4)   
         - [Installing jenkins plugins](#installing-jenkins-plugins)   
      - [Exercise-5](#exercise-5)   
         - [Creating a job](#creating-a-job)   
      - [Exercise-6](#exercise-6)   
         - [Configure the webhook on the gitlab.](#configure-the-webhook-on-the-gitlab)   
      - [Exercise-6](#exercise-6)   
         - [Test the automate deployment](#test-the-automate-deployment)   
      - [Troubleshooting](#troubleshooting)   

<!-- /MDTOC -->

Introduction
-----
Jenkins is an open source tool that is used for scheduling and running automated tests and deployment.  It is essentially the friendly butler of automation who will handle all of your automation tasks for you once you tell him to do so and he only needs to be told once.

In the Server world, Jenkins can be used to perform the typical build server work, such as doing continuous/official/nightly builds, run tests, or perform some repetitive batch tasks.

In the network world, we can use it to automatically deploy network configuration to network devices. This is little unknown application of Jenkins but we will be explore little bit deeper how we can use Jenkins to help with network automation.

### Exercise-1
#### Update the hosts file
1. Open another terminal window and navigate to `training` folder
2. source `source source.docker`
3. Find out the ip address of your `nxosv` switch.  you can login into the switch and type `show ip int brief vrf  management`.  Note down the ip.
4. Update the `/etc/hosts` files on the jenkins server  since we are not using dns. This will allow jenkins server to be able to resolve the names of our switches that are referenced inside the ansible playbook
5. type

	`docker exec -it jenkins-ansible /bin/sh -c "echo 172.16.123.135 n9k-1 > /etc/hosts"`

6. Repeat this for all the switches you have.

### Exercise-2
#### Getting Jenkins login password
1. Get the admin password
	3. type
   4. `docker exec -it jenkins-ansible  cat /var/jenkins_home/secrets/initialAdminPassword `
	5. copy the password
	5. type `docker-machine ip default` and note down the ip address of the docker host.  

### Exercise-3
#### Login into Jenkins Server
1. Open up a chrome browser and login in. Use the ip you got in exercise 2.
2. `http://<ip>:8080`
3. type in the admin password that you copied previously.

	![jenkins](/images/jenkins-12.png)
4. Click `Select plugins to install`. and select none (we will install plugins manually)

	![jenkins](/images/jenkins-200.png)

5. Select `none` (we will install plugins manually)

	![jenkins](/images/jenkins-13.png)

6. Click on `Install`
7. Create a First user account (yourid/cisco123)

	![jenkins](/images/jenkins-201.png)

8. Click `Save and Finish`
9. Click on `Start using Jenkins`


### Exercise-4
#### Installing jenkins plugins

We need gitlab plugin for now.  Lets install them.

1. Go to the dashboard
2. on the left side, click on `Manage Jenkins`
3. On the right side, click on `Manage Plugins`

	![jenkins](/images/jenkins-15.png)
3. Then select plugins
	1. Click on the `Available` plugins
	2. on the `Filter` , type git
	3. Find the following plugin and check the select box.
		1. gitlab plugin

	![jenkins](/images/jenkins-16.png)

5. Click on `Install without restart`.  Note it will automatically install all other required plugins.

	![jenkins](/images/jenkins-17.png)

6. Once install, you need to restart your server. Check the `restart box` at the end of the screen or
7. On the url of the browser type `http://<ip>:8080/restart`
8. It should take couple of mins to restart.

### Exercise-5
#### Creating a jenkins job
Jenkins Job Builder takes simple descriptions of Jenkins jobs in YAML or JSON format and uses them to configure Jenkins. You can keep your job descriptions in human readable text format in a version control system to make changes and auditing easier.

1. Click on `Create new Job`

 	![jenkins](/images/jenkins-jobs-1.png)
2. Name the job `deploy-prod`
3. use `free style project`

	Freestyle project: This provides the ability to create a completely custom job that can behave in any way we choose.

	![jenkins](/images/jenkins-jobs-2.png)

4. Switch to your `gitlab` tab on the browser.  
	1. Click on the `Project`
	2. copy the url for the `ansible` project.

	![jenkins](/images/gitlab-305.png)

5. Switch back to the `jenkins` tab on your browser.  Fill in the `Source Code Management Form` as shown in the screen below. Make sure you have set to `branch to build`  to `master`  branch.

	![jenkins](/images/jenkins-304.png)

6. Make sure to create a `Credentials` for your git login.  Click on `add` and select `jenkins`

   ![jenkins](/images/jenkins-300.png)

7.  Make sure you select the newly created credentials for your repository.  Credentials section should show your new credential.

   ![jenkins](/images/jenkins-301.png)

8. Click on the `Build Trigger` tab and fill in the form as shown below.

	![jenkins](/images/jenkins-401.png)

9. Click on the `Build` tab. Add a `Execute Shell` task. Run a shell script to run the ansible playbook.

   ![jenkins](/images/jenkins-302.png)

	![jenkins](/images/jenkins-402.png)

	Make sure to change the playbook name to meet your requirement

	```
	#!/bin/bash
	echo "Running Ansible against: $FQDN"
	# http://www.ansibleworks.com/docs/gettingstarted.html#a-note-about-host-key-checking
	export ANSIBLE_HOST_KEY_CHECKING=False
	pushd /var/jenkins_home/workspace/deploy-prod/
	    ansible-playbook -i hosts deploy-baseconfig.yml
	popd

	```
Note: jenkins will fetch the repo and store the contents  under `/var/jenkins_home/workspace/deploy-prod/` directory.
10. Repeat step 9 above to play rest of the ansible playbooks.
   1. click on the `Add build step` again.
   2. Select `Execute Shell`
   2. copy paste the same content as above but this time change th ansible playbook to point to `deploy-hostports`
   3. Repeat for `deploy-vlans.yml` and `deploy-uplinks.yml`
10. Click on `Save`
11. Click on `Build Now`

 ![jenkins](/images/jenkins-jobs-10.png)

12. Take a look at the console output.  Click on the `build #` and select `Console Output` in the next screen.

   ![jenkins](/images/jenkins-jobs-9.png)

13. Copy the CI integration url so that we can create a webhook in gitlab
	1. Click on the `deploy-prod` job. Its on the top left corner.
	2. click on the `Configure`.
	3. go down to the `Build Triggers` section
	4. copy the CI service url

	![jenkins](/images/jenkins-403.png)
We will use this information in the next exercise.

### Exercise-6
#### Configure the webhook on the gitlab.
If you like the gitlab to notify Jenkins that something has changed in the repo and go redeploy it again, you need to configure something called webhook.

1. Switch to back to the `gitlab` tab.
2. Go to `ansible` project
3. Click on `settings` and then select webhook.

	![jenkins](/images/jenkins-404.png)
4. paste the url that you copied from the jenkins page

	![jenkins](/images/jenkins-405.png)
5. Click on `Add Webhook`

6. Click on `Test`.  Make sure you get `HTTP 200` success message. The message will be shown at the top.

### Exercise-6
#### Test the automate deployment
1. Switch back to `ATOM` editor
2. add new ntp server to your switches.
3. Go to `ansible --> roles --> baseconfig --> vars`
4. open the `main.yml`
5. Edit the file to add a new ntp server `199.1.5.2`
6. Save the file `CMD + S`
7. Make sure you have your `jenkins` tab open in the browser before you execute the next step. You want to watch what happens when you push your changes to the gitlab.  Basically, as soon as you push the changes to gitlab, it will trigger a webhook call to jenkins.  When the jenkins gets that call, it will start the deployment process. During this process it will download the latest copy of the repo and then run the `deploy-baseconfig.yml` playbook.
8. In the atom window, go to `Packages` and select `GIT-Plus`. Then select `Add All + commit + push`
9. It will ask you to enter your commit message. Type `added new ntp server 199.1.5.2` and then press `CMD + S` to save it.  
10. This will trigger the push to the gitlab.
11. Switch to the browser and go to jenkins tab.  Watch the automatic deploy process.
12. Go to `Console Output`.  Make sure it did all the changes successfully.
13. Switch to the switch console and type `sh run | sec ntp` . Do you see new ntp server added.
12. This will what devops is all about.

### Exercise 7
#### Branch Merge will trigger Jenkins  deploy
1. Switch to terminal window
   2. make sure you are in the `ansible` directory
   3. type
   4. `git branch`
   5. `git branch updatebase`
   6. `git checkout updatebase`
2. Switch back to `ATOM` editor
   1. navigate to `ansible --> roles --> baseconfig --> vars`
   2. open the `main.yml`
   3. add a new ntp server `1.1.1.1`
   4. `CMD + S ` to save it.
3. Switch to the chrome browser and open the `Jenkins` tab. Login in if you are logged out. Make sure you are on `deploy-prod` job.
4. Now we going to push the changes to the git.  This time, Jenkins job should be trigger since we put a filter that it should only trigger `deploy-prod` job only on the changes of the `master` branch.
5. Switch to the terminal window
   6. `./gitpush.sh`
6. Did Jenkins build the job?
7. Now login to `gitlab` portal.
   8. you should see button for `Create Merge Request`.  Click on it.
   9. enter the description for the merge.
   10. assign it to yourself.
   11. click on th `Submit merge request`
   12. In the next screen click on `Accept Merge Request`
   13. Switch to `Jenkins` tab.
   14. Did the `deploy-prod` job got deployed?



### Troubleshooting
**Tips**
Sometime you will need to login into jenkins container to troubleshoot this. Use this method:

`docker exec -u 0 -it jenkins bash`

or

`docker exec -it jenkins /bin/bash`

All the files are under ``/var/jenkins_home/workspace/``  ; your git will be cloned into this directory
