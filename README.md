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

This step prepares the server for domain controller deployment by renaming the server, configuring network adapters, and setting a static IP address. A static IP keeps the address stable instead of changing after lease renewal. This setup supports Active Directory, DNS, and DHCP and maintains reliable network communication across the environment.

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
- After restart, the system handled authentication, domain logins, and directory services.
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
- OUs contain user accounts and groups organized by department such as HR and Finance.
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

<h3 align="center"><strong>Phase III: Group Policy Management</strong></h3>

**` GPOs Key Concepts: `** 
  -  Group Policy provides centralized control over users and computers in an Active Directory domain.<br>
  -  Group Policy Objects apply settings to users or computers based on OU structure.<br>
  -  Policies control security settings, password rules, and account lockout behavior.<br>
  -  User and computer restrictions enforce system limits such as blocking Control Panel or Command Prompt.<br>
  -  GPOs apply through domain and OU linking for targeted management.<br>
  -  Policy updates occur through refresh cycles or manual update using gpupdate /force.<br>
  -  Group Policy reduces manual configuration and ensures consistent system settings across all machines.<br><br>

**` Lab Overview: `** 
-  This phase focuses on managing security and configuration across the Active Directory domain using Group Policy. Group Policy Objects are created and linked to Organizational Units to apply centralized settings to users and computers. Security policies such as password requirements, account lockout rules, and system restrictions are configured to enforce consistent security standards. Policies are tested by updating client machines and verifying that settings apply to domain users and systems, ensuring centralized control and enforcement across the network.<br><br>

**`Creating and Managing Group Policy Objects (GPOs):`** <br/>

<p align="center"> <strong> Step 1:</strong></p><br>

<p align="center"> <strong>Open Group Policy Management Console (GPMC):</strong></p><br>

<p align="center">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/d5f6afef-92ab-4267-b066-69e5b7936d80" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/91a3e434-729b-4d52-9f6f-51cf396abcee" /><br><br>
<b>This opens the main console where all GPOs are managed.</b>
   
**` Steps: `**
   - Open Server Manager.<br>
   - Click Tools at the top right.<br>
   - Select Group Policy Management.<br><br>

<p align="center"> <strong>Step 2:</strong></p><br>

<p align="center"> <strong>Navagating to Domain:</strong></p><br>
 
<p align="center">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/4add2bf9-1b1b-41eb-a45f-7ab3398cf66c" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/680fb357-3011-4fcd-b8bc-e5e20e56092a" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/5f8aa4ee-5ffc-47ae-be02-87aabb72dc73" /><br><br>
<b>GPOs are always linked to Site, Domain, or OU.</br>
   
**` Expand: `**<br>
   - Forest.<br>
   - Domains.<br>
   - Click your Domain Name.<br>

**` Decide where you want the policy: `**<br>
  - Domain Level (**applies to all**).<br>
  - OR specific OU (**Organizational Unit**).<br><br>

<p align="center"> <strong>Step 3:</strong></p><br>
<p align="center"> <strong>Creating a New GPO:</strong></p><br>

<p align="center">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/ceb9d0af-eb68-4ea7-ad19-4aaec78e56d9" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/9e9484a2-1e6a-4ae9-8864-afce8cef43c3" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/8ff5cf62-1c2e-4f1f-b122-0b6b0e2da0be" /><br><br>
 <b>There are two ways to create a new GPO.</br> 
 
- <b>The first option creates the GPO first and links it to a domain or OU later.</b><br>
- <b>The second option creates and links the GPO at the same time.</b><br>
   
**` Method I: `**
   - Click Group Policy Objects.<br>
   - Right click New.<br>
   - Enter a name.<br>
   - Click OK.<br>

**` Method II: `**
  - Right click the OU or Domain.<br>
  - Click Create a GPO in this domain, and Link it here.<br>
  - Enter a name.<br>
  - Click OK.<br><br>
 
<p align="center"> <strong>Step 4:</strong></p><br>
<p align="center"> <strong>Edit & Configure the GPO:</strong></p><br>

