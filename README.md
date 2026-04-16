<h1 align="center">Active Directory User Management & Troubleshooting Home Lab</h1>

<h2>Description:</h2>

This lab covers core Microsoft Active Directory tasks. You create and manage user accounts, reset passwords, unlock and disable users, and control access with groups and permissions. You also troubleshoot issues like trust relationship errors, building key identity and access management skills.

<h2 align="center">Languages and Technologies Used:</h2>

<div align="center">
 
<b>**`Directory and Identity Services:`**</b>

Active Directory Domain Services (**`AD DS`**)<br>
Active Directory Users & Computers (**`ADUC`**)<br>
Group Policy Management<br>

<b>**`Networking Services:`**</b>

DNS<br>
DHCP<br>
TCP/IP IPv4 Addressing<br>
Static IP Configuration<br>
Host-Only Networking<br>

<b>**`File Services & Access Control:`**</b>

File Server<br>
SMB Sharing<br>
NTFS Permissions<br>
Group Policy Objects<br>

<b>**`Scripting and Tools:`**</b>

PowerShell<br>
</div>

<h2>Environments Used:</h2>

- VirtualBox
- Windows 10 Client (**`21H2`**)
- Windows Server 2022

<h1 align="center">Lab Walk-Through:</h1>
 
<h2 align="center"><strong>Phase I: Environment Setup</strong></h2>

<b>**`Active Directory Purpose & Key Concepts:`**</b>
- The server promoted to a domain controller is the most important system in a company’s network, as it hosts Active Directory.
- Active Directory provides centralized identity and access management.
- A domain controller hosts Active Directory and handles authentication and security policies across the network.
- Active Directory manages authentication, passwords, logins, users, groups, and computers, and controls access through permissions and group policies.
- Group Policy Objects enforce security settings and system configuration.
- Active Directory also supports least privilege access to protect network resources.

<b>**`Lab Overview:`**</b>

This step prepares the server for domain controller deployment by renaming the server, configuring network adapters, and setting a static IP address. A static IP keeps the address stable instead of changing after lease renewal. This setup supports Active Directory, DNS, and DHCP, and maintains reliable network communication across the environment.

<h3 align="center">Server Setup & Domain Controller Configuration:</h3>
<p align="center"> <strong>Renaming the Server:</strong> </p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/64b0fe4f-d9cc-4e6d-9910-b6e19a07274e" /> </p>
 
**`Steps:`**
 - Open Server Manager.
 - Click Local Server on the left panel.
 - Locate the Computer Name field.
 - Click the current computer name and enter the System Properties window, click Change.
 - Enter the new Computer Name and click OK to apply changes.
 - Restart the server when prompted.
<br><br>  
<p align="center"> <strong>Configuring Network Adapters for the Server:</strong> </p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/9a9e9c27-ea0a-4a1b-8402-d452e8960920" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/e559f4ec-497c-4b7f-900a-dac157b7a590" /> </p>

 **`Network Services Configuration:`**
- The Host-Only Adapter is the isolated internal lab network where the Domain Controller and Windows 11 Clients will communicate with each other.
- NAT gives the server internal access, but it's kept isolated from the home network.
<br><br>
<p align="center"> <strong>Identifying IP Range for the Host-Only Network:</strong> </p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/6d28d6aa-75d2-4964-a4ba-4c5a54962c69" /> </p>
 
**`Determining IP Address Range for Static Configuration:`**
- The domain controller must keep a static IP address because all network devices depend on it for communication. Computers, users, and printers connect to it for authentication and resource access.
- It acts as the central control point for the home lab network.
- VirtualBox Host-Only Network uses a defined IP range. The first step is identifying the subnet range assigned to the host-only adapter. This range determines valid IP addresses for the virtual machines and the host system within the isolated network.
<br><br>
<p align="center"> <strong>Configuring DC with a Static IPv4 Address:</strong> </p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/285b3c8a-160d-48ae-a5f5-cec6a8d68302" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/090cb017-73dc-4815-9cb6-b528da504f6c" /> </p>

**`Assigning Static Configuration:`**
 - Open IPv4 settings.
 - Select manual configuration.
 - Enter static IP address, subnet mask, default gateway, and DNS server.
 - Save settings and verify connectivity using ping.
<br><br>
<p align="center"> <strong>Installing AD DS & Promoting Server to DC:</strong> </p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/eab11d16-5ca2-4f2a-a3fe-3fd09b21c8b3" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/07c707b8-6917-42aa-9326-827b5c0877ec" /> </p>

**`Installing Active Directory:`**
- Installed AD DS and promoted the server to a domain controller while creating a new forest and domain, setting the Directory Services Restore Mode password, and enabling DNS installation during promotion.
- After the restart, the system handled authentication, domain logins, and directory services.
- The domain stores user accounts, computers, and security policies while controlling access across the environment.
- This step established the foundation for centralized identity and access management in the lab.
<br><br>
<p align="center"> <strong>Confirmation of Creation of the DC:</strong> </p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/a77db9d3-e4df-471a-a3fe-0504288d91b9" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/683029ae-ab6c-40f9-9367-5ece9824c468" /> </p>
 
