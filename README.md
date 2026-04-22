<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Video Demonstration</h2>

- ### [YouTube: How to Deploy on-premises Active Directory within Azure Compute](https://www.youtube.com)

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)



<h2>Deployment and Configuration Steps</h2>

![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/0c36a0a4-e348-4901-afa1-a1fa8faff54b)

In this lab we will create two VMs in the same VNET. One will be a Domain Controller, the other will be a Client machine. We will change the DC to a static IP because its offering Active Directory services to the client machine. Client machine will be joined to the domain. We will control the DNS settings on the client machine, the client machine will use the DC as its DNS server.
</p>
<br />

![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/82d34938-11ee-4c00-a4b6-6bbe2859ac89)![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/07723362-f1c5-4d0e-b364-3bdff70ac9e3)


DC-1 has to have a static Private IP Address. Client one will connect to DC-1 to ensure connectivity we will try to ping DC-1 from Client-1. At first the ping will not work correctly. We have to enable ICMPv4 on the firewall on DC-1. Now we can ping DC-1 successfully from Client-1
</p>
<br />

![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/23322a09-dfd7-4a8a-b66e-c56423dd5c32)

Now we will log back into DC-1 to install AD Users & Computers. Promote the VM to DC, setup a new forest as "mydomain.com" afterwards restart then log back into DC-1 as user: "mydomain.com\labuser". If you performed the steps properly you should be able to run AD Users & Computers as shown below.
</p>
<br />

![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/58cb23a4-cbcb-4cd8-aa30-8f07951a0073)![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/2c8ae314-8354-415e-86cc-852598801714)


Excellent! We can start creating Organizational Units (OU). Let's first create an OU named _EMPLOYEES. Create another OU named _ADMINS. In order to do that right click on the domain area. Select new->Organizational Unit and fill out the field. Then click inside of your OU and right click, select new and select user and fill out the information for your new user. The user should be named Jane Doe, she is going to be an Admin so her username will be Jane_admin. Lastly add Jane to the domain admins security group.
</p>
<br />

![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/8216b386-c6be-4196-8a4a-85b82fa1adfa)![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/7dc5d4d4-aa98-4448-8704-0b7032501438)


From now on you can use Jane_admin as the administrator account. Now we will join Client-1 to the domain (mydomain.com) from the azure portal we will change client-1's DNS settings to the DC's Private IP address. After you do that restart Client-1 from within the Azure portal. Our picture below shows verification that client-1 is on the DC-1 DNS.
</p>
<br />

![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/a4033252-f631-46a4-bbb0-300b9fb954f6)

We have to join Client-1 to the domain in order to do so navigate to your system settings and go to about. Off to the right select rename this pc (advanced). From there select to change the domain. Enter "mydomain.com" after that enter your credentials from mydomain.com\labuser. Your computer will restart and then client-1 will be a part of mydomain.com
</p>
<br />

![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/b6021d2e-9248-40d2-9f78-a2dd821b0b4a)

Wonderful! Client-1 is now a part of the domain. Now we will set up remote desktop for non-administrative users on Client-1. We have to log into Client-1 as an admin and open system properties. Click on "Remote Desktop", allow "domain users" access to remote desktop. After completing those steps you should be able to log into Client-1 as a normal user.
</p>
<br />

![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/c221d85c-407c-4cce-84cf-8fa4f4f34a58)![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/41a8bdd8-8dc1-4f19-8f86-54ed7179e661)![image](https://github.com/JaMyraJones/configure-ad/assets/145633544/faf253f2-7c1a-4cd6-9df6-e0beca7bb6ea)



Lastly to verify that noraml users can RDP into Client-1 we will use a script to generate thousands of users into the domain. We will input the script in powershell, after the users are created we will select one and RDP into Client-1.


</p>
<br />


As you can see the Powershell script created a user with the username "bab.hubo" We were able to login to Client-1 with his credentials as a normal user.
</p>
<br />
