# Security-Groups-and-NACLs-Mini-Project 

 A Security Groups and NACLs Project typically involves designing and implementing security controls for a cloud-based environment—most often in AWS (Amazon Web Services). These two components are part of AWS's layered security model and are used to control inbound and outbound traffic to your resources like EC2 instances, subnets, and more. 

 Before we proceed with setting up security Groups and NACLs, it's essential to ensure a solid understanding of cloud networking basics. If term like "Security Groups" and "NACLs" are unfamiliar it's recommended to review earlier materials to establish a strong foundation in cloud computing concepts. 

## Project Goals:

- Understand the concept of security groups and Network Access Control Lists (NACLs) in AWS.

- Explore how Security Groups and NACLs function as virtual firewalls to control inbound and outbound traffics.

- Gain hands on experience with configuring Security Groups and NACLs to allow or deny traffic types of traffic. 

## Security Group

**Inbound Rules:** Rules that control the incoming traffic to an AWS resource, such as an EC2 instance or an RDS database
.
**Outbound Rules:** Rules that control the outgoing traffic from an AWS resource.

**Stateful:** Security groups automatically allow return traffic initiated by the instances to which they are attached.

**Port:** A communication endpoint that processes incoming and outgoing network traffic. Security groups use ports to specify the types of traffic allowed.

**Protocol:** The set of rules that governs the communication between different endpoints in a network. Common protocols include TCP, UDP, and ICMP.

## Netwoek Access Control List (NACL)

**Subnet-level Firewall:** NACLs act as a firewall at the subnet level, controlling traffic entering and exiting the subnet.

**Stateless:** Unlike security groups, NACLs are stateless, meaning they do not automatically allow return traffic. You must explicitly configure rules for both inbound and outbound traffics.

**Allow/Deny:** NACL rules can either allow or deny traffic based on the specified criteria. 

**Ingress:** Refers to inbound traffic, ie traffic entering the subnet.

**Egress:** Refers to outbound traffic, ie traffic exiting the subnet.

**CIDR Block:** Specifies a range of IP addresses in CIDR notation (eg, 10.0.0.0/24) that the NACL rules applies to.

## Default Settings

**Default Security Group:** Every VPC comes with a default security group that allow all outbound traffic and denies all inbound traffic by default.

**Default NACL:** Every subnet with a VPC is associated with a default NACL that allows all inbound and outbound traffic by default.

## What is Security Group? 

AWS security groups are like bouncers at the door of your party. They decide who gets to come in (inbound traffic) and who gets kicked out (outbound traffic). Each security group is like a set of rules that tells the bouncers what's allowed and what's not. 

## Example 
A security can created for web server that only allows traffic on port 80 (the standard port for web traffic) from the internet. This means only the web traffic can get through, keeping your server safe from other kinds of attacks. 

Similarly, ypu can have another security group for your database server that only allows traffic from your web server. This way your database is protected, and only your web server can access it, like a VIP area at your party. 

In simple terms, security group acts as a barriers that control who can access your AWS resources and what they can do once they're in. They're like digital bouncers that keep your party (or your cloud) safe and secure. 

## What is NACL? 

NACL stands for Network Access Control Lists. Think of it like a security checkpoint for your entire neighbourhood in the AWS cloud. Imagine your AWS resources are houses in a neighbourhood, and you want to control who can come in and out. That is where NACLs come in handy. 

NACLs are neighbourhood security guards. They sit at the entrance point and check every person (or pocket of data) that wants to enter or leave the neighbourhood. 

NACLs works at the subnet level, not the individual resources level like security groups. So instead of controlling access for each house (or AWS resource). they control access to the entire neighbourhood (or subnet).

Rules can be set NACLs to allow or deny traffic based on things like IP addresses, protocols. and ports. For example, you can allow web traffic (HTTP) but block traffic on other ports like FTP or SSH. 

Unlike security groups, which are stateful (meaning like remember previous interactions), NACLs are stateless. THis means you have to explicitly allow inbound and outbound traffic separately, unlike security groups where allowing inbound traffic automatically allows outbound traffic related to that session. 

