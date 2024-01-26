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
  
2. The primary Virtual machine we will create will be our Domain Controller, "DC-1". First, we will create a resource group, "AD-1," to store all the information for our Domain Controller VM and Client VM within the cloud. Next, choose your location under Region, "no infrastructure redundancy required" for Availability options, and "Standard" for Security type. Use "Windows Server 2022 Datacenter: Azure Editions - x64 Gen2" as the Operating System and "Standard_E2s_V3-2 vcpus, 16 Gib memory" for CPU size. Azure allows four virtual machines to run simultaneously. Therefore, we will utilize the processing speed of two virtual machines, "vcpus," for each VM. Under "Administrator account," you want to create a username and password—for example (Username: DC1; Password: Password1). Finally, Review + Create.

3. You want to repeat the same steps when creating the Client VM, "Client-1". You want to give DC-1 time to deploy its resources, then refresh the page once it's finished. Following this step allows for proper connectivity between both VMs. For Client-1, we want to use the same resource group we created for DC-1, AD-1. Doing so enables both VMs to share resources and ensure proper network connectivity. We will use "Windows 10 Pro, version 22H2 -x64 Gen2" as the Operations System. We will make "Client1" our username and copy the "DC-1" password "Password1". Before we create + review, we want to check in to see if "Client-1" is on the same network as "DC-1. Because they are in the same resource group, Client-1's network should reflect DC-1's network by default. Once you have completed that step, we will have finished creating our virtual machines.


<br />
<h2>Step 2:Ensure Network connectivity between Domain Controller and Client PC</h2>

https://github.com/joshuaiharris/configure-ad/assets/155191517/eee6c8d9-2a8a-43f6-9f06-008d5bdcd071

1. The next step is to launch Remote Desktop by searching for it on the taskbar located at the bottom of the Desktop page. Windows Remote Desktop allows users to access and control other CPUs remotely. We will need the public IP address for the DC-1 and Client-1 to access both VMs using Remote Desktop.
(Side note: A shortcut would be using Windows commands. Press the Windows tab+R tab, which will launch Windows Run. Windows Run allows you to access applications and information quickly using coded commands. i.e., windows tab+R tab and search MSTSC will launch Remote Desktop within Windows Run)

2. To find the public IP address for our VMs, we must go back to the Microsoft Azure homepage and locate the virtual machine icon. Once we return to the virtual machines page, you want to click on DC-1 and Client-1. The Public IP address will be on the right side of the virtual machines page under Operating Systems. Copy the public IP address by clicking the copy tab next to it, or highlight the IP address and press Ctrl+C. Go back to Remote Desktop and press Ctrl+V or right-click +paste. You want to use the Username and password we set up when we created each virtual machine. It is always best to take notes or reminders of login info so you can maintain organization and not worry about being unable to accomplish the tasks because of lost information.

3. Once successfully logged into both VMs, we want to return to Azure and set the DC-1’s NIC from dynamic to static. Setting the IP address for the domain controller (DC-1) to static provides stability and predictability. The stability and predictability provide proper functionality when communicating with DNS and DHCP servers; it simplifies firewall security configuration. It helps maintain the integrity of replication in a potential multi-domain controller environment. We can change this IP address by clicking on Network settings located on the left side of the DC-1 page on the virtual machines menu. Click “Network interface/ IP configurations” and click on the IP configuration default settings under “Name.” You want to change the IP address from dynamic to static. Finally, you want to apply the new IP address settings and refresh DC-1.