<p align="center">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/15355fc4-0930-41b1-8cc7-3152a458d673" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/738faf81-80ed-4d85-b26d-b24597460978" /<br><br>
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/88d276ec-9219-4906-aec4-771f3403123d" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/6e2782ac-53cb-4e8b-9e04-8d5fe488bff9" /><br><br>
 <p align="left">
<img width="400" height="900" alt="Image" src="https://github.com/user-attachments/assets/52145cf0-28ae-4550-bced-192c7830d085" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/77632a7c-1497-4063-a914-108c515f3c78" /><br><br>
 <p align="center">
<img width="400" height="900" alt="Image" src="https://github.com/user-attachments/assets/1cfc50df-c360-4d85-b4c4-f8a5dba8a9c2" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/ec425090-5043-44cd-a70a-f5a1eba96502" /><br><br>
 <p align="left">
<img width="400" height="900" alt="Image" src="https://github.com/user-attachments/assets/eae31dbe-119c-440c-a9a0-851d4c8c8510" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/fb55fa37-10fb-43a1-b983-bf70ba114d36" /><br><br>
 <p align="center">
<img width="600" height="400" alt="Image" src="https://github.com/user-attachments/assets/7ab2eabf-dff9-43c1-adc9-2b019d134ca1" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/9fb44df3-3399-46ad-beff-6e06676c2f93" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/26f35b09-ade2-4019-bece-09da589ba157" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/c4db19b2-43c2-45bb-84c4-1632f4240018" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/e9a7d021-87d7-4960-99e5-bcae67c7df27" /><br><br>
  <p align="left">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/ec985f7c-7a94-4d6b-ae27-7566da294235" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/af9c6661-e493-498a-8a4d-5514dbfa9cd0" /><br><br>
 <p align="center">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/e3ef3bc4-4799-4779-a395-574b3a64f78f" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/b4e9aeb8-ef06-429a-be21-2512980ec910" /><br><br>
  <p align="left">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/69ad2293-2547-4dac-948a-aad90204f7b1" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/b7ad1d61-2be8-4b47-ae05-e075b7fa18c9" /><br><br>
   <p align="center">
 <img width="900" height="900" alt="Image" src="https://github.com/user-attachments/assets/eeec96a3-1a6e-497b-9ede-ce2ddafc12eb" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="900" height="900" alt="Image" src="https://github.com/user-attachments/assets/dde6a927-0261-4527-b26c-d53b55bebbe3" /><br><br>
 
**` Steps: `**
   - Right-click the GPO.<br>
   - Click Edit.<br>
   - **`Configure settings under:`**<br>
       - Computer Configuration (**applies on startup**).<br>
       - User Configuration (**applies on login**).<br>
   - **`Navigate inside:`**<br>
       - Policies → Administrative Templates.<br>
   - Double-click any setting.<br>
   - **`Choose:`**<br>
       - Enabled.<br>
       - Disabled.<br>
   - Click Apply → OK.<br>

**` Policies enforce rules; users cannot override them `**.<br>
   
**` This GPO applies specific controls to manage user behavior and system security `**.<br>

User Configs:<br>
1. **Prohibit access to Control Panel and Settings** restricts users from changing system configurations.<br>
2. **Prevent installation from removable media** blocks users from installing unauthorized software from USB devices.<br>
3. **Removable disks deny write access** stops users from copying or transferring data to external drives.<br>
4. **Screen saver enforcement** ensures systems lock after inactivity.<br>
5. **Password-protected screen saver** requires authentication to regain access.<br>
6. **Screen saver timeout of 550 seconds** locks the system after a set idle period.<br>
7. **Force specific screen saver** standardizes the screen lock behavior across all users.<br>

Computer Configs:<br> 
1. **Disable Microsoft Defender Antivirus** turns off built-in antivirus protection for lab control or testing scenarios.<br>
2. **Disable guest accounts** prevents unauthorized or anonymous access to systems.<br>
3. **Restricted Groups** define who has local administrator rights, ensuring only approved users have elevated access while standard users, such as HR, do not.<br>
     
