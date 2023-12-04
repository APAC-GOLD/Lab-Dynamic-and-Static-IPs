# Internet Protocols - Static and Dynamic Addresses
SCENARIO
Your role is a cloud support engineer at Amazon Web Services (AWS). During your shift, a customer from a Fortune 500 company requests assistance regarding a networking issue within their AWS infrastructure. The email and an attachment of their architecture is below:

TICKET FROM YOUR CUSTOMER
Hello Cloud Support! We are having issues with one of our EC2 instances. The IP changes every time we start and stop this instance called Public Instance. This causes everything to break since it needs a static IP address. We are not sure why the IP changes on this instance to a random IP every time. Can you please investigate? Attached is our architecture. Please let me know if you have any questions.

Thanks! Bob, Cloud Admin

ARCHITECTURE DIAGRAM
Customer architecture of a VPC, Internet Gateway, public subnet and an EC2 instance.

Figure: Customer VPC architecture, which includes one public subnet and one EC2 instance

OBJECTIVES
In this lab, you will:

Summarize the customer scenario
Analyze the difference between a statically and dynamically assigned IP addresses using EC2 instances
Assign a persistent (static) IP to an EC2 instance
Develop a solution to the customers issue found within this lab; after developing a solution, summarize and describe your findings
DURATION
This lab total duration is 60 minutes.

AWS SERVICE RESTRICTIONS
In this lab environment, access to AWS services and service actions might be restricted to the ones that are needed to complete the lab instructions. You might encounter errors if you attempt to access other services or perform actions beyond the ones that are described in this lab.

Start lab
To launch the lab, at the top of the page, choose Start lab.
 You must wait for the provisioned AWS services to be ready before you can continue.

To open the lab, choose Open Console.
You are automatically signed in to the AWS Management Console in a new web browser tab.

 Do not change the Region unless instructed.

COMMON SIGN-IN ERRORS
Error: You must first sign out


If you see the message, You must first log out before logging into a different AWS account:

Choose the click here link.
Close your Amazon Web Services Sign In web browser tab and return to your initial lab page.
Choose Open Console again.
Error: Choosing Start Lab has no effect
In some cases, certain pop-up or script blocker web browser extensions might prevent the Start Lab button from working as intended. If you experience an issue starting the lab:

Add the lab domain name to your pop-up or script blocker’s allow list or turn it off.
Refresh the page and try again.
Task 1: Investigate the customer’s environment
Recall what you’ve learned about static and dynamic IP addresses. Which type of IP address do you think Bob assigned his EC2 instance if it constantly changes when it is stopped and started again? You will test this theory by launching one EC2 instance in the AWS lab environment. You will start with how the customer has his configured and troubleshoot the issue from there.

For task 2, you will understand the customer’s environment and replicate their issue.

In the scenario, Bob, who is the customer requesting assistance, is having issues with his EC2 instance constantly changing IP addresses every time he stops and starts his instance. He cannot leave his instance on because it is very expensive for him to do so, and he requires this IP address to be set as a static IP address or else it breaks his other resources attached to it.

Once you are in the AWS console, type and search for EC2 in the search bar on the top-left corner. Select EC2 from the list.
Tip: Alternatively, You can also find EC2 under Services - Compute in the top left corner

The search bar can be used to find the Amazon EC2 service. Once you find the service, select it.

Figure: AWS Management Console search bar.

You are now in the Amazon EC2 dashboard. In the left navigation menu, choose Instances. This option takes you to your current EC2 instances. You should currently see one EC2 instance, which you can ignore for now. We will not use that instance since we will launch our own for task 1.
From the top right corner, select Launch instances. This is how you will launch EC2 instances from the console.
Launch EC2 instance

Figure: Launch EC2 instances by selecting the button at the top right corner.

Follow the steps below to complete the creation of an Amazon EC2 instance:
Step 1: Set Name

In the Name and tags section enter 

test instance
 in the Name field.
Step 2: Choose an Amazon Machine Image (AMI):

In the Application and OS Images (Amazon Machine Image) section, select Amazon Linux from the Quick Start tab.
Make sure Amazon Linux 2023 AMI is selected from dropdown list. An AMI is a template that contains the OS and configuration of the EC2 instance.
Step 3: Choose an Instance Type:

In the Instance type section, select t2.micro if it is available, otherwise select t3.micro.
Step 4: Choose a Key pair:

In the Key pair (login) section, select the key pair AWSLabsKeyPair-<random string - stack Id> | RSA.
Step 5: Configure Network Settings:

At the top of the Network settings section, select Edit, and configure the following settings.
VPC - required: Choose vpc-xxxxxxxx | Lab VPC
Subnet: Choose subnet-xxxxxx | Public Subnet 1
Auto-assign Public IP: Set to enable
Under Firewall (security groups), select the Select an existing security group radio button and choose the security group with the name Linux Instance SG.
Step 6: Launch Instance:

In the bottom right corner of the Summary pane, select Launch Instance.
At bottom of the Next Steps - preview page, select View all instances.
On the EC2 console, a new EC2 instance should appear in the Instances dashboard. Under the Instance state, you will see Initializing. Wait until it says 2/2 before continuing.
This picture shows the current state of the instance. When launched, before using the instance, it must show a "ready" state.

