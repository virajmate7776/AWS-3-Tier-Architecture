# Deploying A Highly Scalable AWS 3-Tier Architecture

<br>
<br>
<br>
<br>


![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/daeb1b31-4705-496c-9170-44100e78c667)


<br>
<br>


### What is 3-Tier Architecture?

<br>
         Three-Tier Architecture is a client-server architecture that consist of three main components namely, process logic, data access, data storage in addition a user interface which helps in developing and maintaining modules independently on separate platforms.
Three-tier architecture can be used in the development of various infrastructures. For this project we will be taking a look at a three-tier architecture relating to AWS.

<br>


### Why is it called 3 tiers?

<br>
        This is called 3 tiers because the system has three distinct layers or tiers. The user accesses the application using a frontend also known as the presentation layer, this layer interacts with the application layer also known as the backend, and this application layer or backend saves and retrieves information from the database as needed.
        For this project we will be building the 3 tiers system with various components namely, designated region, an internet gateway for internet access, a designated virtual private cloud (VPC) for security, application load balancer for high availability, different AZ in the same region for durability and scalability, public and private subnets for performance and speed, NAT gateway, Autoscaling groups, EC2 and private relational database for storage. 

<br>

### Importance of 3 Tier Architecture:
<br>
1.	It adds reliability and more independence of the underlying servers or services.
2.	It gives you the ability to update the technology stack of one tier, without impacting other areas of the application.
3.	Components are reusable
4.	Faster development, as division of labour is implemented. Web designer does presentation, software engineer does logic, and Database admin does data modelling.
5.	Applications can exploit the modular architecture of enabling systems using easily scalable components, which increases availability.

### 1.	Creating a VPC
  
  <br>    
  
  In the AWS console, navigate to the search tab and type “VPC.” Once you are  in the VPC section click “Create VPC”, there we will input the VPC name, set an IPv4 CIDR to 10.0.0.0/16, ensure you click on “VPC only” and create the VPC 

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/b07d6d35-13f8-4326-a42a-138697265444)


<br>


 

### 2.	Creating Subnets
<br>

  We will create 6 subnets (2 for each layer). We will name them, Public Web tier Subnet 1 and 2, Private App tier Subnet 1 and 2, and Private Database Subnet 1 and 2. When creating each subnet we have to select our VPC, name the subnet, choose the availability zone, and assign a CIDR  block as 10.0.1/2/3/4/5/6.0/24.

<br>




 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/87ee3080-4221-444f-b334-9a9efb52e056)

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/f2f09784-3da0-4ef2-a87f-d77dbc5b2b92)


<br> 

 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/f9708d13-1756-4e63-af15-b99c90091b04)

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/38f3544b-63ce-4033-bf3e-e041b764d6b9)


<br>


“Enable auto-assign public IPv4 address” in the public subnet   settings. Just select the subnets one at a time, go to “Actions” on the top, and go to “edit subnet settings”.

<br>


![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/7aa5b8a4-f0c0-4d68-8a1e-9ec38e612f88)

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/11a1ee5f-7494-449f-b78b-eff42284ae05)


 
<br>

 

  Now all of our subnets are set up properly.

<br>

### 3. Internet Gateway
  
  <br>
  
  Let’s set up the internet gateway. In the VPC service section, click on “Internet Gateways” in the left hand column, create a internet gateway . After creating a new Internet Gateway, under “Actions,” click on “Attach to VPC.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/e7499824-6622-4034-bbaa-e5436d67390d)

<br>

 
Let’s make sure we attach it to the VPC we just made. To do that, we go to “actions” and select “attach to VPC”.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/3142aa4f-cea2-47d0-aca6-5655b5612ca3)

 
On the next page select the VPC and click on “attach internet gateway”.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/61fe845b-a343-41d6-a991-c2f69a6e303c)

<br>

 
  The VPC is now attached.

<br>

### 4. Creating The Nat Gateway
   
  <br>
  
   We will now be creating the NAT gateway so that our private subnets can reach the internet and also the other services in the VPC. Navigate to the left panel and select “NAT Gateway”, then select “Create NAT Gateway”. You will want to name your NAT Gateway, assign it to the second public subnet and assign it an Elastic IP by selecting “Allocate Elastic Ip”. Scroll down and select “Create NAT Gateway”.

<br>

 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/cc3afc02-869b-4e37-a7b4-14f357a73040)

<br>

### 5. Creating and Configuring Route Tables

<br>

