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
<h2>Step 2:Ensure Network connectivity between Domain Controller and Client PC</h2>

https://github.com/joshuaiharris/configure-ad/assets/155191517/eee6c8d9-2a8a-43f6-9f06-008d5bdcd071

1. The next step is to launch Remote Desktop by searching for it on the taskbar located at the bottom of the Desktop page. Windows Remote Desktop allows users to access and control other CPUs remotely. We will need the public IP address for the DC-1 and Client-1 to access both VMs using Remote Desktop.
(Side note: A shortcut would be using Windows commands. Press the Windows tab+R tab, which will launch Windows Run. Windows Run allows you to access applications and information quickly using coded commands. i.e., windows tab+R tab and search MSTSC will launch Remote Desktop within window run)

2. To find the public IP address for our VMs, we must go back to the Microsoft Azure homepage and locate the virtual machine icon. Once we return to the virtual machines page, you want to click on DC-1 and Client-1. The Public IP address will be on the right side of the virtual machines page under Operating Systems. Copy the public IP address by clicking the copy tab next to it, or highlight the IP address and press Ctrl+C. Go back to Remote Desktop and press Ctrl+V or right-click +paste. You want to use the Username and password we set up when we created each virtual machine. It is always best to take notes or reminders of login info so you can maintain organization and not worry about being unable to accomplish the tasks because of lost information.

3. Once successfully logged into both VMs, we want to return to Azure and set the DC-1’s network from dynamic to static. Setting the IP address for the domain controller (DC-1) to static provides stability and predictability. The stability and predictability provide proper functionality when communicating with DNS and DHCP servers; it simplifies firewall security configuration. It helps maintain the integrity of replication in a potential multi-domain controller environment. We can change this IP address by clicking on Network settings located on the left side of the DC-1 page on the virtual machines menu. Click “Network interface/ IP configurations” and click on the IP configuration default settings under “Name.” You want to change the IP address from dynamic to static. Finally, you want to apply the new IP address settings and refresh DC-1.
(Side note: After changing the DC-1 IP configuration from dynamic to static, it's best to check and see if the public IP address has changed. Not following this instruction will affect access to DC-1in Remote Desktop.)  



https://github.com/joshuaiharris/configure-ad/assets/155191517/76cc10dd-d29c-4dd0-b7ed-9a6d4f37e3cc

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
</p>
<br />
<h2>Step 3:Setting up Active Directory on Domain Controller and and join Client PC to domain</h2>

