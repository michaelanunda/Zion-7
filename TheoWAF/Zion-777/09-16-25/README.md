Week 2 Lab Notes:

SIGN INTO AWS
-------------
- Sign into the AWS Management Console (preferably with your IAM Admin User)

SETTING A BUDGET IN AWS
-----------------------
- Type budgets in the search bar and click "Budgets" under Features
- Click "Create a budget"
- Under Budget setup, select "Use a template (simplified)"
- Under Templates - new, select "Monthly cost budget"
- Under Monthly cost budget - Template, provide the following values for the given fields: 1. Budget name = nameItWhateverYouWant, 2. Enter your budgeted amount ($) = putWhateverYouCanAfford, and 3. Email recipients = putTheEmailsYouWantToGetNotified
- Click "Create budget"

* Result: 
1. Created a budget to track cost within the associated AWS account

CREATING A SECURITY GROUP
-------------------------
- Type ec2 in the search bar and click "EC2" under Services
- Click on "Security Groups" under Network & Security on the left pane
- Click the orange "Create security group" button
- Enter a name for the security group under the "Security group name" field
- Enter a description for the security group under the "Description" field
- Verify the default VPC is set under the "VPC" field
- Add two Inbound rules
- For the first Inbound rule, provide the following values for the corresponding fields: 1. Type = HTTP, 2. Source = Anywhere IPv4, and 3. Description = web server
- For the second Inbound rule, provide the following values for the corresponding fields: 1. Type = SSH, 2. Source = Anywhere IPv4, and 3. Description = secure shell
- Leave Outbound rules alone entirely
- Under tags provide the following key value: key = environment and value = dev
- Under tags provide the following key value: key = owner and value = putYourFirstAndLastName
- Click the orange "Create security group" button

* Result: 
1. Created a Security Group
   - allows inbound traffic from any IPv4 address via http and ssh, meaning that the traffic on ports 80 (HTTP) and 22 (SSH) from all IPs (0.0.0.0/0) will be permitted to reach the resource behind the Security Group (firewall)