**` Overview: `**
- This Group Policy Object enforces user and system security settings across the domain.<br>
- **User Configuration** restricts access to Control Panel and system settings, blocks installation from removable media, and denies write access to removable disks. It also enforces screen saver policies, including password protection, a timeout of 550 seconds, and a specific screen saver.<br>
- **Computer Configuration** disables Microsoft Defender Antivirus and guest accounts to control system access. Restricted Groups are configured to define local administrator membership, ensuring only authorized users have admin rights while standard users, such as HR, are not granted elevated privileges.<br><br>

<p align="center"> <strong>Step 5:</strong></p><br>
<p align="center"> <strong>Link the GPO (if not already linked):</strong></p><br>

<p align="center">
<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/91c0e541-9a5d-46d2-981c-f39cdc3f8a99" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/b0656635-e263-4feb-bbf6-5c94e4178bc1" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/8650ec29-3938-4476-897d-8e57cacbc1c8" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/43f4af4e-5cb2-4b22-926e-a8c3a7d7da18" /><br><br>

**` Overview: `**
  - Linking the **HR Lab GPO** to the **`Accounts OU`** applies all configured policies to users and computers within that OU.<br>
  - All accounts in this OU automatically receive restrictions such as limited system access, removable media controls, and enforced screen lock settings.<br>
  - Any new user added to the OU inherits these policies, ensuring consistent and centralized management.<br>
 
**` Steps: `**
- Right-click OU / Domain.<br>
- Click Link an Existing GPO.<br>
- Select your GPO.<br>
- Click OK.<br><br>

<p align="center"> <strong>Step 6:</strong></p><br>
<p align="center"> <strong>Apply / Update the Policy:</strong></p><br>

<p align="center">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/5a6cbdea-7a54-45cf-b8c6-fa860fd5b2e7" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/ebffa626-405b-4adc-a374-09c2d8e3275d" /><br><br>
 <p align="left">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/8817d1dc-06da-4ac0-a247-210e1f856f0b" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/95b131f7-4841-4644-8e81-395ce0725d9a" /><br><br>
  <p align="center">
 <img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/af3593f9-d7c6-42ec-abb8-e5c6771a661d" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/d952509f-0987-4812-97e7-edd01a75de6b" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/a9b7163d-3add-458d-b59a-430d41950bd7" />
 
**` Steps: `**

On Client Machine:<br>
  - Open Command Prompt.<br>
  - Run:<br>
      - gpupdate /force.<br>
  - Restart if required.<br>

**`Changes don’t apply until policy refresh or reboot`**.<br>

**` Overview: `**
  - Group Policy changes are applied to the client machine using gpupdate /force. This forces the system to download and apply the latest policies from Active Directory. A restart ensures all settings fully take effect.<br>

**` Key Tasks Completed: `**
  - Opened Group Policy Management Console (**GPMC**) from Server Manager.<br>
  - Navigated Active Directory structure (**Forest → Domains → LAB.local**).
  - Created new Group Policy Objects (**GPOs**) using both creation methods (**standalone and linked creation**).
  - Edited GPO settings through Group Policy Management Editor.<br>
  - Configured **User Configuration policies**:<br>
      - Prohibit access to Control Panel and Settings.<br>
      - Prevent installation from removable media.<br>
      - Deny write access to removable disks.<br>
      - Enforce screen saver settings.<br>
      - Enable password-protected screen saver.<br>
      - Set screen saver timeout to 550 seconds.<br>
      - Force specific screen saver (**scrnsaver.scr**).
  - Configured **Computer Configuration policies**:
     - Disabled Microsoft Defender Antivirus.<br>
     - Disabled Guest accounts.<br>
     - Configured Restricted Groups to control local administrators (**HR users excluded from admin rights**).
  - Linked HR Lab GPO to the Accounts OU.<br>
  - Verified policy scope by applying GPO at OU level.<br>
  - Forced policy update on client machine using gpupdate /force.<br>
  - Restarted client to apply all Group Policy settings.<br>
  - Confirmed policy enforcement across domain-joined systems.<br><br>
