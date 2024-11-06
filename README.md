# Group Policy Objects Lab

## Objective

The Group Policy Objects Lab project was done to deepen understanding of GPOs. This was done through the use of the Group Policy Management Console in a Windows Server to learn about creation of GPOs and implementation of the GPOs on user accounts and computers. Utilizing virtual machines to create a client machine that could join the domain allowed for GPO testing to ensure the GPOs were working as intended. This hands-on project was designed to generate experience with Group Policy Objects and understanding of how they are used in a domain environment.

### Skill Learned

- Creation of group policies
- Implementation of GPOs on users and computers
- Understanding the difference between GPO configurations
- Join client machines to domain

### Tools Used

- Windows Server 2022 to provide a server environment for Active Directory
- Active Directory for domain creation and management
- Group Policy Management Console which allows for GPO creation and implementation
- VMware Workstation Pro for Personal Use for virtual machine creation
- Windows 10 Enterprise to create client machine to add to the domain

## Steps

Group Policy Objects (GPOs) are collections of policies in Active Directory (AD) that can be applied to domains and Organizations Units (OUs). Admins can use GPOs in AD and apply these settings to User and Computers in a domain. Group policies can be applied for many things in a domain, including mapping drives when users log in, restricting access to certain areas/functions of the operating system, or password requirements to ensure user accounts are secure.

Note: This uses a domain that was previously created in <a href="https://github.com/koliman/Active-Directory-Lab">this Active Directory Lab</a>. The instructions here assume that a Windows Server environment with Active Directory is set up already. If you would like to follow along with this lab, it is highly suggested to do the previous lab or create a domain that has some users if you happen to already have a Windows Server set up already.

(Under construction as of 11/4 - Please come back soon)
<!--
1. Install Group Policy Management Console (GPMC)

In order to create and edit group policies, the Group Policy Management Console feature needs to be installed on our server.

- Install GPMC in Server Manager through Manage > Add Roles and Features
- Select Role-based or feature-based installation
- Select your server to install GPMC on
- Under the Features section, install Group Policy Management
- Open Group Policy Management in the Search box to open it - from the Start Menu, you can find it in the Windows Administrative Tools folder, then click Group Policy Management
- Under Forest, it will show the domain you have created along with the OUs and groups that were created in it
- To edit a Group Policy Object, right-click the GPO and select Edit - this will open up the Group Policy Management Editor (GPME) windows where you can create and edit policies for your domain

In the GPME, there are Computer Configurations and User Configurations
- Computer Configurations refer to a policy that will apply to the local computer and will not change per user - this is meant for settings on the computer itself, not for the user logging on to that computer
- User Configurations refer to a policy that will apply to users on the local computer and will apply to any new users in the future on this local computer

Under the Configurations, there are Policies and Preferences folders that can also be expanded to show things like Software Settings, Windows Settings, Administrative Templates, and Control Panel Settings
- Policies can't be changed by users and are enforced by admins and Active Directory - these may include things like password policies and account lockout policies
- Preferences, on the other hand, CAN be changed by users and while a default setting may be selected for it, the user can alter it later if desired - some things that fit preferences better are mapped network drives, printers, and desktop shortcuts

2. Setting up GPOs

In this lab, just a few GPOs will be set up from the myriad of options available to edit. Feel free to look through the other settings or templates to work out what they do and how they can be set!
- Policy 1: Password Policy
- Policy 2: Desktop Wallpaper Policy
- Policy 3: Restrict Access to Control Panel
- Policy 4: Disable USB Storage
- Bonus Challenge for the reader: Account Lockout Policy - Configure a policy for account lockout settings to prevent brute force attacks

3. Create a VM using Windows 10 Enterprise

This step is included because Home editions of Windows can't join a domain, so it will require either Pro or Enterprise editions in order to join the machine to the domain and later implement and test the GPOs that were just created. If you have physical computers that have non-Home editions, feel free to use those instead.

- Download the ISO for your desired OS for the VM - the OS used here is an evaluation license for Windows 10 Enterprise
- Create a new virtual machine for your downloaded ISO, following similar steps for creating a VM for a Windows Server (from the previously linked lab)
  - Select "I will install the OS later", Guest Operating System: Microsoft Windows, Version: Microsoft 10/11 x64, name the virtual machine, and for Specific Disk Capacity: 20 GB
- Right-click the newly created VM and go to Settings
- Select CD/DVD (SATA) and click Use ISO image file, click Browser, then select the downloaded Windows ISO
- Install the OS like installing it on a regular machine
- Once installation is completed, there will be a screen that says "Sign in with Microsoft" - instead of signing in, click the option in the bottom left that says "Domain join instead"
- Assign name and password for the VM and set the security questions
- Once install and set up is complete, the evaluation license should show how long the OS install is valid for in the bottom right of the screen - 90 days is more than enough for lab/practice purposes

