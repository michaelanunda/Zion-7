
1. Sign into the AWS Management Console

2. Navigate to the EC2 Dashboard


CREATING A SECURITY GROUP
-------------------------

3. Click on "Security Groups" under Network & Security on the left pane

4. Click the orange "Create security group" button

5. Enter a name for the security group under the "Security group name" field

6. Enter a description for the security group under the "Description" field

7. Verify the default VPC is set under the "VPC" field

8. Click "Add rule" under Inbound rules

9. For the Inbound rule, provide the following values for the corresponding fields: 1. Type = HTTP, 2. Source = Anywhere IPv4, and 3. Description = web server

10. Leave Outbound rules alone entirely

11. Under tags provide the following key value: key = environment and value = dev

12. Under tags provide the following key value: key = owner and value = putYourFirstAndLastName

13. Click the orange "Create security group" button


CONFIGURING and LAUNCHING AN EC2 INSTANCE
-----------------------------------------

14. Navigate to "Instances" under Instances on the left pane

15. Click the orange "Launch instances" button

16. Enter a name for the security group under the "Name" field

17. Click "Add additional tags"

18. Click "Add new tag" 2x

17. Provide the following key value for the second tag: key = environment and value = dev

18. Provide the following key value for the third tag: key = owner and value = putYourFirstAndLastName

19. For the Application and OS Images (Amazon Machine Image) section, keep the preselected default values/options associated with Free tier eligibility then skip over to the next section

20. For the Instance type section, keep the preselected default values/options associated with Free tier eligibility then skip over to the next section

21. For the Key pair (login) section, select "Proceed without a key pair"

22. For the Network settings section, do the following: 1. Under "Network", ensure the default vpc is selected (like step 7), 2. Under "Auto-assign public IP", ensure that the value is "Enable", 3. Under "Firewall (security groups)", click on "Select existing security group" then select the security group that was made (see steps 3-13) under the field "Common security groups"

23. For the Configure storage section, keep the preselected default values/options then skip over to the next section

24. For the Advanced details section, expand it, scroll down to "User data - optional", then copy and paste the contents of the ec2 user-data script (link) into the empty box associated with "User data - optional"

25. Click the orange button called "Launch instance"

26. Click "Instances" on the top left of the screen to return to the EC2 Dashboard


VERIFYING THAT THE EC2 INSTANCE AND SECURITY GROUP ARE OPERATING ACCORDINGLY
----------------------------------------------------------------------------

27. Wait for the instance to show passing status checks under the column "Status check"

28. Click on the empty white box to the left of the instance name in order to select the instance

29. Under "Details", copy the ec2 instance's public DNS address

30. On a separate tab on the browser, type http:// , then paste the copied public DNS address => http://ec2-54-234-91-12.compute-1.amazonaws.com, then go to the public DNS address for the ec2 instance

31. The rendered screenshot will confirm that the ec2 instance can be accessed from any machine in the world via port 80 (http)


TERMINATE THE EC2 INSTANCE
--------------------------

32. Go back to the tab for the AWS Management Console

33. Click on the empty white box to the left of the instance name in order to select the instance

34. Click Instance state, then select "Terminate (delete) instance"

DELETE THE SECURITY GROUP ASSOCIATED WITH YOUR EC2 INSTANCE

35. Click on "Security Groups" under Network & Security on the left pane

36. Click on the empty white box to the left of the security groups name in order to select the security group

37. Click on Actions then select "Delete security group"

38. Click the orange "Delete" button => NOTE: The actual requirement to delete a security group in AWS is that the security group cannot be associated with any instance or network interface at all, regardless of the instance state. This means the security group must not be attached to any running, stopped, or terminated instance or any other resource like network interfaces. Also, the security group cannot be referenced by rules in other security groups to be deleted.