(Side note: After changing the DC-1 IP configuration from dynamic to static, it's best to check and see if the public IP address has changed. Not following this instruction will affect access to DC-1in Remote Desktop.)  



https://github.com/joshuaiharris/configure-ad/assets/155191517/76cc10dd-d29c-4dd0-b7ed-9a6d4f37e3cc

4. For the next step, we will log back into Client-1 and attempt to ping DC-1 within the command prompt. This step will inevitably fail because there are default firewall restrictions on DC-1 that we will have to configure. Once we log on to the Client-1 desktop, you want to access the command prompt by going to the taskbar below and searching "command prompt," or we can use Windows+R and search cmd. Once we launch the command prompt, you want to ping the private IP address of the DC-1. You can find the personal IP address by returning to Azure and the DC-1 page. You will find the private IP address once you are on the DC-1 page in Properties under Networking. We want to use the Private IP address because this is an identifier between connected CPUs on the same network. An example of a private IP address would be 10.0.0.4.
 
5. Next, we will copy the Private IP address of DC-1 and return to the command prompt in Client-1. In the command prompt, you want to type "ping -t (Private IP address)," and you will get a continuous "Requested timeout" message. "Ping" is used to check connectivity between CPUs on the same network. "-t" in the command prompt sends a continuous number of packets to the pinged private IP address. Now that we have checked that there is no connectivity between Client-1 and DC-1, we must log in to DC-1. Once logged into DC-1, we want to search "Windows Firewall with Advanced Security" in the search bar. Once we have launched the application, you want to click on "Inbound rules." Next, scroll to the right until you find the category "Protocols" and connect it to organize the protocols so that we can quickly locate what we are looking for. We are looking for the protocol "ICMPv4"; the rules are "Core Networking Diagnostic: ICMP echo request." You want to click both regulations and enable them. Doing so will allow Client-1 to communicate with DC-1 when we ping its private IP address in the command prompt. Now, we must go back to the command prompt in Client-1. You will notice that there is a continuous reply from DC-1. If we execute the following successfully, we have completed this step. 
 
<br />
<h2>Step 3:Setting up Active Directory on Domain Controller and join Client PC to domain</h2>





https://github.com/joshuaiharris/configure-ad/assets/155191517/2fb90268-e86f-44ea-b17d-1ad9e940b236





1. In this next step, we will install Active Directory within our Domain Controller (DC-1). We will log back into DC-1 and launch "Server Manager". Select "Roles and Features" and install "Active Directory and Domain Services. 
After installing Active Directory,  click the flag icon with a yellow warning sign on the top right of the Service Manager page. Click on "Promote this server to a domain controller." Here, we will create a new forest and set up the domain. Setting up a new forest in Active Directory establishes a new, designated directory structure for your organization. You will enter the domain for your organization in the "Root Domain Name" section, i.e., mydomain.com. Then, make a password for your domain controller, essential for security and access control purposes. We will click next until prompted to install. Once installed, DC-1 will restart. Launch Remote Desktop and enter the credits for DC-1. You will notice that the Username no longer works under "DC1," our initial Username. Because we have set the domain for the Domain Controller, we must use the user/domain as our new Username. Our new Username will be DC1@mydomain.com. 




https://github.com/joshuaiharris/configure-ad/assets/155191517/933a85d0-8562-4a90-beae-b13fc79742ce



2. Once we log back into DC-1, we will go to the Server manager dashboard and click "Manage." Proceed by clicking "Active Directory Users and Computers." A folder with the domain name will be on the top left corner. In the folder, we will create three Organisational Units (_employees and _Admins). (Side note: _Empolyees will be for the users created in Powershell in the last step. _Admins are the group with executive permissions that are access allowed by users.)
Next, click on _Admins and create a user. For example, we will create a user called John Doe. Next, set up a user login name and password. The user login name will be john_doe@mydomain.com, and the password will be Password1. By default, "change password after first login" will be prompt. Instead, changing it to "Password never expires" is best for this lab. Right-click on the user and go back to properties. Go to "Member of" to give the user permissions of the Domain Controller. Type in "domain," and It will provide several options. We will choose " domain admin". Making John Doe an Admin will give him exuctive permission to have access to shared information within the Domain Controller. Next, we will log out of DC-1 and back in as John Doe. The username is john_doe@mydomain.com, and the password is Password1.



https://github.com/joshuaiharris/configure-ad/assets/155191517/2823f84d-1208-4068-932d-96500d197be9


3. Next, within the Azure Portal, we must set Client-1's DNS settings to the DC's Private IP address. Doing so is essential because it will allow Client-1 to join the domain mydomain.com. The VNET, a hidden DNS server in Client-1, will randomly search mydomain.com (domain controller) and fail. Using a DC-1 private IP address as a Client-1 DNS server is best because DC-1 already knows what mydomain.com is. 
4. Once in the Azure Portal, we will go to the Client-1 setting, click on Network settings, and then click on DNS servers. There, you will choose the "custom" server, enter the private IP address of DC-1, and then save. Once finished, we must refresh and restart Client 1. We now log back into Client-1 and continue the next step.



https://github.com/joshuaiharris/configure-ad/assets/155191517/7491e82c-b3dc-4ea1-a78b-67f0d4b79758

 5. Once we are logged back into Client-1, we will join Client-1 with mydomain.com. We should type and click settings in the taskbar search and then click System. Once in the System menu, scroll down until you see "About." Scroll down once more  and click " Rename this pc advanced." It will open System properties, and there you will click Change. Under "Member of," we will type the domain mydomain.com. Click ok, and enter the credentials to the user we created, John Doe( username:john_doe@mydomain.com, password: Password1). A prompt will pop up and say welcome to mydomain.com. It will also say the System must restart to apply all the changes. Restart the Client-1. We will log in to Client-1 using Joe Doe's username and password. After successful login, we must reconfigure the permissions in the Remote Desktop settings in Client-1. In the taskbar, search "About your PC". Search "Remote Desktop advanced setting" in the About your PC menu. You will search "domain" and then click on domain users there. All users we will create in DC-1 Powershell can now log into Client-1 remotely. After successful execution, we can log out of Client-1


<h2>Step 4:Create users on Domain Controller and attempt to login with users on Client PC</h2>




https://github.com/joshuaiharris/configure-ad/assets/155191517/67a19993-1b2a-4321-bf36-44fce5026e27


1. For the final step, we will run Powershell ISE to run a script to generate hundreds of users within our Active Directory. Search Powershell ISE within the taskbar search. You must run the application as administrator; you can do this by right-clicking on Powershell ISE and selecting administrator. Running applications as an administrator grants access to more privileges and allows it to change the system settings and files that require administrative access. We will copy and paste a script from my instructor, Josh Madakor, from course careers into Powershell ISE. Once we are in Powershell ISE, we will open a new script page, paste the script we copied from my instructor, and then run the script. 
2. While Powershell ISE generates hundreds of users into Active Directory,  return to Secure Server and click tools, then Active Directory users and computers. Click mydomain.com and then select _Employees. There, you will notice all of the users Powershell ISE has generated. Select a user and repeat the process of setting up a login name like we did with the user John Doe. The script already has all the users for whom it created passwords set to Password1, so we do not have to worry about setting a password. Once the user is created, log out of DC-1 and log in to Client-1 with the newly created user. After successfully logging in, we have completed setting up Active Directory!