**`Key Tasks Completed:`**
- Server renamed to **DC02** to match the domain controller role and rebooted successfully.
- VirtualBox networking configured with Host-Only and NAT adapters to isolate lab traffic and allow controlled external access.
- Host-Only IP range identified for static addressing.
- Domain controller assigned a static IP address **192.168.56.10**, subnet mask **255.255.255.0**, and DNS **192.168.56.10** with manual IPv4 configuration.
- No default gateway was configured because the lab uses an isolated internal network. All communication occurs within the local environment between the Domain Controller and the client system.<br>
- Network settings applied and verified for connectivity.
- Installed Active Directory and promoted the server to a DC.
<br>

----------------

<h2 align="center"><strong>Phase II: Organizational Unit (OU) Design & User Provisioning</strong></h2>

**`Organizational Unit (OU) Design & User Provisioning Key Concepts:`** 
-  OU design follows business structure, such as departments, roles, or locations.
-  User provisioning includes creating and configuring user accounts with the correct attributes and settings.
-  Security groups are used to assign permissions and control access to resources.
-  OU structure supports delegation of administration and simplifies policy application through Group Policy Objects.

<b>**`Lab Overview:`**</b>

This phase builds and manages the Active Directory structure. Organizational Units organize users and computers for easier administration. New user accounts are created and placed into security groups, including admin accounts for elevated privileges. A Windows 10 system is joined to the domain to integrate with Active Directory. A new domain user account is then used to log in and verify authentication and access control.

<h3 align="center">Active Directory Infrastructure Setup:</h3>
<p align="center"> <strong>Creating OUs:</strong> </p> 
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/44a17e7f-d772-4b59-bee7-0db5bd3420f4" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/885f9994-7b3a-4247-a0d6-8169263b087e" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/4d1b5e38-a3e9-4599-8450-66f123605b18" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/c7b5f4ba-83ba-47c2-90d8-01dee3115750" />

**`OU & User Account Structure in Active Directory:`**
- Built-in groups support system functions, while standard users should not be placed inside them due to security and management issues.
- Organizational Units structure Active Directory by separating users and computers for organized administration and Group Policy application.
- To create an OU, open the domain in Active Directory Users and Computers, right-click the domain name, select New, choose Organizational Unit, enter a name, then confirm.
- OUs contain user accounts and groups organized by department, such as HR and Finance.
- This structure improves control, simplifies administration, and supports scalable Group Policy management.
<br><br>
<p align="center"> <strong>Joining a Windows Client to an Active Directory Domain:</strong> </p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/3a3307e2-8412-49f6-8ad9-ab680308cef5" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/9cb42af5-5a08-415a-8f18-e8f4c54fc304" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/deb2cf54-a301-4f5a-9184-ea6df373cc8a" />

**`Domain Integration of a Windows Client with Active Directory:`**
- Adding a client machine to the domain connects it to Active Directory so it can authenticate users through the domain controller.
- This allows centralized login, consistent security policies, and controlled access to network resources.
- It ensures users sign in with domain accounts instead of local accounts, which improves security and simplifies management for administrators.
   
**`Steps:`**
- Set the client DNS to the domain controller IP address.
- Open system settings on the Windows 10 client.
- Go to About or System Properties.
- Select Rename this PC (advanced).
- Choose the Domain option.
- Enter the domain name. Provide domain admin credentials.
- Restart the client machine.
- After reboot, sign in using a domain user account.
<br><br>
<p align="center"> <strong>Enabling Domain User Login on Client Machines</strong> </p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/d1cc46eb-d728-4301-8b85-d142596e5d3a" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/245491b6-0802-43c6-82be-1d7a41d801c9" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/dbcbd970-22d5-40c2-9749-c7a8dc3cccb9" />

**`Key Tasks Completed:`**
- Created Organizational Units to structure Active Directory by department and role.
- Provisioned user accounts **Patricia Johnson & Jim Watkins** and security groups **HR & Finance** within appropriate OUs.
- Followed best practices by avoiding default containers and using dedicated OUs.
- Joined Windows 10 client machine **(CLIENT02)** to the domain **LAB.local** using domain credentials.
- Configured CLIENT02 DNS **192.168.56.10** to point to the domain controller **LAB.local** for proper communication.
- Verified domain integration by logging in with a domain user account.
- Confirmed authentication and access control through the domain controller.
<br>

----------------

<h2 align="center"><strong>Phase III: Group Policy Management</strong></h2>

