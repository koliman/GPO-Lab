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
- Install GPMC in Server Manager through Manage > Add Roles and Features
- Select Role-based or feature-based installation
- Select your server to install GPMC on
- Under the Features section, install Group Policy Management
- Open Group Policy Management in the Search box to open it - from the Start Menu, you can find it in the Windows Administrative Tools folder, then click Group Policy Management
- Under Forest, it will show the domain you have created along with the OUs and groups that were created in it
- To edit a Group Policy Object, right-click the GPO and select Edit - this will open up the Group Policy Management Editor (GPME) windows where you can create and eidt policies for your domain

In the GPME, there are Computer Configurations and User Configurations.
- Computer Configurations refer to a policy that will apply to the local computer and will not change per user - this is meant for settings on the computer itself, not for the user logging on to that computer
- User Configurations refer to a policy that will apply to users on the local computer and will apply to any new users in the future on this local computer

Under the Configurations, there are Policies and Preferences folders that can also be expanded to show things like Software Settings, Windows Settings, Administrative Templates, and Control Panel Settings
- Policies can't be changed by users and are enforced by admins and Active Directory - these may include things like password policies and account lockout policies
- Preferences, on the other hand, CAN be changed by users and while a default setting may be selected for it, the user can alter it later if desired - some things that fit preferences better are mapped network drives, printers, and desktop shortcuts

2. Setting up GPOs
3. Create a VM using Windows 10 Enterprise
4. Configure Windows Server
5. Add Computer to Domain
6. Implementing and Testing GPOs
-->
