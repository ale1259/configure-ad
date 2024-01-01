<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />



<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure:

  -Create a Resource Group called "ADLab" (AD stands for Active Directory).

  -Create the Domain Controller Virtual Machine(VM) (Windows Server 2022) named “DC-1”.

  -Take note of the Resource Group and Virtual Network (Vnet) that were created at this time "ADlab" and "DC-1-vnet".

  -Set Domain Controller’s NIC Private IP address to be static. To do this go to "DC-1" in the Azure Portal -> Virtual Machines -> DC-1 -> Networking -> Network Interface(click there) -> IP Configurations -> ipconfig -> Allocation -> Static and Save. 
  
  -Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and  the Vnet of "DC-1" in Step 1.

  -Set up DNS Server of "Client-1" to be "DC-1" IP Address. For this step we need to go to the Azure Portal -> Virtual Machines ->  Client-1 -> Networking -> Network Interface(click there) -> DNS Servers -> Custom -> DC-1 IP Address -> Save. Restart "Client-1"

 
  -Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher)

- Ensure Connectivity between the client and Domain Controller:

  -Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t "ip address of DC-1" (perpetual ping).

  <img src="https://i.imgur.com/ExD61na.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

  -It will say "Requested timed out" and to fix this we need to login to the Domain Controller and enable ICMPv4 in on the local windows Firewall. For this open Windows Defender Firewall -> Advanced Settings -> Inbound Rules and look up for Core Networking Diagnosticos-ICMPv4 Echo Request (ICMPv4-In), enable both right click Enable Rule.

  <img src="https://i.imgur.com/kIJVisq.png" height="29%" width="29%" alt="Disk Sanitization Steps"/><img src="https://i.imgur.com/WVbx9k6.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>


  -Check back at Client-1 to see the ping succeed.

  <img src="https://i.imgur.com/aCDi8pt.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
  


<h2>Deployment and Configuration Steps of Active Directory</h2>

- Install Active Directory:
 
  -Login to DC-1 and install Active Directory Domain Services. In the Server Manager click on Add roles and features, click next until Server Roles and click on "Active Directory Domain" Services and Add Features continue and install. Wait the installation to complete.

<img src="https://i.imgur.com/T5bxHmw.png" height="60%" width="=60%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/1KVOre5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
  -Close the installation window and go to notifications and promote this server to a domain controller: Setup a new forest as example.com (can be anything, just remember what it is). Set the password and click next until installation.
  
  <img src="https://i.imgur.com/vXmeiwA.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/zC9UDvk.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

  -It will restart, but if not, restart and then log back into DC-1 as user: example.com\labuser.

 <img src="https://i.imgur.com/o838YHq.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
 
- Create an Admin and Normal User Account in AD:
 
  -In Active Directory Users and Computers (ADUC), create an Organizational Unit (OU) called “_EMPLOYEES”, one called "_ADMINS" and another called "_CLIENTS"

  <img src="https://i.imgur.com/jwF5pQF.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

  -Create a new employee named “John Smith” (same password) with the username of “johnadmin”. When creating the passsword uncheck. "User must change password at next logon" and check "Password never expires"
  <img src="https://i.imgur.com/hm6V026.png" height="50%" width="50%" alt="Disk Sanitization Steps"/><img src="https://i.imgur.com/DohxPGj.png" height="41%" width="41%" alt="Disk Sanitization Steps"/>

  
  -Add "johnadmin" to the “Domain Admins” Security Group. Click on example.com to see the new user. Right click on johnadmin -> Properties -> Member of -> type domain on the blank -> Check names -> Domain Admins. Drag "johnadmin" to "_ADMINS" OU for organizational purposes
  
  <img src="https://i.imgur.com/MuHOHYw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  -Log out/close the Remote Desktop connection to DC-1 and log back in as “example.com\johnadmin”

<img src="https://i.imgur.com/7ZTQFvF.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
  
  -User "johnadmin" as your admin account from now on

- Join Client-1 to your domain (example.com):

 -Login to Client-1 (Remote Desktop) as the original local admin (labuser) and join it to the domain (computer will restart)
 
 -We need to check "Client-1" DNS Server is "DC-1" IP Address (10.0.0.4 in this case). To do this open command prompt and type ipconfig /all
 
  <img src="https://i.imgur.com/XV6OLu5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

 -Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers (ADUC) inside the “Computers” container on the root of the domain
 
 -Create a new OU named “_CLIENTS” and drag Client-1 into there (Step is not really necessary, just for organizational purposes. I guess I skipped this in the lab!)





- Item 4







- Item 5

<img src="https://i.imgur.com/AGKe52p.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