**`GPOs Key Concepts:`** 
-  Group Policy centralizes configuration for users and computers in an Active Directory domain.
-  GPOs link to sites, domains, or OUs to apply settings based on structure.
-  Security settings include password rules, account lockout policies, and system restrictions.
-  User and computer policies enforce limits such as blocking the Control Panel or Command Prompt.
-  Policy application follows Active Directory inheritance and updates through refresh cycles or gpupdate /force.
-  This approach reduces manual configuration and maintains consistent system settings across the domain.

<b>**`Lab Overview:`** </b>

This phase manages security and settings using Group Policy. GPOs are created and linked to Organizational Units to apply settings to users and computers. Security settings include password rules, account lockout policies, and system settings, including Control Panel access and software installation limits. Client machines are updated and tested to confirm policy application across domain users and systems.

<h3 align="center">Creating and Managing Group Policy Objects (GPOs):</h3>

**`Step 1:`**
<p align="center"> <strong>Open Group Policy Management Console (GPMC):</strong></p>

<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/d5f6afef-92ab-4267-b066-69e5b7936d80" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/91a3e434-729b-4d52-9f6f-51cf396abcee" />

This opens the console used to manage GPOs.
   
**`Steps:`**
- Open Server Manager.
- Click Tools at the top right.
- Select Group Policy Management.
<br>

**`Step 2:`**
<p align="center"> <strong>Navigating the Domain Structure:</strong></p>
 
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/4add2bf9-1b1b-41eb-a45f-7ab3398cf66c" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/680fb357-3011-4fcd-b8bc-e5e20e56092a" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/5f8aa4ee-5ffc-47ae-be02-87aabb72dc73" />

GPOs link to a Site, Domain, or OU.
   
**`Expand:`**
- Forest.
- Domains.
- Click your Domain Name.

**`Decide where you want the policy:`**
- Domain Level (**applies to all**).
- OR specific OU (**Organizational Unit**).
<br>

**`Step 3:`**
<p align="center"> <strong>Creating a New GPO:</strong></p>

<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/ceb9d0af-eb68-4ea7-ad19-4aaec78e56d9" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/9e9484a2-1e6a-4ae9-8864-afce8cef43c3" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/8ff5cf62-1c2e-4f1f-b122-0b6b0e2da0be" />

 <b>There are two ways to create a new GPO.
   - The first option creates the GPO first and links it to a domain or OU later.
   - The second option creates and links the GPO at the same time.
   
**`Method I:`**
   - Click Group Policy Objects.
   - Right-click New.
   - Enter a name.
   - Click OK.

**`Method II:`**
  - Right-click the OU or Domain.
  - Click Create a GPO in this domain, and Link it here.
  - Enter a name.
  - Click OK.
 <br>
 
**`Step 4:`**
<p align="center"> <strong>Edit & Configure the GPO:</strong></p>

<p align="center"> <strong>User Configuration Part 1</strong> </p> <p align="center"> <img width="500" height="500" src="https://github.com/user-attachments/assets/15355fc4-0930-41b1-8cc7-3152a458d673" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/738faf81-80ed-4d85-b26d-b24597460978" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/88d276ec-9219-4906-aec4-771f3403123d" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/6e2782ac-53cb-4e8b-9e04-8d5fe488bff9" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/ec425090-5043-44cd-a70a-f5a1eba96502" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/eae31dbe-119c-440c-a9a0-851d4c8c8510" /> </p> <p align="center"> <strong>User Configuration Part 2</strong> </p> <p align="center"> <img width="500" height="500" src="https://github.com/user-attachments/assets/fb55fa37-10fb-43a1-b983-bf70ba114d36" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/7ab2eabf-dff9-43c1-adc9-2b019d134ca1" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/9fb44df3-3399-46ad-beff-6e06676c2f93" /> </p> <p align="center"> <strong>Computer Configuration Part 1</strong> </p> <p align="center"> <img width="500" height="500" src="https://github.com/user-attachments/assets/26f35b09-ade2-4019-bece-09da589ba157" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/c4db19b2-43c2-45bb-84c4-1632f4240018" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/e9a7d021-87d7-4960-99e5-bcae67c7df27" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/ec985f7c-7a94-4d6b-ae27-7566da294235" /> </p> <p align="center"> <strong>Computer Configuration Part 2</strong> </p> <p align="center"> <img width="500" height="500" src="https://github.com/user-attachments/assets/af9c6661-e493-498a-8a4d-5514dbfa9cd0" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/e3ef3bc4-4799-4779-a395-574b3a64f78f" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/b4e9aeb8-ef06-429a-be21-2512980ec910" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/69ad2293-2547-4dac-948a-aad90204f7b1" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/b7ad1d61-2be8-4b47-ae05-e075b7fa18c9" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/eeec96a3-1a6e-497b-9ede-ce2ddafc12eb" /> <img width="500" height="500" src="https://github.com/user-attachments/assets/dde6a927-0261-4527-b26c-d53b55bebbe3" /> </p>
 