----------------
  <h3 align="center"><strong>Phase IV: File Server and Permissions</strong></h3>

 **` File Servers & Security Permissions in Active Directory Key Concepts: `**
   - Active Directory uses security groups to control access to shared resources.<br>
   - NTFS permissions are assigned to AD users and groups for file and folder access.<br>
   - Share permissions manage access to resources over the network.<br>
   - Group-based access simplifies permission management and reduces errors.<br>
   - Least privilege ensures users only have access to required resources.<br>
   - Organizational Units help align users with the correct security groups.<br>
   - Permission inheritance applies consistent access control across folders.<br>
   - Effective permissions are determined by the combination of NTFS and share permissions.<br><br>
   
   **`Lab Overview:`**
  - This phase focuses on creating a file server and controlling access using Active Directory security groups. Shared folders are created on the domain controller or a member server. Permissions are applied to restrict or allow access based on user roles such as HR and Finance. This demonstrates how organizations secure data and enforce least privilege access.<br><br>

 <p align="center"> <strong>File Server Role Installation and Configuration:</strong></p><br>
 <p align="center">
<img width="700" height="700" alt="Image" src="https://github.com/user-attachments/assets/eeed713d-4e07-4887-97d5-0b18b0fa44a2" /><br>
<b>The File Server role was already installed and verified through Server Manager under File and Storage Services.</b><br>
  
**`Steps to Install File Server Role:`**
  - Open Server Manager.<br>
  - Click Manage.<br>
  - Select Add Roles and Features.<br>
  - Click Next until you reach Server Roles.<br>
  - Select File and Storage Services.<br>
  - Expand and check File Server.<br>
  - Click Next and then Install.<br>
  - Wait for installation to complete.<br>
  - Verify File Server role is installed in Server Manager.<br><br>

<p align="center"> <strong>Installing Windows Server Backup:</strong></p><br>
<p align="center">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/f340f642-e7f9-4bad-9212-fdfc8488dfec" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/cd6c5150-511d-409c-a430-92c696d12f39" /><br>

 A backup drive is created to store copies of file server data. It helps recover files if data is deleted or damaged. It ensures shared resources are protected and can be restored when needed.<br><br>

 <p align="center"> <strong>Creating a Backup Drive Using Disk Management:</strong></p><br>
 <p align="center">
<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/07fb66af-94b1-4ddf-8d86-fe87cd849911" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/a0afe743-4890-4bde-838a-c5c3b0b516ab" /><br>
<p align="left">
<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/bf4d27a2-7cf7-4f1a-a706-10b717d380c1" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/ef99be77-2ef6-4005-a4b8-eacff677590e" />
<p align="center">
<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/6dad557c-3593-41f0-a769-33c772b69839" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/623c6d25-e452-4233-b87f-dd97272f341e" />
 
 **` Steps to Install Windows Backup Server: `**
  - Open Server Manager.<br>
  - Click Manage.<br>
  - Select Add Roles and Features.<br>
  - Click Next until Features section.<br>
  - Scroll and select Windows Server Backup.<br>
  - Click Next then Install.<br>
  - Wait for installation to complete.<br>
  - Open Tools and confirm Windows Server Backup is available.<br><br>
  
 **` Steps to Create a Backup Drive Using Disk Management: `**
  - Open Disk Management.<br>
  - Right-click the main drive (usually C:).<br>
  - Select Shrink Volume.<br>
  - Enter amount of space to shrink (example: 20–50 GB).<br>
  - Click Shrink.<br>
  - Right-click the new unallocated space.<br>
  - Select New Simple Volume.<br>
  - Click Next.<br>
  - Assign a drive letter (D: or E:).<br>
  - Name the volume Backup.<br>
  - Format as NTFS.<br>
  - Click Finish.<br>
  - Verify the new drive appears in File Explorer.<br><br>

**` Steps to Create a Daily Backup Using Windows Server Backup: `**
  - Open Server Manager.<br>
  - Click Tools.<br>
  - Select Windows Server Backup.<br>
  - Click Backup Schedule.<br>
  - Select Full Server or Custom backup.<br>
  - Choose items to back up if Custom is selected.<br>
  - Select destination drive (Backup Drive created in Disk Management).<br>
  - Choose Daily Backup schedule.<br>
  - Set backup time (example: 2:00 AM).<br>
  - Confirm backup destination settings.<br>
  - Click Finish to apply schedule.<br>
  - Verify backup job appears in Windows Server Backup console.<br><br>