In simple terms, NACLs act as a gatekeepers for your AWS subnets, controlling who can come in and out based on a set of rules you define.They are like security guards that keep your neighbourhood (or your AWS network) safe and secure. 

## Difference Between Security Groups and NACLs

Security Groups in AWS act like virtual firewalls that control traffic at the instance level. They define rules for inbound and outbound traffic based on protocols, ports. and IP addresses. Essentially, they protect individual instances by filtering traffic, allowing only authorised communication. 

On the other hand, NACLs function at the subnet level, overseeing traffic entering and leaving subnets. They operate as a barrier for entire subnets, filtering traffic based on IP addresses and protocol numbers. Unlike security groups, NACLs are stateless, meaning they don't remember the state of connection, and each rule applies to both inbound and outbound traffic independently. 

**Note-** In security groups, there's no explicit "deny" option as seen in NACLs, any rule configured within a security group implies permission, meaning that if a rule is established, it's automatically allowed. 
It is time for practical which will be in two parts.

## 1. Security group

## 2. NACL 

## Security group

- Initially we will examine the configuration of inbound and outbound rules for security groups. 

- Create a security group allowing HTTP for all traffic and attach it to the instance.

## Explore Various Scenarios 

- Implement inbound traffic rules for HTTP and SSH protocols and allow outbound traffic for all. 

- Configure inbound rules for HTTP with no outbound rules. 

- Remove both inbound and outbound rules. 

- Have no inbound rules but configure outbound rules for all traffic. 

## NACL 

- Examine the default settings for both inbound and outbound rules in NACL configuration. 

- Modify the inbound rules to permit  traffic from any IPv4 CIDR on all ports. 

- Adjust the outbound rules to allow traffic to all CIDRs. 

## Part - 1

In the public subnet, we have created an EC2 instance that is running, hosting our website. Now, let's take a moment to see if we can access the website using it public IP address.

![EC2 Created Successfully](./img/01.%20EC2%20Created%20Successfully.png) 

So, this EC2 instance host our website. 

![EC2 Running](./img/02.%20EC2%20Running.png)

Here's the security groups configuration for the instance. In the inbound rules, only IPv4 SSH traffic on port 22 is permitted to access this instance

![Inbound Rules](./img/03.%20Inbound%20Rules.png) 

For the outbound rule, you will notice that all IPv4 traffic with any protocol on any port number is allowed, meaning this instance has unrestricted access to anywhere on the internet. 

![Outbound Rules](./img/04.%20Outbound%20Rules.png) 

Let test accessibility to the website using the public IP address assigned to this instance. Retrieving the public IP address as shown. 

![Retrieve IP Address](./img/05.%20Retrieve%20IP%20Address.png) 

Entering http:// 34.227.161.20 into your chrome browser, and hit enter. you will notice that the page doesn't load, it keeps attempting to connect. After sometimes, you will see a page indicating that the site can't be reached. 

![Page Not Reachable](./img/06.Page%20Not%20Reacheable.png) 

This is because of the security group, the HTTP protocol have not defined in the security group so whenever the outside world is trying to go inside our instance and trying to get the data, security group is restricting it and that's why we are unable to see the data. 

To resolve this issue, we are can create a new security group that allows HTTP (port 80) traffic. 

1. Navigate to the "Security Groups" section on the left sidebar. 

- Then click on "Create Security Groups". 

![Navigate to SEcurity Groups and Click Create SG](./img/07.%20Navigate%20to%20Security%20Groups%20and%20Click%20Create%20SG.png)

2. Provide a name and description for the new security group. 

- Ensure to select your VPC during the creation process. 

![Name, Description and Select VPC Created](./img/08.%20Name,%20Description%20and%20Select%20VPC%20Created.png)

- Click add rule to select ssh and http. 

![Click Add Rule](./img/09.%20Click%20Add%20Rule.png) 

- Now select ssh and http as the type. 

![Select SSH and HTTP](./img/10.%20Select%20SSH%20and%20HTTP.png) 

- Use 0.0.0.0/0 as the CIDR block. Allowing every CIDR block by using this CIDR. 