**`Steps:`**
- Right-click the GPO.
- Click Edit.
- **`Configure settings under:`**
  - Computer Configuration (**applies on startup**).
  - User Configuration (**applies on login**).
- **`Navigate inside:`**
  - Policies → Administrative Templates.
  - Double-click any setting.
- **`Choose:`**
  - Enabled.
  - Disabled.
- Click Apply → OK.

Policies enforce rules; users cannot override them.
This GPO applies specific controls to manage user behavior and system security.

**`User Configs:`**
 1. **Prohibit access to Control Panel and Settings** restricts users from changing system configurations.
 2. **Prevent installation from removable media** blocks users from installing unauthorized software from USB devices.
 3. **Removable disks deny write access** stops users from copying or transferring data to external drives.
 4. **Screen saver enforcement** ensures systems lock after inactivity.
 5. **Password-protected screen saver** requires authentication to regain access.
 6. **Screen saver timeout of 550 seconds** locks the system after a set idle period.
 7. **Force specific screen saver** standardizes the screen lock behavior across all users.

**`Computer Configs:`**
 1. **Disable Microsoft Defender Antivirus** turns off built-in antivirus protection for lab control or testing scenarios.
 2. **Disable guest accounts** prevents unauthorized or anonymous access to systems.
 3. **Restricted Groups** define who has local administrator rights, ensuring only approved users have elevated access while standard users, such as HR, do not.
     
**`Overview:`**
- This Group Policy Object enforces user and system security settings across the domain.
- **User Configuration** restricts access to Control Panel and system settings, blocks installation from removable media, and denies write access to removable disks. It also enforces screen saver policies, including password protection, a timeout of 550 seconds, and a specific screen saver.
- **Computer Configuration** disables Microsoft Defender Antivirus and guest accounts to control system access. Restricted Groups are configured to define local administrator membership, ensuring only authorized users have admin rights while standard users, such as HR, are not granted elevated privileges.
<br>

**`Step 5:`**
<p align="center"> <strong>Link the GPO (if not already linked):</strong></p>

<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/91c0e541-9a5d-46d2-981c-f39cdc3f8a99" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/b0656635-e263-4feb-bbf6-5c94e4178bc1" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/8650ec29-3938-4476-897d-8e57cacbc1c8" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/43f4af4e-5cb2-4b22-926e-a8c3a7d7da18" />

 **` Steps: `**
- Right-click OU / Domain.
- Click Link an Existing GPO.
- Select your GPO.
- Click OK.

**`Overview:`**
- Linking the **HR Lab GPO** to the **`Accounts OU`** applies all configured policies to users and computers within that OU.
- All accounts in this OU automatically receive restrictions such as limited system access, removable media controls, and enforced screen lock settings.
- Any new user added to the OU inherits these policies, ensuring consistent and centralized management.
<br>
 
**`Step 6:`**
<p align="center"> <strong>Apply / Update the Policy:</strong></p>

<p align="center">
<img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/5a6cbdea-7a54-45cf-b8c6-fa860fd5b2e7" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/ebffa626-405b-4adc-a374-09c2d8e3275d" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/8817d1dc-06da-4ac0-a247-210e1f856f0b" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/95b131f7-4841-4644-8e81-395ce0725d9a" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/af3593f9-d7c6-42ec-abb8-e5c6771a661d" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/d952509f-0987-4812-97e7-edd01a75de6b" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/a9b7163d-3add-458d-b59a-430d41950bd7" />
 
**`Steps:`**

On Client Machine:
- Open Command Prompt.
- Run:
  - gpupdate /force.
- Restart if required.

Changes don’t apply until policy refresh or reboot.

**`Overview:`**
  - Group Policy changes are applied to the client machine using gpupdate /force. This forces the system to download and apply the latest policies from Active Directory. A restart ensures all settings fully take effect.

**`Key Tasks Completed:`**
- Opened Group Policy Management Console (**GPMC**) from Server Manager.
- Navigated Active Directory structure (**Forest → Domains → LAB.local**).
- Created new Group Policy Objects (**GPOs**) using both creation methods (**standalone and linked creation**).
- Edited GPO settings through Group Policy Management Editor.
- Configured **User Configuration policies**:
  - Prohibit access to Control Panel and Settings.
  - Prevent installation from removable media.
  - Deny write access to removable disks.
  - Enforce screen saver settings.
  - Enable password-protected screen saver.
  - Set screen saver timeout to 550 seconds.
  - Force specific screen saver (**scrnsaver.scr**).
- Configured **Computer Configuration policies**:
  - Disabled Microsoft Defender Antivirus.
  - Disabled Guest accounts.
  - Configured Restricted Groups to control local administrators (**HR users excluded from admin rights**).
- Linked HR Lab GPO to the Accounts OU.
- Verified policy scope by applying GPO at the OU level.
- Forced policy update on client machine using gpupdate /force.
- Restarted the client to apply all Group Policy settings.
- Confirmed policy enforcement across domain-joined systems.
<br>