**` Overview: `**
  - This phase focuses on setting up backup and recovery for the file server. Windows Server Backup was installed to enable system protection. A backup drive was created using Disk Management by shrinking existing storage and formatting a new NTFS volume. A daily backup schedule was configured to capture system state or selected data. This ensures critical server data and configurations can be restored after failure, corruption, or accidental deletion.<br><br>

<p align="center"> <strong>Creating and Configuring Shared Folders:</strong></p><br>
<p align="center">
<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/40c50c92-dae7-4ec7-96c4-69212255eb29" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/8feaa51c-c0b5-4289-afc1-21f82f6875cf" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/74eabb85-f985-4cab-9c41-9710396cca79" /><br><br>

**`Steps to Create Shared Folders and Set Permissions:`**
  - **Open File Explorer:**
      - Go to the data drive (example D:).<br>
      - Right-click and select New → Folder.<br>
      - Create folders (HR, Finance, IT, Accounts).<br>
  - **Set Share Permissions:**
      - Right-click folder → Properties.<br>
      - Open Sharing tab.<br>
      - Click Advanced Sharing.<br>
      - Enable Share this folder.<br>
      - Click Permissions.<br>
      - Assign permissions (Read, Change, Full Control).<br>
      - Click Apply and OK.<br><br>
      
**` Overview: `**
  - Shared folders were created on the data drive for departmental use, including HR, Finance, IT, and Accounts. Each folder was configured for network sharing through Advanced Sharing settings. Access permissions were assigned to control user access based on department roles, ensuring secure and organized file access across the network.<br><br>

<p align="center"> <strong>Creating Shared Folder Organizational Unit (OU) and Security Group Management:</strong></p><br>
<p align="center">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/3c78aef8-edc7-49c3-8879-8704af66dea4" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/8f49f78f-cc6e-421e-8c78-64bad1062391" /><br>
 <p align="left">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/65d0c525-abd3-4102-a54b-cea95889db73" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/3b956647-3c83-4e6c-ad4b-c651c187c546" /><br>
 <p align="center">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/4036a79f-e094-4608-ac6a-388fdbd8b1e2" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/719522c2-c8ac-44f3-8398-7d4612ff27ed" /><br>
 <p align="left">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/530770f7-bc21-4330-9ada-e57cb2b4690b" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/327b2356-b521-4933-95b8-96a0e97776f8" /><br><br>

**`Creating Organizational Unit and Security Groups for Shared Folder Access Control:`**
  - Opened Server Manager on the domain controller.<br>
  - Launched Active Directory Users and Computers.<br>
  - Right-clicked the domain and created a new Organizational Unit named Shared Folders.<br>
  - Opened the new OU and created security groups for HR, Finance, IT, and Accounts.<br>
  - Set group type to Security and scope to Global.<br>
  - Named each group based on department function for clear identification.<br>
  - Added user accounts into the matching department security groups.<br>
  - Checked each user’s properties to confirm correct group membership.<br>
  - Verified the OU structure and groups appeared correctly in Active Directory.<br>
  - Prepared the groups for use in shared folder permission assignment.<br>

**` Overview: `**
  - An Organizational Unit (OU) was created to organize shared folder resources within Active Directory. Security groups were then created and managed to control access for different departments. Users were assigned to the appropriate groups, allowing centralized permission management for shared resources and supporting consistent access control across the environment.<br><br>
 
<p align="center"> <strong>Assigning Security Permissions:</strong></p><br>
<p align="center">
<img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/bdad6646-39c6-470a-aa6e-9f2b8e9daa81" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/e8738a95-67e9-4140-b5e2-610048f97aa1" /><br>
<p align="left">
<img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/16daffe8-b492-4062-aaad-7da239b2a230" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/e3fad763-c6ff-4c3e-8bd9-f7bcaf2e24e2" /><br>
<p align="center">
<img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/b239eb7f-adf9-4177-9fc0-218eca17bd94" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/0c570cd1-64c0-43e7-b222-ffa6777a4e6b" /><br>
<p align="center">
<img width="900" height="900" alt="Image" src="https://github.com/user-attachments/assets/d6c28c0a-d5aa-4960-ae14-1d49e6670121" /><br><br>
<p align="left">
 <img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/5488558b-5fe7-43a7-8af1-8361be63d7bb" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/1de0ccd2-39f0-4450-9976-118e5e133351" /><br>