In order to ensure the three-tier system is functioning and communicating well with each other there are certain services that need to be created, namely the route table and subnet association. In the search bar navigate to VPC, on the left tab click on “Route tables” and click on “Create route table”. Create  tables for the Private and Public  and select the VPC created earlier.

<br>


![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/01a3c5dd-0195-4d58-b087-3d7939da4270)


<br>
 
We’re not done with this route table yet. We need to add the two public subnets to it. To do that, we go to the Explicit subnet associations section and click on “Edit subnet associations”.
 
![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/262ddc9c-8542-4370-b5ce-1ea61e68cece)

<br>

Select the public subnets and click “Save associations”. We’re still not done here. Now we go to routes and click “Edit routes” to add the internet gateway to the route.
Click ‘add route” and select 0.0.0.0/0, then “Internet Gateway” for the target. Click “Save changes”.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/ebddc622-1708-45a8-b1b3-e0a8434595b2)

<br>

   Now we can create the private route table. We can just repeat the same process of naming the table and selecting the VPC but we should include private on the name so we can find it later.
<br>

When we go to “Edit subnet associations”, we’re going to check all 4 private subnets and click “Save associations”.
For the routes, we’re going to add a route with destination 0.0.0.0/0 on this table too but for the target, we’re selecting NAT gateway instead of internet gateway. Save the changes.

<br>

For the routes, we’re going to add a route with destination 0.0.0.0/0 on this table too but for the target, we’re selecting NAT gateway instead of internet gateway. Save the changes.


<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/7cfac227-3e8f-46c8-81b0-86e5cd68fba2)


<br>


 
We now have the infrastructure needed to set up the three tiers. We’re going to built the tiers one at a time starting with the web tier.

<br>

### Web Tier

<br>

 We will create auto scaling groups, but before we do that we need to create launch template.

<br>

From the AWS console, navigate to Instances and click Launch templates > Create launch template.

<br>

#### From here complete the following:

<br>

1.	Name your Template & provide description.
2.	Auto Scaling guidance — check the box that says “Provide guidance to help me set up a template that I can use with EC2 Auto Scaling”
3.	Instance type — select t2.micro.
4.	Select the AMI you want -I chose the Amazon Linux
5.	Create or select an existing key pair
6.	Configure security group rules. For this step, we will create two rules for ssh and HTTP with 0.0.0.0/0 as the source. As you can see from the alert message, by setting the source to 0.0.0.0/0 will allow all IP addresses to access our instance. 
7.	Under Advanced network configuration — Click ENABLE: Auto-assign public IP
8.	In the Advanced details section, add the below bootstrap script under User data.

<br>

    #!/bin/bash

    #Update
    sudo yum update -y

    #Install
    sudo yum install -y httpd

    #Start
    sudo systemctl start httpd

    #Enable
    sudo systemctl enable httpd

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/dfb0d311-0c68-4544-a3dc-ba815eb3d25d)

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/e89a0f99-3d28-46fd-9d80-9dae576f4158)

 
<br>

 

### Create Web Tier Auto Scaling Group

<br>

1.	Navigate to EC2 in the search tab.

2.	On the left scroll down to “Auto Scaling” and click on “Auto Scaling Groups” and fill in with the desired name and launch template.

 <br>

 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/66c76413-27c7-4173-86e5-cf4cafb672d7)

 
<br>


Select the VPC for this project and the two public web tier subnets. Click “Next”.
 
<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/b5212581-1ec6-454a-a4c4-5f34d811242e)

<br>

Select Attach to a new load balancer, Application load balancer, and Internet-facing. Then select Create a target group and click “Next”.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/3e085c13-32e3-43aa-96d9-9232dd1c77ae)

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/18fb0d30-957f-4b33-b7a1-86712f66097c)

<br>

Select 2 for Desired and Minimum capacity, 4 for Maximum capacity. Leave everything else in default and click “Next” from here to the final screen and click “Create auto scaling group”.
 
<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/7c9ae1f0-79f5-4170-ba6c-8a08c0936043)

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/5060e675-b3b8-4450-baf5-c836b4e06b03)

<br>


We should have two instances running.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/131b5bac-b47b-4df8-b325-f2f3ca7d3332)

<br>

Let’s make sure we can access  Apache servers on both machines.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/a2952883-e4f9-4326-9011-9308a41adda9)
 
<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/85fed4d3-ebd6-4036-bd18-718b71d5f340)

<br>


Web tier is done. Now we can move on to the application tier.

<br>

### Application Tier

<br>

