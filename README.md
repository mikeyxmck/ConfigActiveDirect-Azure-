# ConfigActiveDirect-Azure
<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Computer)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure
- Ensure Connectivity between the client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1


<h2>Deployment and Configuration Steps</h2>

<p>
<img src="https://i.imgur.com/WOVvZXq.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/jsPvjXM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/GvL36an.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/yPb9ucE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/ytmiXGj.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>

</p>
<p>
1)   Go into portal.azure.com and create a resource group, it will be needed for the Client-1 and Domain Controller.

2)  Then we will go and create the first VM, this will be our Domain Controller (DC-1) and will use the Windows Server 2022. (NOTE: Make sure of the Resource Group and Virtual Network (Vnet) that is being created at this point. Once we are done creating the VM we will go into it, head to Networking settings, and then click on the Network Interface.

3)  Once inside, you will need to click on IP Configurations and then click on 'ipconfig1' to edit the Private IP Address, changing it from Dynamic to Static.
</p>
<br />

<p>
<img src="https://i.imgur.com/1MTqFRt.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/MozuV2h.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/iYStHnp.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/v6MLiSG.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/q4rZkYM.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/Ch5ap8s.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/GJT7UvH.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/i2anVJd.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
4)The next step is creating the Client-1 VM, using the same Resource Group and Vnet that was used in Steps 1-2. This will ensure that we have the same Vnet that will be needed for this tutorial.
  
5)Next, we will log into Client-1 using Remote Desktop and ping DC-1's private IP address through PowerShell with 'ping -t 10.0.0.4' we will be doing a perpetual ping, and you will see that the Ping is not succeeding.

6) Next, we will log in to the Domain Controller (DC-1) and enable ICMPv4 through the local Windows Firewall. (NOTE: I am unable to provide screenshots inside of DC-1)

7) Go back to Client-1 and see if the ping is working, once the connection is enabled, you will need to go back to DC-1 and install Active Directory Domain Services.
8) Once you have it all installed, it will not be a Domain Server yet, you will have to promote it as a DC by setting up a new forest as 'mydomain.com'. After we do that, we will restart the VM and log back into DC-1 as user: 'mydomain.com/labuser''
   
9) Next, we are going to create Admin and Normal Users Accounts in AD (Active Directory), inside of the AD Users and Computers, we will need to create an Organizational Unit called _EMPLOYEES and another OU called _ADMINS. These will let us know what OUs we have created so to not mix them with the ones originally created by the AD.

10)Inside of _ADMINS OU, we will create a user by the name of 'Jane Doe' (same password as labuser) with the username of 'jane_admin'

11) add jane_admin to the "Domain Admins" Security Group. Afterward, we will close the Remote Desktop connection and log back in as "mydomain.com\jane_admin". She will be our admin account for now.




</p>
<br />

<p>
<img src="https://i.imgur.com/mBfkVnB.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/2jHeVHk.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/imOY31O.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/USelj2E.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/xXWXrNl.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/PvDyFzh.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>
<p>
12) Now we are going to join Client-1 to mydomain.com we are going to Client-1's DNS settings and changing it to DC-1's Private IP Address. 

13)Next, you must log back into Client-1 as the original local admin (labuser) and join it to the domain (This will restart the computer) We will go to the settings inside of Client-1 and attempt to add it to DC_1's Domain. 

14)Next you will go back to DC-1's remote desktop and verify if Client-1 shows up in (ADUC) inside the 'Computers' container on the root of the domain. 

15)Create a new OU name "_CLIENTS" and drag Client-1 into it.
</p>

<p>
  <img src="https://i.imgur.com/qaj8U2t.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/3JU27Rd.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/FRgfZPC.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
  <img src="https://i.imgur.com/mblTTCA.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
</p>

<p>
  16) Log in to Client-1 as mydomain.com-jane_admin and open system properties, click on Remote Desktop and allow "domain users" access to the remote desktop. Now we can log in to Client-1 as normal without any administrative user. (NOTE: Normally, we would want to do this with Group Policy that allows you to change MANY systems at once.)
</p>
<br />