4. Configure Windows Server

Before the VM can join the domain, the DNS server needs to be set up on the domain controller and DNS on the client machine. This is done so the client machine can communicate with the domain controller. Best practice for servers is that servers should always have a static IP address. The server that was previously created is currently using Dynamic Host Configuration Protocol (DHCP) to automatically assign it an IP address but that makes it hard for other machines to find and connect to it later (unless the server has a hostname). Giving the server a static IP makes it easy to manage, since you know where it is and know it will not change, and is vital if using the server as a DNS server, which is what this set up step is about.

- Power on the VM for your server and log in
- Right-click the Network icon in the bottom-right corner and select Open Network and Internet settings
- Select Change adapter options under Advanced network settings
- Right-click on Ethernet in Network Connections window and select Properties
- Click on Internet Protocol Version 4 (TCP/IPv4) - the IPv4 Properties should show that the server is using DHCP with "Obtain an IP address automatically"
  - A static IP is necessary in order to use the server as a DNS server - the server's IP address is what will be used for the DNS settings on the client machine
- If a specific IP address is desired, use that IP address for the static IP, but this is also a good opportunity for using Command Prompt and ipconfig - open cmd and enter the ipconfig command (Keep this window open since this information will be used for changing IP address shortly)
- In the IPv4 Properties window, select Use the following IP address and copy the information from the Command Prompt window, filling in the IP address, subnet mask, and default gateway fields
- Since we want to use this server as a DNS server, we can set the Preferred DNS Server IP address to the loopback address, which is 127.0.0.1 - this will point back to this domain controller that is also running our Active Directory tools
  - Under Alternate DNS server, we can set it to something that will have high uptime reliability, like one of Google's DNS servers, which is 8.8.8.8 or 8.8.4.4
- Press OK on all the windows after you finish entering the addresses and other information
- Verify nothing has change on the server VM by going into Command Prompt again and entering ipconfig /all (and verify you still have network connection!)
  - The ipconfig /all command will show the machine's IP address, subnet mask, default gateway, as well as the DNS servers we just entered, which should show ::1 (loopback address for IPv6), 127.0.0.1, and 8.8.8.8 (or 8.8.4.4 if used instead)

5. Add Computer to Domain

After setting up the server, the DNS settings on the client machine need to be updated with the information for the DNS server so that the client machine and domain controller can communicate with each other. Once connection to the domain controller has been established, the client machine can then join the domain.

- If closed/shut down, start up the VM for the client machine
- In the client machine, right-click the Network icon, select Open Network and Internet settings, and then select Change adapter options
- Right-click on Ethernet in Network Connections window and select Properties, then click on IPv4
- For client machines, it's okay to leave DHCP on for IP assignment - the important thing is to change the Preferred DNS Server IP address to the server's IP address, which can easily be retrieved since the server is still running and/or the IP address of the server was written down
  - In Preferred, used the static UP address of the Windows server, and in Alternate, it's always a good idea to use Google's DNS server
- Once IP addresses are entered and settings are saved, verify that the client machine still has network connection
- Next, verify you have connection to the domain controller by opening Command Prompt and use the ping command in combination with the IP address of the server - if it pings successfully, the client machine can communicate with the domain controller
  - Another way to confirm if the DNS server is working is to use the nslookup command with the domain name and it should resolve the DNS and show the domain's IP
- To join the client to the domain, open the File Explorer and then right-click on This PC and select Properties
- Scroll down on Status until you can select Rename this PC (advanced)
- Under the Computer Name tab, there should be an option for "To rename this computer or change its domain or workgroup, click Change" and click on the Change button
- Change the computer name as appropriate and under the Member of area, change it from Workgroup to Domain and enter the domain name in the textbox and then click OK
- If a security window pops up asking for account with permission to join the domain, it's a good sign - enter the admin credentials and upon success, a confirmation window will pop up and welcome the client to the domain
- Restart the client machine to finish the joining process
- Upon restart, the login screen should default to the local user account - to see if other user accounts related to the domain are working, select Other user in the bottom right and verify that under the user name and password boxes, it says "Sign in to: \<domain name you entered>"
- Using one of the previously created accounts on the domain, use their user name and passowrd to log in to the domain using the client computer
  - If you forgot the password (oops!), you can always reset it using Active Directory by right-clicking the user and selecting Reset Password

6. Implementing and Testing GPOs

-->