This should be a lot quicker to set up for us given that we’re repeating the same process we did for the web tier for the most part but with a few differences.
1)  On the EC2 dashboard, go back to Launch Templates → Create launch template. Let’s name the template and add a description. Be sure to check “Provide guidance” on the Auto scaling guidance section, click on quick start and choose the same free tier OS you used for the web tier- in my case, Amazon Linux.
2) Use the same free tier instance type as the web tier- I went with t2.micro, and select the key pair we made for this project. Next, network settings → create security group. Name it and add a description, then select the VPC you’ve been using for the project. Click “Add security group role”.
3) Select ssh for type, and select the security group we made for this project as the source. Add a 2nd security group rule and select “All ICMP-IPv4” for the type, and 0.0.0.0/0 as the source. Leave everything else in default and click “Create launch template”.

   <br>
 
![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/94e98119-60b2-4917-b280-bf5d32823fa4)

<br>

Let’s set up the auto scaling group for it.

 <br>
 
 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/3af36d4d-a3cb-45da-bb3c-e556d7ea99a5)

<br>

Name it, select the template we just made, and click “Next”. Select the VPC, and the corresponding availability zones for this tier. Click “next” again to move on to the next screen.

 <br>

 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/ce9a504f-f08b-47a7-8ef4-6863eab5318e)

<br>

We’re going to go with No load balancer for the app tier and click “Next”.

 <br>
 
 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/9a554989-27da-4a43-87a9-c0530db27c01)

<br>

The Group size will be the same as the web tier- Desired and Minimum capacity will be 2 and maximum capacity will be 

4. Click “Next”.

 <br>

 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/e818049b-f00f-4050-88c2-49d1b4c2e33a)

 <br>

 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/55e77172-c291-4395-be9c-8b73ec44397b)


 <br>

 
It was successfully created but let’s also check to see of the instances are up and running.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/ec1afca7-c95e-4b2a-bfd3-1e5395a7d198)

<br>

 
Name the EC2 Instances.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/53983501-ace4-45ac-a337-5b325c217f33)

<br>



 
One more thing before we move on to the database tier, let’s SSH into the web tier’s subnet 1 to verify we can ping the app tier from the web tier.

<br>

Select the corresponding instance and click “Connect”. Click on SSH client, and open your preferred terminal app on your computer. Navigate to the directory where you saved the key pair for this project.
*Run the following command on the terminal to make sure the key pair file has the right permissions.

<br>

     chmod 400 yourkeypair.pem


<br>

Copy the example on the SSH client screen and paste it on your terminal. Once we have access to the instance, run the ping command for the private IP address of the app tier’s private subnet.
<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/4bb92898-e26d-452f-a87c-a60627a37b96)

<br>

It’s working!

<br>

type in ctrl C to stop the ping.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/954ba7af-b356-4f8a-af22-82188129dd01)

<br>


Alright! Now we can start the last tier.

<br>

### Database Tier

<br>

Search for RDS on the AWS console and click “Create database”.

<br>

Select Standard create and MySQL.
 
<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/ce35409f-4039-471d-9005-0846e0556453)

<br>

Select free tier for the templates and name the database. You can keep admin for the master username but be prepared to make a master password.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/3d23c2d4-3771-4ab8-a499-cc43055bd1c6)

<br>

For Instance configuration let’s make sure burstable classes is checked and then skip to the connectivity section to select the VPC for the project. Click “Create new” for VPC security group and name it. Leave everything else at default as you scroll down and click “Create database” at the bottom.
 
 <br>
 
![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/8c642ae1-c573-4755-907e-105b00583442)

 <br>

 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/3b7bc5cb-8a44-4ca7-b3a9-fe5096c706bc)

 <br>

 ![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/ecf20ef4-e033-453b-be42-a20b2671fbcf)

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/e061c17b-d048-4ef4-9601-5fb38361fec2)

 <br>
 
Last thing left is editing the security group for this tier. Go the VPC dashboard in the AWS console and click on security groups. Select the security group for the database tier, click “Actions”, and select “Edit inbound rules”.

<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/ba8489dd-8b28-48c4-8d37-41a9703ab416)

<br>


Delete the security group rule, click “Add rule”, select MySQL/Aurora for the Type, and select the database security group for the Source. Click “Save rules”.
<br>

![image](https://github.com/virajmate7776/AWS-3-Tier-Architecture/assets/117629972/18aa7f45-42b6-4807-9e79-a406b58c91ac)

<br>


That’s the last step for 3 tier project!
Make sure to clean up the resources terminate the instances, NAT gateways, and other services we created.