![Allowing Every CIDR](./img/11.%20Allowing%20Every%20CIDR.png)

- Keep the outbound rules as it is (default). 

![Keep Outbound Rule](./img/12.%20Keep%20Outbound%20Rule.png) 

- Click on the Create security group. 

![Create Security Group](./img/13.%20Create%20Security%20Group.png)

- Now, Security group created successfully. 

![Created Security Group Successfully](./img/14.%20Created%20Security%20Group%20Successfully.png) 

- Attaching the Security group to the instance. 

3. Now navigate to the instance section of the left sidebar. 

- Select the instances, click on "Actions" and choose "Security". 

![Navigate to Instance, Select Actions and Security](./img/15.%20Navigate%20to%20Instance,%20Select%20Action%20and%20Security.png) 

- Click on "Change security group" 

![Click Change Security](./img/16.%20Click%20Change%20Security.png) 

4. Choose the security you created. 

- Click on the "Add security group". 

![Choose Security Group and Add  Security Group](./img/17.%20Choose%20Security%20Group%20and%20Add%20%20Security%20Group.png) 

- Security group added can seen and click save. 

![Click Save](./img/18.%20Click%20Save.png)

**Note -** The security group named "Launch Wizard" seen is the default security group automatically attached when instance was created, this can also be edited if needed. 

![Security Group Added Successfully](./img/19.%20Attached%20Successfully.png) 

5. Now it being attached successfully. 

- If you again copy the public address. 

![Copy IP Address Again](./img/20%20Copy%20IP%20Address%20Again.png) 

- Write http:// 34.227.161.20 in Chrome, we will be able to see the data of our website. 

![Data of the Website](./img/21.%20Data%20of%20the%20Website.png) 

Currently, let's take a look at how our inbound and outbound rules are configured. 

This setup allows the HTTP and SSH protocols to access the instance. 

![Inbound Rules Permit](./img/22.%20Inbound%20Rules%20Permit.png) 

The outbound rules permits all traffic to exit the instance. 

![Outbound Rules Permit](./img/23.%20Outbound%20Rules%20Permit.png) 

Through this rule we are able to access the website. 

![Outbound Rules Permit the Website](./img/24.%20Outbound%20Rules%20Permit%20the%20Website.png) 

6. Let's see how removing the outbound rule affects the instance connectivity. Means now, no one can go outside the instance. 

- Go to the outbound tab. 

- Click on "edit outbound rules" 

![Click Edit the Outbound Rules](./img/25.%20Click%20Edit%20the%20Outbound%20Rules.png)

- Click on "Delete" 

- Click on "Save rules"

![Click Delete and Saves](./img/26.%20Click%20Delete%20and%20Saves.png)

Now that the outbound rules have been removed, let's take a look at how it appears in the configuration. 

![Outbound Deleted Successfully](./img/27.%20Outbound%20Rules%20Deleted%20Successfully.png)

After making this change, let's feat whether wa can still access the website. 

![Inbound Rule Permit the Website](./img/28.%20Inbound%20Rule%20Permit%20the%20Website.png)

So even though we have removed the outbound rule that allows all traffic from the instance to the outside world, we can still access the website. According to the logic we discussed, when a user accesses the instance, the inbound rule permits HTTP protocol traffic to enter. However, when the instance sends data to the user's browser to display the website, the outbound rule should prevent it. Yet, we are still able to view the website. 


Security groups are stateful, which means they automatically allow return traffic initiated by the instances to which they are attached. So, even though we removed the outbound rule, the security group allows the return traffic necessary for displaying the website, hence we can still access it. 

Exploring the scenario. 

If we delete both the inbound and outbound rules, essentially, we are closing all access to and from instance. This means no traffic can come into the instance, and the instance cannot send any traffic out. So, if we attempt to access the website from a browser or any other client, it will fail because there are no rules permitting traffic to reach the instance. Similarly, the instance won't be able to communicate with any external services or website because all outbound traffic is also blocked. 

7. You will be able to delete the inbound rule in the same way we have deleted the outbound rule. 

- Go to the inbound tabs.

- Click on "edit inbound rule".