----------------

<h2 align="center"><strong>Phase IV: File Server and Permissions</strong></h2>

**` File Servers & Security Permissions in Active Directory Key Concepts: `**
- Active Directory uses security groups to control access to shared resources.
- NTFS permissions are assigned to AD users and groups for file and folder access.
- Share permissions manage access to resources over the network.
- Group-based access simplifies permission management and reduces errors.
- Least privilege ensures users only have access to required resources.
- Organizational Units help align users with the correct security groups.
- Permission inheritance applies consistent access control across folders.
- Effective permissions are determined by the combination of NTFS and share permissions.
   
 **`Lab Overview:`**
 
This phase focuses on creating a file server and controlling access using Active Directory security groups. Shared folders are created on the domain controller or a member server. Permissions are applied to restrict or allow access based on user roles such as HR and Finance. This reflects how organizations secure data and enforce least privilege access.

<p align="center"> <strong>File Server Role Installation and Configuration:</strong></p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/eeed713d-4e07-4887-97d5-0b18b0fa44a2" />
  
The File Server role was already installed and verified through Server Manager under File and Storage Services.
  
**`Steps to Install File Server Role:`**
- Open Server Manager.
- Click Manage.
- Select Add Roles and Features.
- Click Next until you reach Server Roles.
- Select File and Storage Services.
- Expand and check the File Server.
- Click Next and then Install.
- Wait for installation to complete.
- Verify the File Server role is installed in Server Manager.
<br>
<p align="center"> <strong>Installing Windows Server Backup:</strong></p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/f340f642-e7f9-4bad-9212-fdfc8488dfec" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/cd6c5150-511d-409c-a430-92c696d12f39" />

A backup drive is created to store copies of the file server data. It helps recover files if data is deleted or damaged. It ensures shared resources are protected and can be restored when needed.
<br>
<p align="center"> <strong>Creating a Backup Drive Using Disk Management:</strong></p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/07fb66af-94b1-4ddf-8d86-fe87cd849911" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/a0afe743-4890-4bde-838a-c5c3b0b516ab" />
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/bf4d27a2-7cf7-4f1a-a706-10b717d380c1" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/ef99be77-2ef6-4005-a4b8-eacff677590e" />
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/6dad557c-3593-41f0-a769-33c772b69839" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/623c6d25-e452-4233-b87f-dd97272f341e" />
 
**`Steps to Install Windows Server Backup:`**
- Open Server Manager.
- Click Manage.
- Select Add Roles and Features.
- Click Next until the Features section.
- Scroll and select Windows Server Backup.
- Click Next, then Install.
- Wait for installation to complete.
- Open Tools and confirm Windows Server Backup is available.
  
**`Steps to Create a Backup Drive Using Disk Management:`**
- Open Disk Management.
- Right-click the main drive (usually C:).
- Select Shrink Volume.
- Enter the amount of space to shrink (example: 20–50 GB).
- Click Shrink.
- Right-click the new unallocated space.
- Select New Simple Volume.
- Click Next.
- Assign a drive letter (D: or E:).
- Name the volume Backup.
- Format as NTFS.
- Click Finish.
- Verify the new drive appears in File Explorer.

**`Steps to Create a Daily Backup Using Windows Server Backup:`**
- Open Server Manager.
- Click Tools.
- Select Windows Server Backup.
- Click Backup Schedule.
- Select Full Server or Custom backup.
- Choose items to back up if Custom is selected.
- Select destination drive (Backup Drive created in Disk Management).
- Choose the Daily Backup schedule.
- Set backup time (example: 2:00 AM).
- Confirm backup destination settings.
- Click Finish to apply the schedule.
- Verify backup job appears in Windows Server Backup console.

**`Overview:`**
- This setup protects server data by enabling scheduled backups and recovery of files, system state, and configurations after failure, corruption, or accidental deletion.
<br>
<p align="center"> <strong>Creating and Configuring Shared Folders:</strong></p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/40c50c92-dae7-4ec7-96c4-69212255eb29" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/8feaa51c-c0b5-4289-afc1-21f82f6875cf" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/74eabb85-f985-4cab-9c41-9710396cca79" />

**`Steps to Create Shared Folders and Set Permissions:`**
- **Open File Explorer:**
  - Go to the data drive (example D:).
  - Right-click and select New → Folder.
  - Create folders (HR, Finance, IT, Accounts).
- **Set Share Permissions:**
  - Right-click folder → Properties.
  - Open the Sharing tab.
  - Click Advanced Sharing.
  - Enable Share this folder.
  - Click Permissions.
  - Assign permissions (Read, Change, Full Control).
  - Click Apply and OK.
      
