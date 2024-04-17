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

  -Set Domain Controller’s NIC Private IP address to be static. To do this go to "DC-1" in the Azure Portal -> Virtual Machines -> DC-1 -> Left column click on "Networking Settings" -> "Network interface/IP configurations"(click the link) ->  blue link that says "ipconfig1"(click there) -> Allocation -> Static and Save. 
  
  -Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and  the Vnet of "DC-1" in Step 1.

  -Set up DNS Server of "Client-1" to be "DC-1" IP Address. For this step we need to go to the Azure Portal -> Virtual Machines ->  Client-1 -> Networking -> Network interface/IP configuratiopn(click there) -> DNS Servers on the left column after you click  -> Custom -> DC-1 IP Address -> Save. Restart "Client-1"

 
  -Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher)

- Ensure Connectivity between the client and Domain Controller:

  -Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t "ip address of DC-1" (perpetual ping).

  <img src="https://i.imgur.com/HMg4Rrn.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

  -It will say "Requested timed out" and to fix this we need to login to the Domain Controller and enable ICMPv4 in on the local windows Firewall. For this open Windows Defender Firewall -> Advanced Settings -> Inbound Rules and look up for Core Networking Diagnosticos-ICMPv4 Echo Request (ICMPv4-In), enable both right click -> Enable Rule.

  <img src="https://i.imgur.com/6vv0EWz.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>


  -Check back at Client-1 to see the ping succeed.

  <img src="https://i.imgur.com/NBuzgdv.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
  


<h2>Deployment and Configuration Steps of Active Directory</h2>

- Install Active Directory:
 
  -Login to DC-1 and install Active Directory Domain Services. In the Server Manager click on Add roles and features.

<img src="https://i.imgur.com/WqNBtAT.png" height="60%" width="=60%" alt="Disk Sanitization Steps"/>

 -Click next until "Server Roles" section and click on "Active Directory Domain" Services and Add Features continue and install. Wait the installation to complete.

<img src="https://i.imgur.com/YlxT3bm.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
  -Close the installation window and go to notifications and promote this server to a domain controller: Setup a new forest as example.com (can be anything, just remember what it is). Set the password and click next until installation.
  
  <img src="https://i.imgur.com/JF2D03s.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
  
  <img src="https://i.imgur.com/Fy6rMzM.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>

  -It will restart, but if not, restart and then log back into DC-1 as user: "username of the domain\username used to login previously". In my case is "example.com\labuser" .

 <img src="https://i.imgur.com/o838YHq.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
 
- Create an Admin and Normal User Account in AD:
 
  -Go to Active Directory Users and Computers on your search bar, create an Organizational Unit (OU) called “_EMPLOYEES”, one called "_ADMINS" and another called "_CLIENTS"

  <img src="https://i.imgur.com/jwF5pQF.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>

  -Create a new employee named “John Smith” on the "_ADMINS" OU with the username of “johnadmin”, to do this right-click on _ADMINS OU -> New -> User. When creating the passsword uncheck "User must change password at next logon" and check "Password never expires"

  <img src="https://i.imgur.com/95Iegb5.png" height="50%" width="50%" alt="Disk Sanitization Steps"/><img src="https://i.imgur.com/DohxPGj.png" height="41%" width="41%" alt="Disk Sanitization Steps"/>

  
  -Now this user is not an admin yet is in the _ADMINS OU but does not have admins privileges so let's add "johnadmin" to the “Domain Admins” Security Group. Click on _ADMINS OU to see the new user. Right click on johnadmin -> Properties -> Member of -> type domain on the blank -> Check names -> Domain Admins. 
  
  <img src="https://i.imgur.com/1mxFjDq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  -Log out/close the Remote Desktop connection to DC-1 and log back in as “example.com\johnadmin”

<img src="https://i.imgur.com/d0ixsr1.png" height="60%" width="60%" alt="Disk Sanitization Steps"/>
  
  -User "johnadmin" as your admin account from now on

- Join Client-1 to your domain (example.com):

 -Login to Client-1 (Remote Desktop) as the original local admin (labuser)

 -We need to check "Client-1" DNS Server is "DC-1" IP Address (10.0.0.4 in this case). To do this open command prompt and type ipconfig /all
 
  <img src="https://i.imgur.com/XV6OLu5.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
 -Join "Client-1" to our domain "example.com". Right-click on windows logo -> Systems -> "Rename this PC(advanced)" not  Rename this PC -> Change -> Member of -> Click on Domain and type example.com

 <img src="https://i.imgur.com/23Z8svY.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

 -When this window shows off you can use either "johnadmin" or "labuser" accounts. And a welcome window will follow it click Ok as well on the next windows and close to restart the PC and apply the changes 

 <img src="https://i.imgur.com/l8BppWY.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

 -Login to the Domain Controller (Remote Desktop) and verify Client-1 shows up in Active Directory Users and Computers  inside the “Computers” container on the root of the domain and drag "Client-1" to "_CLIENTS" OU just for organizational purposes

 <img src="https://i.imgur.com/neoQS4o.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
 
- Setup Remote Desktop for non-administrative users on Client-1

  -Log into Client-1 as example.com\johnadmin and open System like before when joining "Client-1" to the domain, but this time go to "Remote Desktop". Click on "Select users that can remotly access this PC" -> Add -> type domain on the blank -> Check names-> Select Domain Users -> OK
  
  <img src="https://i.imgur.com/sXXxcN6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
  
  -You can now log into Client-1 as a normal, non-administrative user now
 
  -Normally you’d want to do this with Group Policy that allows you to change MANY systems at once 








- Create a bunch of additional users and attempt to log into "Client-1" with one of the users:

  -Login to DC-1 as "johnadmin"

  -Open PowerShell_ISE as an administrator

  -Create a new File and paste the contents of the script into it (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)

  <img src="https://i.imgur.com/yUBkaGH.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

  -Run the script and observe the accounts being created. You don't need to create 10000 accounts, to try just 10 will be perfect, also the password could be whatever you want it will be the same for the users. Click on the green play button to run the script

  <img src="https://i.imgur.com/ajFmkIT.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

  -You'll see the accounts being created on the blue window. When finished, open ADUC and observe the accounts in the appropriate OU "_EMPLOYEES"

  -Choose any of the accounts created, take note of the name, you can double-click on one of them and go to Account and see the name there as well and 

  -Attempt to log into Client-1 with one of the accounts (take note of the password in the script). If you login without any issue, Congratulations!!!  

   <img src="https://i.imgur.com/Tc42aPY.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>