![Click Edit the Inbound Rules](./img/29.%20Click%20Edit%20the%20Inbound%20Rules%20.png)

- Click on delete.

- Click on "Save rule". 

![Delete and Saves Inbound Rules](./img/30.%20Delete%20and%20Saves%20Inbound%20Rules.png) 

Currently, let's have a look at how our inbound and outbound rules are configured. 

![Current Look Inbound Rules](./img/31.%20Current%20Look%20Inbound%20Rules.png)

![Current Look Outbound Rules](./img/32.%20Current%20Look%20Outbound%20Rules.png) 

Now, as both the inbound and outbound rules deleted, there's no way for traffic to enter or leave the instance. This means that any attempts to access the website from a browser or any other client will fail because there are no rules permitting traffic to reach the instance. In this state, the instance is essentially isolated from both incoming and outgoing traffic. 

So you can't access the website now. 

![Website Not Accessible](./img/33.%20Website%20Not%20Accessible.png) 

In the next scenario, 

We'll add a rule specifically allowing HTTP traffic in the outbound rules. This change will enable the instance to initiate outgoing connections over HTTP. 

8. Click on edit outbound rule in the outbound tab, 

![Click Edit Outbound Rule](./img/34.%20Click%20Edit%20Outbound%20Rule.png) 

- Click on "add rule". 

![Click Add Rule](./img/35.%20Click%20Add%20Rule.png)

- Choose type.

- Choose "Destination. 

- Choose CIDR. 

- Click on "Save rules" 

![Choose Type, Destination, CIDR and Save Rules](./img/36.%20Choose%20Type,%20Destination,%20CIDR%20and%20Save%20Rules.png)

![Successfully Modified Outbound Rules](./img/37.%20Successfully%20Modified%20Outbound%20Rules.png)

Now, let's see if we can access the website, So, we are not able to see it. 

![Checking Website Access](./img/38.%20Checking%20Website%20Access.png) 

But if you look here, we are able to go to the outside world from the instance. We are using here. 

![Curl Command that Fetches Data from URL](./img/39.%20Curl%20Command%20that%20Fetches%20Data%20from%20URL.png) 

**Note-** curl is a command-line tool that fetches data from a URL. 

As a result, the instance will be able to fetch data from external sources or communicate with other HTTP-based services on the internet. This adjustment ensures that while incoming connections to the instance may still be restricted, the instance itself can actively communicate over HTTP to external services. 

## Part - 2 

Let's come Network Access Control Lists (NACLs) 

1. First navigate to the search bar and search for VPC. 

- Then click on VPC 

![Navigate Search Bar and Click VPC](./img/40.%20Navigate%20Search%20Bar%20and%20Click%20VPC.png) 

2. Navigate to the Network ACLs in the left sidebar. 

- Click on "Create Network ACL" 

![Navigate Network ACLs and Click Create](./img/41.%20Navigate%20Network%20ACLs%20and%20Click%20Create.png) 

3. Now, provide a a name for your Network ACL, 

- Choose the VPC you created in the [Previous session](./AWS VPC mini project.md) for the practical on VPC creation. 

- Then click on "Create Network ACL". 

![Create Network ACL](./img/42.%20Create%20Network%20ACL.png) 

![NACL Created Successfully](./img/43.%20NACL%20Created%20Successfully.png)

4. If you selected the Network ACL you created. 

- Navigate to the "inbound" tab. 

By default, it will be notice that it's denying all traffic from all ports. 

![Navigate to NACL Inbound Tab](./img/44.%20Navigate%20to%20NACL%20Inbound%20Tab.png) 

![Navigate to NACL Inbound Tab Cont](./img/45.%20Navigate%20to%20NACL%20Inbound%20Tab%20Cont.png)

Similarly if you look at the outbound rules, you will observe that it's denying all outbound traffic on all parts by default. 

- Select NACL. 

- And navigate to the "Outbound" tab. 

![Navigate to NACL Outbound Tab](./img/46.%20Navigate%20to%20NACL%20Outbound%20Tab.png) 

5. To make changes, 

- Select the NACL. 

- Go to the "Inbound" tab. 

- And click on "Edit inbound rules". 

