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

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1:Create Resource Group and VMs (Virutal Machines) in Mircosoft Azure 
- Step 2:Ensure Network connectivity between Domain Controller and Client PC
- Step 3:Setting up Active Directory on Domain Controller and and join Client PC to domain
- Step 4:Create users on Domain Controller and attempt to login with users on Client PC

<h2>Step 1:Create Recource Group and VMs (Virtual Machines) in Mircosoft Azure</h2>

https://github.com/joshuaiharris/configure-ad/assets/155191517/090c697a-69ff-4fa5-8c06-6a7af99786f6

1. Once on the home page in Microsoft Azure, you want to find the Virtual Machines icon. If the icon isn't on the home page, go to the search bar above and type "VM," the Virtual Machines page link should appear under "Services." We will create virtual machines in Azure, The Domain Controller VM, and the Client VM. We will use the Domain Controller VM to install Active Directory and The Client VM to access the resources from the Domain Controller.
  
2. The primary Virtual machine we will create will be our Domain Controller, "DC-1". First, we will create a resource group, "AD-1," which will store all the information for our Domain Controller VM and Client VM within the cloud. Next, choose your location under Region, "no infrastructure redundancy required" for Availability options, and "Standard" for Security type. Use "Windows Server 2022 Datacenter: Azure Editions - x64 Gen2" as the Operating System and "Standard_E2s_V3-2 vcpus, 16 Gib memory" for CPU size. Azure allows four virtual machines to run simultaneously. Therefore, we will utilize the processing speed of two virtual machines, "vcpus," for each VM. Under "Administrator account," you want to create a username and password—for example (Username: DC1; Password: Password1). Finally, Review + Create.

3. You want to repeat the same steps when creating the Client VM, "Client-1". You want to give DC-1 time to deploy its resources, then refresh the page once it's finished. Following this step allows for proper connectivity between both VMs. For Client-1, we want to use the same resource group we created for DC-1, which was AD-1. Doing so enables both VMs to share resources and ensure proper network connectivity. We will use "Windows 10 Pro, version 22H2 -x64 Gen2" as the Operations System. We will make "Client1" our username and copy the "DC-1" password "Password1". Before we create + review, we want to check in to see if "Client-1" is on the same network as "DC-1. Because they are in the same resource group, Client-1's network should reflect DC-1's network by default. Once you have completed that step, we will have finished creating our virtual machines.


<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Once on the home page in Mircosoft Azure, you want to find the Virctual Machines icon. If the icon isnt present on the home page you can go to the search bar above and type in "VM" and the link to the Virtual Machines page should appear under "Services"
<br />

<p>
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