**` Assigning NTFS Permissions to the Main Shared Folder and All Subfolders with Inheritance Control: `**
  - Opened File Explorer on the file server.<br>
  - Navigated to the shared data drive containing department folders.<br>
  - Right-clicked a department folder and selected Properties.<br>
  - Opened the Security tab to view NTFS permissions.<br>
  - Clicked Edit to modify permission entries.<br>
  - Added domain security groups for each department.<br>
  - Assigned Read, Modify, or Full Control based on role requirements.<br>
  - Applied changes and confirmed permission entries were updated.<br>
  - Opened Advanced Security Settings to review inheritance configuration.<br>
  - Verified permissions propagated to subfolders and files.<br>
  - Repeated the process for each department folder shown in the images.<br>
  - Confirmed correct group based access across all shared folders.<br>

**` Overview: `**
  - NTFS permissions were configured on the main shared folder and applied across all subfolders to enforce controlled access. Inheritance was disabled to prevent unwanted parent permissions, while essential entries such as SYSTEM and Administrators were retained. Unnecessary default permissions were removed, and department security groups were added with specific access levels based on role requirements. Permissions were verified to ensure consistent and secure access across all shared folders.<br><br>

<p align="center"> <strong>Mapping Network Drives Using Group Policy:</strong></p><br>
<p align="center">
<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/e2eacfe0-a387-4881-9317-9493ba0e75ed" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/82817d58-e8f5-4507-acaf-a029c9c0f44b" /><br>
<p align="left">
<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/8d62824a-e425-4638-a6a1-e2c031e4536f" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/2ae5fc16-bd91-4e6f-bf0f-52f7001eed9b" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/6cbfa668-8d85-4ec6-bd43-c38fdf961247" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/b33b5a37-891b-47c6-ac89-70ada3b7b597" /><br>
<p align="center">
<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/41507a48-0235-40cb-9c9a-321e9d416e5f" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/ec840f08-aa59-40e7-bdb6-b1738526f96b" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/ab81cc1e-b7c6-4373-9a9a-41ed56355e5d" /><br><br>

**` Mapping Network Drives Using Group Policy for Centralized Access: `**
  - Opened Group Policy Management from Server Manager Tools.<br>
  - Navigated to the domain and located the appropriate Organizational Unit containing users.<br>
  - Right-clicked the OU and selected Create a GPO in this domain and Link it here.<br>
  - Named the policy based on the department drive mapping purpose.<br>
  - Right-clicked the new GPO and selected Edit to open Group Policy Management Editor.<br>
  - Navigated to User Configuration → Preferences → Windows Settings → Drive Maps.<br>
  - Right-clicked Drive Maps and selected New → Mapped Drive.<br>
  - Set the action to Create to ensure the drive maps for users.<br>
  - Entered the shared folder path using UNC format such as \ServerName\HR.<br>
  - Assigned a drive letter for the mapped drive.<br>
  - Enabled options such as Reconnect and labeled the drive for clarity.<br>
  - Used Item-level targeting to apply the drive map to specific security groups.<br>
  - Repeated the process for each department shared folder shown in the images.<br>
  - Closed the editor and ensured the GPO remained linked to the correct OU.<br>
  - Verified the mapped drives appeared on user systems after Group Policy update.<br>

**` Overview: `**
  - Group Policy was used to map network drives for department shared folders. A new GPO was created and linked to the appropriate organizational unit. Drive Maps settings were configured under User Configuration to point users to specific shared folder paths using UNC naming. Drive letters were assigned for easy access, and item level targeting was used to apply each drive to the correct security group. This setup ensured users received automatic access to their department network drives based on group membership.<br>