**`Overview:`**
- Departmental shared folders were created and configured for network access. Permissions were assigned based on role to ensure secure and organized access across the environment.
<br>
<p align="center"> <strong>Creating Shared Folder Organizational Unit (OU) and Security Group Management:</strong></p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/3c78aef8-edc7-49c3-8879-8704af66dea4" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/8f49f78f-cc6e-421e-8c78-64bad1062391" />
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/65d0c525-abd3-4102-a54b-cea95889db73" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/3b956647-3c83-4e6c-ad4b-c651c187c546" />
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/4036a79f-e094-4608-ac6a-388fdbd8b1e2" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/719522c2-c8ac-44f3-8398-7d4612ff27ed" />
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/530770f7-bc21-4330-9ada-e57cb2b4690b" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/327b2356-b521-4933-95b8-96a0e97776f8" />

**`Creating Organizational Unit and Security Groups for Shared Folder Access Control:`**
- Opened Server Manager on the domain controller.
- Launched Active Directory Users and Computers.
- Right-clicked the domain and created a new Organizational Unit named Shared Folders.
- Opened the new OU and created security groups for HR, Finance, IT, and Accounts.
- Set group type to Security and scope to Global.
- Named each group based on department function for clear identification.
- Added user accounts to the matching department security groups.
- Checked each user’s properties to confirm correct group membership.
- Verified the OU structure and groups appeared correctly in Active Directory.
- Prepared the groups for use in shared folder permission assignment.

**`Overview:`**
- An Organizational Unit was created to organize shared resources. Security groups were configured for each department, and users were assigned accordingly, enabling centralized and consistent access control.
<br>
<p align="center"> <strong>Assigning Security Permissions:</strong></p>
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/bdad6646-39c6-470a-aa6e-9f2b8e9daa81" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/e8738a95-67e9-4140-b5e2-610048f97aa1" />
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/16daffe8-b492-4062-aaad-7da239b2a230" /><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/e3fad763-c6ff-4c3e-8bd9-f7bcaf2e24e2" />
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/b239eb7f-adf9-4177-9fc0-218eca17bd94" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/0c570cd1-64c0-43e7-b222-ffa6777a4e6b" />
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/d6c28c0a-d5aa-4960-ae14-1d49e6670121" />
<p align="center"><img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/5488558b-5fe7-43a7-8af1-8361be63d7bb" /><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/1de0ccd2-39f0-4450-9976-118e5e133351" />

**`Assigning NTFS Permissions to the Main Shared Folder and All Subfolders with Inheritance Control:`**
- Opened File Explorer on the file server.
- Navigated to the shared data drive containing department folders.
- Right-clicked a department folder and selected Properties.
- Opened the Security tab to view NTFS permissions.
- Clicked Edit to modify permission entries.
- Added domain security groups for each department.
- Assigned Read, Modify, or Full Control based on role requirements.
- Applied changes and confirmed permission entries were updated.
- Opened Advanced Security Settings to review inheritance configuration.
- Verified permissions propagated to subfolders and files.
- Repeated the process for each department folder shown in the images.
- Confirmed correct group-based access across all shared folders.
- Ensured permissions were assigned only through security groups and not directly to users.

**`Overview:`**
- NTFS permissions were configured on the main shared folder and applied across all subfolders to enforce controlled access. Inheritance was disabled to prevent unwanted parent permissions while retaining SYSTEM and Administrators. Default permissions were removed, and department security groups were assigned specific access levels. This design enforces least privilege and ensures access is controlled through group membership.
<br>
<p align="center"> <strong>Mapping Network Drives Using Group Policy:</strong></p>
<p align="center"><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/e2eacfe0-a387-4881-9317-9493ba0e75ed" /><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/82817d58-e8f5-4507-acaf-a029c9c0f44b" />
<p align="center"><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/8d62824a-e425-4638-a6a1-e2c031e4536f" /><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/2ae5fc16-bd91-4e6f-bf0f-52f7001eed9b" /><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/6cbfa668-8d85-4ec6-bd43-c38fdf961247" /><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/b33b5a37-891b-47c6-ac89-70ada3b7b597" />
<p align="center"><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/41507a48-0235-40cb-9c9a-321e9d416e5f" /><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/ec840f08-aa59-40e7-bdb6-b1738526f96b" /><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/ab81cc1e-b7c6-4373-9a9a-41ed56355e5d" />

**`Mapping Network Drives Using Group Policy for Centralized Access:`**
- Opened Group Policy Management from Server Manager Tools.
- Navigated to the domain and located the appropriate Organizational Unit containing users.
- Right-clicked the OU and selected Create a GPO in this domain and Link it here.
- Named the policy based on the department drive mapping purpose.
- Right-clicked the new GPO and selected Edit to open Group Policy Management Editor.
- Navigated to User Configuration → Preferences → Windows Settings → Drive Maps.
- Right-clicked Drive Maps and selected New → Mapped Drive.
- Set the action to Create to ensure the drive maps for users.
- Entered the shared folder path using UNC format, such as \ServerName\HR.
- Assigned a drive letter for the mapped drive.
- Enabled options such as Reconnect and labeled the drive for clarity.
- Used Item-level targeting to apply the drive map to specific security groups.
- Repeated the process for each department's shared folder shown in the images.
- Closed the editor and ensured the GPO remained linked to the correct OU.
- Verified the mapped drives appeared on user systems after the Group Policy update.