CONFIGURING and LAUNCHING AN EC2 INSTANCE
-----------------------------------------
- Navigate to "Instances" under Instances on the left pane
- Click the orange "Launch instances" button
- Enter a name for the instance under the "Name" field
- Click "Add additional tags"
- Click "Add new tag" 2x
- Provide the following key value for the second tag: key = environment and value = dev
- Provide the following key value for the third tag: key = owner and value = putYourFirstAndLastName
- For the Application and OS Images (Amazon Machine Image) section, keep the preselected default values/options associated with Free tier eligibility then skip over to the next section
- For the Instance type section, keep the preselected default values/options associated with Free tier eligibility then skip over to the next section
- For the Key pair (login) section, click "Create a new key pair" and enter the following values for the given fields: 1. Key pair name = class7-week2-key, 2. Key pair type = RSA, and 3. Private key file format = .pem
- Click "Create key pair"
- ^^^ Note, once this key pair is created download it to your local machine, execute chmod 400 in the terminal in order to make the key read only for the owner of the file, then move the file into the hidden directory on the local machine that is called .ssh
- For the Network settings section, do the following: 1. Under "Network", ensure the default vpc is selected (like step 7), 2. Under "Auto-assign public IP", ensure that the value is "Enable", 3. Under "Firewall (security groups)", click on "Select existing security group" then select the security group that was made (see CREATING A SECURITY GROUP) under the field "Common security groups"
- For the Configure storage section, keep the preselected default values/options then skip over to the next section
- For the Advanced details section, expand it, scroll down to "User data - optional", then copy and paste the contents of the ec2 user-data script (https://github.com/MookieWAF/bmc4/blob/main/ec2scrpit) into the empty box associated with "User data - optional" 
- Click the orange button called "Launch instance"
- ^^^ Note, there is a field called "Number of instances" that allows you the enter the desired number of instances you want to launch based on the configuration within the launch instance menu
- Click "Instances" on the top left of the screen to return to the EC2 Dashboard

* Result: 
1. Created an ec2 instance
   - created a key pair that is associated with the ec2 instance (in order to ssh into the instance via port 22)
   - attached a security group to the ec2 instance (see CREATING A SECURITY GROUP)
   - attached a user-data script to the ec2 instance which will be executed when the ec2 instance initially boots => https://github.com/MookieWAF/bmc4/blob/main/ec2scrpit

VERIFYING THAT THE EC2 INSTANCE AND SECURITY GROUP ARE OPERATING ACCORDINGLY VIA HTTP
-------------------------------------------------------------------------------------
- Continuing from the last section, wait for the instance to show passing status checks under the column "Status check"
- Click on the empty white box to the left of the instance name in order to select the instance
- Under "Details", copy the ec2 instance's public DNS address
- Open a separate tab on the browser, type http:// , paste the copied public DNS address in the address bar of the browser after http:// (e.g.) => http://ec2-54-234-91-12.compute-1.amazonaws.com, and finally go to the public DNS address for the ec2 instance
- The rendered user-data script screen within the browser window will confirm that the ec2 instance can be accessed via port 80 (http)

* Result: 
1. Confirmed that the security group allowed the ec2 instance to be accessed via port 80 (http)
   - also confirmed that the user-data script successfully executed on the initial boot of the ec2 instance 

VERIFYING THAT THE EC2 INSTANCE AND SECURITY GROUP ARE OPERATING ACCORDINGLY VIA SSH
------------------------------------------------------------------------------------
- Go back to the tab for the AWS Management Console
- Click on the empty white box to the left of the instance name in order to select the instance
- Click the "Connect" button on the top right
- Make sure the "EC2 Instance Connect" tab is selected and leave all the default values as they are
- Click "Connect"
- Within the terminal for the ec2 instance, execute the following linux command => ping 8.8.8.8
- Press control + c on the keyboard in order to end the continuous terminal output from the command that was previously executed

* Result: 
1. Confirmed that the security group allowed the ec2 instance to be accessed via port 22 (ssh) 
   - also executing the terminal command, ping 8.8.8.8, within the terminal of the ec2 instance helped verify that the ec2 instance has basic outbound internet connectivity as well as tests that the ec2 instance can successfully send and receive data packets from the external internet

CREATING A LAUNCH TEMPLATE OFF OF THE RUNNING EC2 INSTANCE FOR VERSIONING PURPOSES (VERSION 1)
----------------------------------------------------------------------------------------------
- Go back to the tab for the AWS Management Console
- Click on the empty white box to the left of the instance name in order to select the instance
- Click Actions > Image and templates > Create template from instance
- Under the Launch template name and description section, provide a name within the "Launch template name - required" field (e.g.) => class7-week1-hw-1
- Keep the default values for the rest of the fields
- Click Create launch template
- Click View launch templates

* Result: 
1. Version 1 of a Launch Template has been created off of a running ec2 instance
   - A launch instance can be made without a running ec2 instance via EC2 Dashboard > Instances > Launch Templates > Create launch template

MODIFYING VERSION 1 OF THE LAUNCH TEMPLATE IN ORDER TO MAKE A VERSION 2 LAUNCH TEMPLATE
---------------------------------------------------------------------------------------
- Click on the empty white box to the left of the launch template => class7-week1-hw-1
- Click Actions to the top right
- Click "Modify template (Create new version)"
- Under Launch template name and version description, put the following value under "Template version description" => class7-week1-hw-2
- Scroll all the way down to Advanced details
- Expand this section and replace the contents of the "User data - optional" section with the following script => https://github.com/michaelanunda/Zion-7/blob/main/TheoWAF/Zion-777/09-09-25/2.%20BAM%201.1/user-script-ec2-1.1.txt
- Click Create template version
- Click View launch templates

* Result:
1. Version 2 of a Launch Template has been created off of Version 1 of the Launch Template

MODIFYING VERSION 2 OF THE LAUNCH TEMPLATE IN ORDER TO MAKE A VERSION 3 LAUNCH TEMPLATE
---------------------------------------------------------------------------------------
- Click on the empty white box to the left of the launch template => class7-week1-hw-1
- Click Actions to the top right
- Click "Modify template (Create new version)"
- Under Launch template name and version description, put the following value under "Template version description" => class7-week1-hw-3
- Scroll all the way down to Advanced details
- Expand this section and replace the contents of the "User data - optional" section with the following script => https://github.com/michaelanunda/Zion-7/blob/main/TheoWAF/Zion-777/09-09-25/3.%20BAM%201.2/user-script-ec2-1.2.txt
- Click Create template version
- Click View launch templates

* Result: 
1. Version 3 of a Launch Template has been created off of Version 1 of the Launch Template

LAUNCH AN EC2 INSTANCE OFF OF VERSIONS 1 OF THE LAUNCH TEMPLATE
---------------------------------------------------------------
- Click on the empty white box to the left of the launch template => class7-week1-hw-1
- Click Actions to the top right
- Click "Launch instance from template"
- Under Choose a launch template, ensure the following values under "Source template" => 1. class7-week1-hw-1 and 2. 1 (Default)
- Click Launch instance
- Navigate to the instance menu
- Confirm that the ec2 instance is running properly by accessing it via port 80 (http) and port 22 (ssh)
- Once confirmation is done, terminate the instance and return back to the launch template menu

* Result: 
1. Confirmed Version 1 of the Launch Template has been properly configured via launching an ec2 instance off of it and testing port access on 80 (http) and 22 (ssh)

LAUNCH AN EC2 INSTANCE OFF OF VERSIONS 2 OF THE LAUNCH TEMPLATE
---------------------------------------------------------------
- Click on the empty white box to the left of the launch template => class7-week1-hw-1
- Click Actions to the top right
- Click "Launch instance from template"
- Under Choose a launch template, ensure the following values under "Source template" => 1. class7-week1-hw-1 and 2. 2
- Click Launch instance
- Navigate to the instance menu
- Confirm that the ec2 instance is running properly by accessing it via port 80 (http) and port 22 (ssh)
- Once confirmation is done, terminate the instance and return back to the launch template menu

* Result: 
1. Confirmed Version 2 of the Launch Template has been properly configured via launching an ec2 instance off of it and testing port access on 80 (http) and 22 (ssh)

LAUNCH AN EC2 INSTANCE OFF OF VERSIONS 3 OF THE LAUNCH TEMPLATE
---------------------------------------------------------------
- Click on the empty white box to the left of the launch template => class7-week1-hw-1
- Click Actions to the top right
- Click "Launch instance from template"
- Under Choose a launch template, ensure the following values under "Source template" => 1. class7-week1-hw-1 and 2. 3
- Click Launch instance
- Navigate to the instance menu
- Confirm that the ec2 instance is running properly by accessing it via port 80 (http) and port 22 (ssh)
- Once confirmation is done, terminate the instance and return back to the launch template menu

* Result: 
1. Confirmed Version 3 of the Launch Template has been properly configured via launching an ec2 instance off of it and testing port access on 80 (http) and 22 (ssh)

TEARDOWN
--------
- Make sure all ec2 instances are terminated
- Optional, you can delete the security group and launch template