![Click Edit Inbound Rule on NACL](./img/47.%20Click%20Edit%20Inbound%20Rule%20on%20NACL.png) 

6. Now, click on "Add new rule" 

![Click Add New Rule](./img/48.%20Click%20Add%20New%20Rule.png) 

7. Now, choose the rule number. 

- Specify the type 

- Select the source 

- And determine whether to allow or deny the traffic. 

- Then click on "Save changes". 

![Updated Inbound Rule Successfully](./img/49%20Updated%20Inbound%20Rule%20Successfully.png) 

Currently, this NACL is not associated with any of the subnets in the VPC. 

![NACL not Associated with Subnet](./img/50.%20NACL%20not%20Associted%20with%20Subnet.png) 

8. Let's associate it. 

- Select your NACL 

- Click on "Actions" 

- Choose "Edit subnet association". 

![Edit Subnet Association](./img/51.%20Edit%20Subnet%20Association.png) 

- Then select your public subnet, as our instance resides in the public subnet.

Once selected, you will see it listed under "Selected subnets"

- Finally, click on "Save changes". 

![Select Public Subnet and Save Changes](./img/52.%20Select%20Public%20Subnet%20and%20Save%20Changes.png) 

Public subnet have been successfully associated to the NACL. 

![Successfully Added Subnet to NACL](./img/53.%20Successfully%20Added%20Subnet%20to%20NACL.png) 

As soon as the public subnet is attached to NACl, and then you try to access the website again by typing the URL http:// 54.227.161.20, you will notice that you are unable to see the website. 

![Unable to Access website](./img/54.%20Unable%20to%20Access%20website.png) 

Although we are permitted all traffic in the inbound rule of our NACL, we are still unable to access the website. This raises the question why isn't the website visible despite these permission? 

The reason why we are unable to access the website despite permitting inbound traffic in the NACL is because NACLs are stateless. They don't automatically allow return traffic. As a result, we must explicitly configure rules for both inbound and outbound traffic. 

Even though the inbound rule allows all traffic into the subnet, the outbound rules are still denying all traffic. You can see. 

![Inbound Rule Allows All Traffic](./img/55.%20Inbound%20Rule%20Allows%20All%20Traffic.png) 

![Outbound Rule Deny All Traffic](./img/56.%20Outbound%20Rule%20Deny%20All%20Traffic.png) 

![Network ACLs Diagram](./img/57.%20Network%20ACLs%20Diagram.png) 

9. If we allow outbound traffic as well, 

- Choose your NACL. 

- Go to outbound tab. 

- Click "Edit outbound rules". 

![](./img/58.%20Edit%20Outbound%20Rules.png) 

- Click on "Add rule".

- Duplicate the process you followed for creating the inbound rules to establish the outbound rules in a similar manner. 

![Add New Rule, Update and Save Changes](./img/59.%20Add%20New%20Rule,%20Update%20and%20Save%20Changes.png) 

Successfully updated the Outbound rules. 

![Successfully updated the Outbound rules](./img/60.%20Successfully%20updated%20the%20Outbound%20rules.png) 

Upon revisiting the website, you should now be able to access it without any issues. 

![Website Accessible](./img/61.%20Website%20Accessible.png) 

Now, let's see one more interesting scenario. 

## In this Scenario

**Security Groups:** Allows inbound traffic for HTTP and SSH protocols and permits all outbound traffic. 

**Network ACL:** Denies all inbound traffic. Let's observe the outcome of this configuration. 

## Security Group 

Configuring security group 

![Configuring Security Group Inbound Rule](./img/62.%20Configuring%20Security%20Group.png)

![](./img/63.%20Configuring%20Security%20Inbound%20Rule%20Cont.png) 

![Configuring Security Group Outbound Rule](./img/64.%20Configuring%20Security%20Group%20Outbound%20Rule.png) 

![Configuring Security Group Outbound Rule Successfully](./img/65.%20Configuring%20Security%20Group%20Outbound%20Rule%20Successfully.png) 

## NACL 

Let's remove, it so by default it be denied all traffic. 