<p align="center"> <strong>Verifying Network Drive Access on Client Machine:</strong></p><br>
<p align="center">
<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/9ea6a613-4969-4e9f-8156-9c9e23173a3f" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/378d8a8f-c153-4e8a-8670-1021c0e1b948" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/05979eec-fc60-40fe-a887-1dfbea520cec" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/25176253-6dac-4575-adaf-1f6c2f6cefce" /><br>

**` Overview: `**
  - Network drive access was verified on a client machine using a domain user account. The system successfully applied Group Policy settings, automatically mapping the correct departmental drives in File Explorer. Each mapped drive was accessed to confirm connectivity to the proper shared folders. Permission controls were validated by confirming access matched the user’s security group while restricting unauthorized folders. This verified that Group Policy, shared folder permissions, and Active Directory security groups were working together correctly.<br>

**` Key Tasks Completed: `**
  - Installed and verified the File Server role in Server Manager.<br>
  - Installed Windows Server Backup feature for system protection.<br>
  - Created a dedicated backup drive using Disk Management.<br>
  - Configured a daily backup schedule using Windows Server Backup.<br>
  - Created departmental shared folders on the data drive (HR, Finance, IT, Accounts).<br>
  - Enabled file sharing using Advanced Sharing settings.<br>
  - Configured share permissions for controlled network access.<br>
  - Created an Organizational Unit in Active Directory for shared resources.<br>
  - Created and configured security groups for each department.<br>
  - Added users to correct security groups for role based access.<br>
  - Verified group membership inside Active Directory Users and Computers.<br>
  - Configured NTFS permissions on shared folders.<br>
  - Disabled inheritance on the main shared folder.<br>
  - Converted inherited permissions into explicit permissions.<br>
  - Removed default unnecessary permissions such as Users and Authenticated Users.<br>
  - Retained SYSTEM and Administrators for system control.<br>
  - Applied department security groups with Read, Modify, or Full Control access.<br>
  - Propagated and verified permissions across subfolders.<br>
  - Created and linked a Group Policy Object for drive mapping.<br>
  - Configured Drive Maps under User Configuration in Group Policy.<br>
  - Assigned UNC paths for each departmental shared folder.<br>
  - Assigned drive letters for mapped network drives.<br>
  - Enabled reconnect and labeling options for usability.<br>
  - Applied item level targeting for security group based mapping.<br>
  - Updated and applied Group Policy settings to client machines.<br>
  - Verified mapped network drives on a client system.<br>
  - Tested access to confirm correct permissions and restrictions.<br>
  - Confirmed integration of Active Directory, Group Policy, NTFS, and share permissions.<br><br>
----------------
<h3 align="center"><strong>Phase V: DHCP Configuration and Network Address Assignment</strong></h3>

**` DHCP Key Concepts: `**
   - DHCP automatically assigns IP addresses to client devices.<br>
   - It removes the need for manual IP configuration.<br>
   - DHCP works closely with DNS to resolve domain names in Active Directory.<br>
   - DHCP scopes define the range of available IP addresses for clients.<br>
   - Lease duration controls how long a device keeps an assigned IP address.<br>
   - DHCP reservations assign a fixed IP to specific devices.<br>
   - DHCP options provide settings like default gateway and DNS server.<br>
   - Active Directory relies on DHCP for consistent network connectivity between clients and domain.<br><br>
   
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
  - Select DHCP Server role.<br>
  - Add required features when prompted.<br>
  - Complete installation wizard.<br>
  - Open DHCP post-install configuration.<br>
  - Authorize DHCP server in Active Directory.<br>
  - Confirm DHCP service is running.<br>

**` Overview: `**
  - The DHCP Server role was installed through Server Manager to enable automatic IP address assignment for domain-joined clients. This setup allows the domain controller to distribute network configuration settings such as IP address, subnet mask, gateway, and DNS. It supports consistent connectivity and simplifies network management across the Active Directory environment.<br><br>

<p align="center">Configuring DHCP Scope and Network Options in Active Directory:</strong></p><br>
<p align="center">
<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/607cab9c-2168-442e-8c41-fbfea0440f98" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/da7ef310-4619-4d77-840d-1e3071c55fcd" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/f5e71db1-143b-4fad-ad28-fd597dd5fa86" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/afdcca0d-d0d9-44aa-aed6-e2e2a1a275cc" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/b44ae450-9b93-4604-a12e-1515d939766a" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/93504523-378e-46b5-b784-ad6d20bc53c5" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/6a84c80a-049b-4516-b14f-7be054d70e59" /><br><br>

