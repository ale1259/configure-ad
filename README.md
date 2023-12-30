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

  -Set up DNS Server of "Client-1" to be "DC-1" IP Address. For this step we need to go to the Azure Portal -> Virtual Machines ->  Client-1 -> Networking -> Network Interface(click there) -> DNS Servers -> Custom -> DC-1 IP Address -> Save.

 
  -Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher)

- Ensure Connectivity between the client and Domain Controller:

  -Login to Client-1 with Remote Desktop and ping DC-1’s private IP address with ping -t "ip address of DC-1" (perpetual ping).

  <img src="https://i.imgur.com/ExD61na.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>

  -It will say "Requested timed out" and to fix this we need to login to the Domain Controller and enable ICMPv4 in on the local windows Firewall. For this open Windows Defender Firewall -> Advanced Settings -> Inbound Rules and look up for Core Networking Diagnosticos-ICMPv4 Echo Request (ICMPv4-In), enable both right click Enable Rule.

  <img src="https://i.imgur.com/kIJVisq.png" height="29%" width="29%" alt="Disk Sanitization Steps"/><img src="https://i.imgur.com/WVbx9k6.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>


  -Check back at Client-1 to see the ping succeed.

  <img src="https://i.imgur.com/aCDi8pt.png" height="40%" width="40%" alt="Disk Sanitization Steps"/>
  
- Step 3
- Step 4

<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/aCDi8pt.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