Figure: Instances go through states, just like when a computer is booting up. When it is ready to use, the state will say “running” and you will be able to use it for services like SSH.

Select the checkbox of your test instance. At the bottom, select the Networking tab. In this tab, observe and note the Public IPv4 address and the Private IPv4 address. Once noted, navigate to the top right of the window, select the Instance state drop-down button, and select Stop instance. Once the Instance state changes to Stopped, navigate back down to the tabs and observe the Public and Private IPv4 address.
This picture shows the instance networking tab. Here you can see public and private IPv4 addresses, public and private IPv4 DNS, VPC ID, subnet ID, and Availability Zones.

Figure: This is the networking tab for instances. This shows any networking configurations related to the instance such as public and private IPv4 addresses and public and private IPv4 DNS.

When stopping an instance, navigate to the top of the EC2 dashboard and select the "Instance state" button and select "Stop instance".

Figure: To start, stop, or terminate an instance, navigate to the top of the EC2 dashboard and select the “Instance state” button.

Now restart the test instance by navigating to the top window and selecting the Instance state and Start instance. Wait until the Instance state changes to Running. Take note of the Public and Private IPv4 addresses. What did you notice between the public and private IP addresses when you stopped and started the EC2 instance? Would you consider this the Public IP a static or dynamic IP address? What would you consider the Private IP address for the EC2 instance? Do you think we have replicated the customer’s issue?
When the instance was restarted, in the picture, the networking tab was revisited.

Figure: By starting the instance, you can see the details populate in the Networking tab.

We still haven’t solved the customer’s issue. Bob needs a permanent Public IP address that doesn’t change when he stops and restarts his instance. AWS does have a solution that allocates a persistent public IP address to an EC2 instance, called an Elastic IP (EIP).
From the EC2 dashboard, navigate to Network and Security on the left navigation and select Elastic IPs. Notice that there are no EIPs. Create one by selecting the button Allocate Elastic IP address in the top right. Keep everything as default and hit Allocate. Take note of the EIP address.

This picture shows the left hand navigation pane of the EC2 dashboard and how to navigate to "Elastic IPs". While in the EC2 dashboard, under "Network and Security" select "Elastic IPs".

Figure: Within the EC2 dashboard, under “Network and Security” in the left navigation, select “Elastic IPs”.

This picture shows the EIP home page when "Elasti IP addresses" is selected. This is where you will allocate an EIP by navigating to the top right button and selecting Allocate Elastic IP address.

Figure: Allocate an EIP by selecting the Allocate Elastic IP address button.

Select the EIP you just created by selecting the checkbox. Now attach this permanent, public IP address to the dynamic instance by navigating to the top right and navigating to Actions and Associate Elastic IP address.
This picture shows that the EIP created was selected and will now be associated to an instance by going to the "Actions" drop down menu at the top and selecting "Associate Elastic IP address".

Figure: The EIP created will now be associated to the EC2 instance by going to the actions menu and selecting “Associate Elastic IP address”.

Leave the resource type as Instance, and select test instance from the Choose an Instance drop down menu. Under Private IP address, select the empty box. The Private IP associated with that instance is selected. Click the Associate button.
This picture shows the association of the EIP to the resource type which is an instance, by choosing the instance you created in the lab and selecting "Associate".

Figure: Associate the EIP to the test instance.

Navigate back to the Instances page using the left navigation pane. Select the checkbox for the test instance and navigate to the Networking tab. Take note of the Public IPv4 address. Did you notice that the EIP address is now the Public IP address? Now stop and start the instance and observe the differences. What did you observe? Is this a static or dynamic IP address? Did you solve the customer’s issue? Why or why not?
Task 2: Send the Response to the customer (Group Activity)
Within your group, submit your findings.

Person 1 will act as Bob the customer, while Person 2 will act as the Cloud Support Engineer. Person 2 will talk over their findings to Person 1.

Note This task should only take 5-10 minutes. Walk through your findings to the class.

End lab
Follow these steps to close the console and end your lab.

Return to the AWS Management Console.

At the upper-right corner of the page, choose AWSLabsUser, and then choose Sign out.

Choose End lab and then confirm that you want to end your lab.

RECAP
In this lab, you have investigated the customer’s environment and applied troubleshooting techniques that allowed you to resolve the customers’ issue. Within the scenario, you discovered that the customer Amazon EC2 instance (public instance) had a dynamic IP address which caused it to constantly change IPs when the instance was stopped and started. In order to fix this issue, you suggested attaching an EIP in order for the IP to become persistent (static). This was tested by SSHing into the test instance and starting and stopping it with a dynamic IP address.

ADDITIONAL RESOURCES
Amazon EC2 Instance public IP addressing

EIP

For more information about AWS Training and Certification, see https://aws.amazon.com/training/.

Your feedback is welcome and appreciated. If you would like to share any suggestions or corrections, please provide the details in our AWS Training and Certification Contact Form.

© 2022 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

https://awsrestart.labs.awsevents.com/lab/arn%3Aaws%3Alearningcontent%3Aus-east-1%3A470679935125%3Ablueprintversion%2FCUR-TF-100-RSNETK-3%2F262-lab-NF-static-dynamic-ip-addresses%3A3.0.0-c9824a48/en-US