**` Domain Controller baseline from Phase 1: `**
  - Domain Controller IP: **192.168.56.10**<br>
  - Subnet mask: **255.255.255.0**<br>
  - DNS server: **192.168.56.10**<br>
  
Active Directory Domain Services already installed and functional.<br>
Host-Only network used for isolated lab communication.<br>
These settings define the core infrastructure DHCP must support.<br>

**` DHCP Scope Configuration (Client Addressing Layer) `**
  - Open DHCP Management Console.<br>
  - Expand IPv4 and create new scope.<br>
  - Set scope name: Lab Client Scope.<br>
  - Define IP range: **192.168.56.100 to 192.168.56.200**<br>
  - Set subnet mask: **255.255.255.0**<br>
  - Add exclusion range: **192.168.56.10** (Domain Controller static IP).<br>
  - Set lease duration for lab usage, example 8 days.<br>

**` DHCP Options Configuration (Network Services Layer)`**
  - Set default gateway.
      - Leave blank for isolated Host-Only network.<br>
  - Set only if NAT internet access is required.<br>
  - Set DNS server.<br>
  - **192.168.56.10** (Domain Controller).<br>
  - Set domain name.<br>
  - Match Active Directory domain created in Phase 1.<br>
  - Apply and activate scope.<br>

**` Overview: `**
  - DHCP was configured on the domain controller using the existing Phase 1 network setup. The Domain Controller used a static IP of 192.168.56.10 with DNS set to itself, supporting Active Directory and domain services within a Host-Only isolated network.<br>
  - A DHCP scope was created to assign client IP addresses from 192.168.56.100 to 192.168.56.200 with a 255.255.255.0 subnet mask. The Domain Controller IP was excluded to prevent conflicts, and a lease duration was set for address management.<br>
  - DHCP options were configured to match the domain environment. The default gateway was left blank due to the isolated network design. DNS was set to 192.168.56.10, and the Active Directory domain name was applied so clients receive correct domain configuration automatically.<br><br>

<p align="center">Testing DHCP Configuration on Client Machine:</strong></p><br>
<p align="center">
<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/bde15900-84b0-4026-9495-1ca98820edd9" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/3529e415-68ff-46c6-987f-df3eec74bbe3" /><br>
 <p align="left">
<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/2175c445-0f47-47d6-a077-0b807b6e74fc" />&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/4bd4f186-ddf2-4ab2-a978-356e72228b54" />&nbsp;&nbsp;&nbsp;<img width="475" height="475" alt="image" src="https://github.com/user-attachments/assets/8670c98a-b4e8-4288-88b2-79be96cb404c" /><br>

**` Overview: `**
  - Client testing confirmed DHCP assigned a valid IP from the 192.168.56.100–200 range. The client received correct subnet and DNS set to 192.168.56.10. Connectivity to the domain controller and Active Directory services worked as expected.<br>

**` Key Tasks Completed: `**
  - Installed DHCP Server role using Server Manager.<br>
  - Configured DHCP server within Active Directory environment.<br>
  - Created IPv4 DHCP scope for client IP assignment.<br>
  - Defined IP range **192.168.56.100 to 192.168.56.200**<br>
  - Set subnet mask to **255.255.255.0**<br>
  - Excluded Domain Controller static IP **192.168.56.10**<br>
  - Configured lease duration for client address allocation.<br>
  - Set DNS server option to **192.168.56.10**<br>
  - Applied Active Directory domain name in DHCP options.<br>
  - Left default gateway blank due to isolated Host-Only network.<br>
  - Activated DHCP scope for client distribution.<br>
  - Verified DHCP service functionality in server console.<br>
  - Tested client machine IP assignment using DHCP.<br>
  - Confirmed correct subnet, DNS, and domain configuration on client.<br>
  - Verified communication between client system and Domain Controller.<br>
  - Confirmed Active Directory connectivity through DHCP configuration.<br>
----------------