![Remove NACL Inbound Rule Allow](./img/66.%20Remove%20NACL%20Inbound%20Rule%20Allow.png) 

![NACL Inbound Rule By Default](./img/67.%20NACL%20By%20Default.png) 

Additionally, the outbound rule will be removed, defaulting to deny all traffic by default. 

![Remove NACL Outbound Rule Allow](./img/68.%20Remove%20NACL%20Outbound%20Rule%20Allow.png)

Let's try to access the website. 

![NACL Outbound Rule By Default](./img/69.%20NACL%20Outbound%20Rule%20By%20Default.png) 

![Access Denied](./img/70.%20Access%20Denied.png)
So we are unable to access the website. Why? Even if we have allowed inbound traffic for HTTP in the security group. 
Imagine you are at the entrance of a building and their is a security guard checking everyone who wants to come in. That security guard is like NACL. They have a list of rules (like "no backpacks allowed" or "no food or drinks inside"), and they check each person against these rules as they enter. 

Once you are inside the building, there's another layer of security at each room's door. These are like the Security Groups. Each room has its own rules, like "only employees allowed" or "no pets". These rules are specific to each room, just like Security Groups are specific are specific to each EC2 instance. 

So, the traffic first goes through the NACL (the security guard at the entrance), and if it passes those rules, it then goes through the Security Group (the security check at each room's door). If it doesn't meet any of the rules along the way, it's denied entry. 

The reason we can't see the website is because the NACL has denied inbound traffic. This prevents traffic from reaching the security group, much like a security guard not allowing entry to another room if access to the building denied. Similarly, if someone can't enter a building, they can't access any rooms inside without first gaining entry to the building. 

## Let's Have a Look on Some Scenario and their Outcomes 

- NACL allows all inbound and outbound traffic, Security Group denies all inbound and outbound traffic: Outcome: Website access will be blocked because the Security Group denies all traffic, overriding the NACLs allowance. 

- NACL denies all inbound and outbound traffic, Security Group allows all inbound and outbound traffic: Outcome: Website access will be blocked because the NACL denies all traffic, regardless of the Security Group's allowances.   

- NACL allows HTTP inbound traffic, outbound traffic is denied, Security Group allows inbound traffic and denies outbound traffic: Outcome: Website access will be allowed because the Security Group allows HTTP inbound traffic, regardless of the NACL's allowances. However, if the website requires outbound traffic to function properly, it won't work due to the Security Group's denial of the outbound traffic. 

- NACL allows all inbound and outbound traffic, Security Group allows HTTP inbound traffic and denies outbound traffic; Outcomes: Website access will be allowed because the Security Group allows HTTP inbound traffic, regardless of the NACL's allowances. However, if the website requires outbound traffic to function properly, it won't work due to the Security Group's denial of the outbound traffic. 

- NACL, allows all inbound and outbound traffic, Security Group allows inbound and outbound traffic: Outcome: Website access will be allowed, as both NACL and Security Group allows all traffic. 

- NACL denies all inbound and outbound traffic, Security Group allows HTTP inbound traffic and denies outbound traffic: Outcome: Website access will be blocked because the NACL denies all traffic, regardless of the Security Group's allowances. 

## Projection Reflection 

During this learning module, the student gained practical experience and in-depth understanding of network security within AWS environments. Key areas of learning included:

**Configuration of Security Groups and NACLs:** Successfully configured both Security Groups and Network Access Control Lists (NACLs) to regulate inbound and outbound traffic, ensuring secure and controlled access to AWS resources.

**Understanding Key Differences:** Clearly identified the distinctions between Security Groups and NACLs, including their stateful and stateless behavior, and recognized their specific roles in enhancing network security.

**Scenario-Based Exploration:** Analyzed various real-world scenarios to observe how Security Groups and NACLs interact and collectively influence network traffic patterns.

**Troubleshooting Techniques:** Acquired effective troubleshooting skills for diagnosing and resolving network connectivity issues, using tools and strategies tailored for the AWS environment. 

**Confidence in Practical Application:** Overall, the student developed a strong foundation and confidence in applying network security concepts, with a focus on practical, hands-on management of AWS networking components. 