**`Overview:`**
- Group Policy was used to map network drives for department shared folders. A new GPO was created and linked to the appropriate organizational unit. Drive Maps settings were configured under User Configuration to point users to specific shared folder paths using UNC naming. Drive letters were assigned for easy access, and item-level targeting was used to apply each drive to the correct security group. This setup ensured users received automatic access to their department network drives based on group membership.
<br>
<p align="center"> <strong>Verifying Network Drive Access on Client Machine:</strong></p>
<p align="center">
<img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/9ea6a613-4969-4e9f-8156-9c9e23173a3f" /><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/378d8a8f-c153-4e8a-8670-1021c0e1b948" /><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/05979eec-fc60-40fe-a887-1dfbea520cec" /><img width="500" height="500" alt="image" src="https://github.com/user-attachments/assets/25176253-6dac-4575-adaf-1f6c2f6cefce" />

**`Overview:`**
- Network drive access was verified on a client machine using a domain user account. The system successfully applied Group Policy settings, automatically mapping the correct departmental drives in File Explorer. Each mapped drive was accessed to confirm connectivity to the proper shared folders. Permission controls were validated by confirming that access matched the user’s security group while restricting unauthorized folders. This verified that Group Policy, shared folder permissions, and Active Directory security groups were working together correctly.

**`Key Tasks Completed:`**
- Installed and configured the File Server role.
- Implemented Windows Server Backup with scheduled daily backups.
- Created and configured a dedicated backup drive using Disk Management.
- Built departmental shared folders for HR, Finance, IT, and Accounts.
- Configured share permissions for controlled network access.
- Designed and implemented an Organizational Unit for shared resources.
- Created and managed security groups for role-based access control.
- Assigned users to appropriate security groups.
- Configured NTFS permissions using least privilege principles.
- Disabled inheritance and converted permissions to explicit entries.
- Removed default permissions and secured access using group-based controls.
- Applied and verified permission propagation across subfolders.
- Created and linked a Group Policy Object for drive mapping.
- Configured mapped network drives using UNC paths and drive letters.
- Applied item-level targeting based on security group membership.
- Deployed Group Policy settings to client systems.
- Verified mapped drives and enforced access restrictions on client machines.
- Validated integration of Active Directory, Group Policy, NTFS, and share permissions.

This lab demonstrates centralized file access control using Active Directory, NTFS permissions, and Group Policy, ensuring secure, role-based access across the network.

<br>

----------------

<h2 align="center"><strong>Phase V: DHCP Configuration and Network Address Assignment</strong></h2>

**` DHCP Key Concepts: `**
   - DHCP automatically assigns IP addresses to client devices.<br>
   - It removes the need for manual IP configuration.<br>
   - DHCP works closely with DNS to resolve domain names in Active Directory.<br>
   - DHCP scopes define the range of available IP addresses for clients.<br>
   - Lease duration controls how long a device keeps an assigned IP address.<br>
   - DHCP reservations assign a fixed IP to specific devices.<br>
   - DHCP options provide settings like default gateway and DNS server.<br>
   - Active Directory relies on DHCP for consistent network connectivity between clients and the domain.<br><br>
   
**`Lab Overview:`**
  - This phase focuses on core network services that support Active Directory. A DHCP scope is configured to automatically assign IP addresses, subnet masks, default gateway, and DNS settings to client machines. This removes the need for manual network configuration and ensures consistent addressing across the domain. After configuration, client systems receive IP settings automatically from the DHCP server. Connectivity is tested by joining a Windows client to the domain and verifying communication with the domain controller.

<p align="center">Installing DHCP Server <strong>:</strong></p><br>
<p align="center">
<img width="450" height="450" alt="image" src="https://github.com/user-attachments/assets/54063a32-26f9-4d96-b988-9c90f2823bd5" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="450" height="450" alt="image" src="https://github.com/user-attachments/assets/a1ca2eff-9837-4c1f-be96-91a4f332ba3b" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="450" height="450" alt="image" src="https://github.com/user-attachments/assets/795a5d4f-ff27-446c-aea3-6c55ae971238" /><br><br>
 
**` Installing DHCP Server: `**
  - Open Server Manager on the domain controller.<br>
  - Select Add Roles and Features.<br>
  - Choose Role-based installation.<br>
  - Select the correct server (DC02).<br>
  - Select the DHCP Server role.<br>
  - Add required features when prompted.<br>
  - Complete installation wizard.<br>
  - Open DHCP post-install configuration.<br>
  - Authorize the DHCP server in Active Directory.<br>
  - Confirm the DHCP service is running.<br>

**` Overview: `**
  - The DHCP Server role was installed through Server Manager to enable automatic IP address assignment for domain-joined clients. This setup allows the domain controller to distribute network configuration settings such as IP address, subnet mask, gateway, and DNS. It supports consistent connectivity and simplifies network management across the Active Directory environment.<br><br>

<p align="center">Configuring DHCP Scope and Network Options in Active Directory:</strong></p><br>
<p align="center">
<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/607cab9c-2168-442e-8c41-fbfea0440f98" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/da7ef310-4619-4d77-840d-1e3071c55fcd" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/f5e71db1-143b-4fad-ad28-fd597dd5fa86" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/afdcca0d-d0d9-44aa-aed6-e2e2a1a275cc" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/b44ae450-9b93-4604-a12e-1515d939766a" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/93504523-378e-46b5-b784-ad6d20bc53c5" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/6a84c80a-049b-4516-b14f-7be054d70e59" /><br><br>

**` Domain Controller baseline from Phase 1: `**
  - Domain Controller IP: **192.168.56.10**<br>
  - Subnet mask: **255.255.255.0**<br>
  - DNS server: **192.168.56.10**<br>
  
Active Directory Domain Services is already installed and functional.<br>
Host-Only network used for isolated lab communication.<br>
These settings define the core infrastructure DHCP must support.<br>

**` DHCP Scope Configuration (Client Addressing Layer) `**
  - Open DHCP Management Console.<br>
  - Expand IPv4 and create a new scope.<br>
  - Set scope name: Lab Client Scope.<br>
  - Define IP range: **192.168.56.100 to 192.168.56.200**<br>
  - Set subnet mask: **255.255.255.0**<br>
  - Add exclusion range: **192.168.56.10** (Domain Controller static IP).<br>
  - Set lease duration for lab usage, example 8 days.<br>

**` DHCP Options Configuration (Network Services Layer)`**
  - Set default gateway.
      - Leave blank for an isolated Host-Only network.<br>
  - Set only if NAT internet access is required.<br>
  - Set DNS server.<br>
  - **192.168.56.10** (Domain Controller).<br>
  - Set domain name.<br>
  - Match Active Directory domain created in Phase 1.<br>
  - Apply and activate the scope.<br>

**` Overview: `**
  - DHCP was configured on the domain controller using the existing Phase 1 network setup. The Domain Controller used a static IP of 192.168.56.10 with DNS set to itself, supporting Active Directory and domain services within a Host-Only isolated network.<br>
  - A DHCP scope was created to assign client IP addresses from 192.168.56.100 to 192.168.56.200 with a 255.255.255.0 subnet mask. The Domain Controller IP was excluded to prevent conflicts, and a lease duration was set for address management.<br>
  - DHCP options were configured to match the domain environment. The default gateway was left blank due to the isolated network design. DNS was set to 192.168.56.10, and the Active Directory domain name was applied so that clients receive the correct domain configuration automatically.<br><br>

<p align="center">Testing DHCP Configuration on Client Machine:</strong></p><br>
<p align="center">
<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/bde15900-84b0-4026-9495-1ca98820edd9" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/3529e415-68ff-46c6-987f-df3eec74bbe3" /><br>
 <p align="left">
<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/2175c445-0f47-47d6-a077-0b807b6e74fc" />&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/4bd4f186-ddf2-4ab2-a978-356e72228b54" />&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/8670c98a-b4e8-4288-88b2-79be96cb404c" /><br>

**` Overview: `**
  - Client testing confirmed DHCP assigned a valid IP from the 192.168.56.100–200 range. The client received the correct subnet, and the DNS was set to 192.168.56.10. Connectivity to the domain controller and Active Directory services worked as expected.<br>

**` Key Tasks Completed: `**
  - Installed the DHCP Server role using Server Manager.<br>
  - Configured a DHCP server within an Active Directory environment.<br>
  - Created IPv4 DHCP scope for client IP assignment.<br>
  - Defined IP range **192.168.56.100 to 192.168.56.200**<br>
  - Set subnet mask to **255.255.255.0**<br>
  - Excluded Domain Controller static IP **192.168.56.10**<br>
  - Configured lease duration for client address allocation.<br>
  - Set DNS server option to **192.168.56.10**<br>
  - Applied the Active Directory domain name in the DHCP options.<br>
  - Left the default gateway blank due to an isolated Host-Only network.<br>
  - Activated DHCP scope for client distribution.<br>
  - Verified DHCP service functionality in the server console.<br>
  - Tested client machine IP assignment using DHCP.<br>
  - Confirmed correct subnet, DNS, and domain configuration on client.<br>
  - Verified communication between client system and Domain Controller.<br>
  - Confirmed Active Directory connectivity through DHCP configuration.<br>
----------------
