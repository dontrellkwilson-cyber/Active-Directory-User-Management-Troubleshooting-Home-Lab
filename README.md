<h1>Active Directory User Management & Troubleshooting Home Lab</h1>

<h2>Description:</h2>
This lab covers core Microsoft Active Directory tasks. You create and manage user accounts, reset passwords, unlock and disable users, and control access with groups and permissions. You also troubleshoot issues like trust relationship errors, building key identity and access management skills.<br /><br>

<h2>Languages and Technologies Used:</h2>

- <b>PowerShell</b> 
- <b>DNS</b>
- <b>DHCP</b>
- <b>Active Directory Users and Computers (ADUC)</b>
- <b>Active Directory Domain Services (AD DS)</b>

<h2>Environments Used:</h2>

 - <b>VirtualBox 10</b>
 - <b>Windows 10</b> (21H2)
 - <b>Windows Server 2022</b>

<h2>Lab Walk-Through:</h2>

<p align="center"> <strong>Phase I: Environment Setup</strong> </p><br>

**` Active Directory Purpose & Key Concepts: `**
  -  The server promoted to a domain controller is the most important system in a company’s network, as it hosts Active Directory.<br>
  -  Active Directory provides centralized identity and access management.<br>
  -  A domain controller hosts Active Directory and handles authentication and security policies across the network.<br>
  -  Active Directory manages authentication, passwords, logins, users, groups, and computers, and controls access through permissions and group policies.<br>
  -  Group Policy Objects enforce security settings and system configurations.<br>
  -  Active Directory also supports least privilege access to protect network resources.<br><br>

**` Lab Overview: `** 
-  This step prepares the server for domain controller deployment. The process includes renaming the server, configuring network adapters, and assigning a static IP address. A static IP keeps the server address consistent, unlike dynamic IPs that change after lease renewal. This setup establishes reliable network communication for the domain controller. It supports Active Directory, DNS, and DHCP services and ensures stable connectivity across the network.<br><br>

**` Server Setup & Domain Controller Configuration:`**<br><br> 

 <p align="center"> <strong>Renaming the Server:</strong> </p>
 <p align="center">
 <img width="550" height="550" alt="Image" src="https://github.com/user-attachments/assets/64b0fe4f-d9cc-4e6d-9910-b6e19a07274e" /><br>
 
**` Steps: `**
  - Open Server Manager.<br>
  - Click Local Server on the left panel.<br>
  - Locate the Computer Name field.<br>
  - Click the current computer name and enter the System Properties window, click Change.<br>
  - Enter the new Computer Name and click OK to apply changes.<br>
  - Restart the server when prompted.<br><br>
    
<p align="center"> <strong>Configuring Network Adapters for the Server:</strong> </p><br>
<p align="center">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/9a9e9c27-ea0a-4a1b-8402-d452e8960920" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/e559f4ec-497c-4b7f-900a-dac157b7a590" /> <br><br>

**` Configuring Network Adapters: `**
  - The Host-Only Adapter is the isolated internal lab network where the Domain Controller and Windows 11 Clients will communicate with each other.<br>
  - NAT gives the server internal access, but it's kept isolated from the home network.<br><br>

<p align="center"> <strong>Identifying IP Range for the Host-Only Network:</strong> </p><br>
<p align="center">
 <img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/6d28d6aa-75d2-4964-a4ba-4c5a54962c69" /><br>
 
**` Determining IP Address Range for Static Configuration: `**
  - The domain controller must keep a static IP address because all network devices depend on it for communication. Computers, users, and printers connect to it for authentication and resource access.<br>
  - It acts as the central control point for the home lab network.<br>
  - VirtualBox Host-Only Network uses a defined IP range. The first step is identifying the subnet range assigned to the host-only adapter. This range determines valid IP addresses for the virtual machines and the host system within the isolated network.<br><br>

<p align="center"> <strong>Configuring DC with a Static IPv4 Address:</strong> </p><br>
<p align="center">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/285b3c8a-160d-48ae-a5f5-cec6a8d68302" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/090cb017-73dc-4815-9cb6-b528da504f6c" /><br><br>

 **` Assigning Static Configuration: `**
  - Open IPv4 settings.<br>
  - Select manual configuration.<br>
  - Enter static IP address, subnet mask, default gateway, and DNS server.<br>
  - Save settings and verify connectivity using ping.<br><br>

<p align="center"> <strong>Installing AD DS & Promoting Server to DC:</strong> </p><br>
 <p align="center">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/eab11d16-5ca2-4f2a-a3fe-3fd09b21c8b3" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/07c707b8-6917-42aa-9326-827b5c0877ec" /><br><br>

 **` Installing Active Directory: `**
   - I installed Active Directory Domain Services and promoted the server to a domain controller, which converts the system from a standalone server into the core identity system of the network.
   - I added the AD DS role through Server Manager, then ran the promotion wizard.
   - During promotion, I created a new forest and domain, set the Directory Services Restore Mode password, and allowed the server to install and configure DNS.
   - After the restart, the server began handling authentication, domain logins, and directory services.
   - It now stores user accounts, computers, and security policies, and it controls access to resources across the environment.
   - This step establishes the foundation for centralized identity and access management in my lab.<br><br>

<p align="center"> <strong>Confirmation of Creation of the DC:</strong> </p><br>
<p align="center">
<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/a77db9d3-e4df-471a-a3fe-0504288d91b9" /> &nbsp;&nbsp;&nbsp;<img width="500" height="650" alt="Image" src="https://github.com/user-attachments/assets/683029ae-ab6c-40f9-9367-5ece9824c468" /><br><br>
 
**` Key Tasks Completed: `**
- Server renamed to **DC02** to match the domain controller role and rebooted successfully.<br>
- VirtualBox networking configured with Host-Only and NAT adapters to isolate lab traffic and allow controlled external access.<br>
- Host-Only IP range identified for static addressing.<br>
- Domain controller assigned a static IP address **192.168.56.10**, subnet mask **255.255.255.0**, and DNS **192.168.56.10** with manual IPv4 configuration.<br>
- No default gateway was configured because the lab uses an isolated internal network. All communication occurs within the local environment between the Domain Controller and the client system.<br>
- Network settings applied and verified for connectivity.<br>
- Installed Active Directory and promoted the server to a DC.<br><br>

<p align="center"> <strong>Phase II: Organizational Unit (OU) Design & User Provisioning</strong> </p><br>

 **` Organizational Unit (OU) Design & User Provisioning Key Concepts: `** 
  -  OUs organize Active Directory objects like users and computers into structured groups for easier management.<br>
  -  OU design follows business structure, such as departments, roles, or locations.<br>
  -  User provisioning includes creating and configuring user accounts with the correct attributes and settings.<br>
  -  Security groups are used to assign permissions and control access to resources.<br>
  -  OU structure supports delegation of administration and simplifies policy application through Group Policy Objects.<br><br>

**` Lab Overview: `** 
-  This phase focuses on building and managing the Active Directory structure. Organizational Units are created to organize users and computers for structured administration. New user accounts are added and placed into appropriate security groups, including administrator accounts for elevated privileges. A Windows 10 system is then joined to the domain to integrate it with Active Directory. After the join process, a newly created domain user account is used to log in and verify authentication and access control functionality.<br><br>

**`Active Directory Infrastructure Setup:`**<br/>
 <p align="center"> <strong>Creating OUs:</strong> </p> 
<p align="center">
  <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/44a17e7f-d772-4b59-bee7-0db5bd3420f4" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/885f9994-7b3a-4247-a0d6-8169263b087e" /><br><br>
  <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/4d1b5e38-a3e9-4599-8450-66f123605b18" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/c7b5f4ba-83ba-47c2-90d8-01dee3115750" />

**` OU & User Account Structure in Active Directory: `**
- Organizational Units in Active Directory are not meant for built-in groups or default users.<br>
- Built-in groups exist for system functions, and adding standard users to them creates security and management issues.<br>
- New OUs are created to keep Active Directory clean and organized. They allow structured management of users and computers and support future Group Policy applications.<br>
- To create an OU, open the domain, right-click the domain name, such as LAB.local, select New, then select Organizational Unit, enter a name, and select OK.<br>
- Inside Organizational Units, accounts are created and organized based on function or department. OUs can contain user accounts like Jim and Patricia, as well as groups such as HR or Finance.<br>
- Separating users into OUs improves control, simplifies administration, and supports scalable Group Policy management across the domain.<br><br>

 <p align="center"> <strong>Joining a Windows Client to an Active Directory Domain:</strong> </p><br/>
<p align="center">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/3a3307e2-8412-49f6-8ad9-ab680308cef5" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/9cb42af5-5a08-415a-8f18-e8f4c54fc304" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/deb2cf54-a301-4f5a-9184-ea6df373cc8a" /><br><br>

**` Domain Integration of a Windows Client with Active Directory: `**
   - Adding a client machine to the domain connects it to Active Directory so it can authenticate users through the domain controller.<br>
   - This allows centralized login, consistent security policies, and controlled access to network resources.<br>
   - It ensures users sign in with domain accounts instead of local accounts, which improves security and simplifies management for administrators.<br><br>
   
**` Steps: `**
   - Set the client DNS to the domain controller IP address.<br>
   - Open system settings on the Windows 10 client.<br>
   - Go to About or System Properties.<br>
   - Select Rename this PC (advanced).<br>
   - Choose the Domain option.<br>
   - Enter the domain name. Provide domain admin credentials.<br>
   - Restart the client machine.<br>
   - After reboot, sign in using a domain user account.<br><br>

<p align="center"> <strong>Enabling Domain User Login on Client Machines</strong> </p><br/>
<p align="center">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/d1cc46eb-d728-4301-8b85-d142596e5d3a" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/245491b6-0802-43c6-82be-1d7a41d801c9" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/dbcbd970-22d5-40c2-9749-c7a8dc3cccb9" />

**` Key Tasks Completed: `**
- Created Organizational Units to structure Active Directory by department and role.<br>
- Provisioned user accounts **(Patricia Johnson & Jim Watkins)** and security groups **(HR & Finance)** within appropriate OUs.<br>
- Followed best practices by avoiding default containers and using dedicated OUs.<br>
- Joined Windows 10 client machine **(CLIENT02)** to the domain **(LAB.local)** using domain credentials.<br>
- Configured CLIENT02 DNS **(192.168.56.10)** to point to the domain controller **(LAB.local)** for proper communication.<br>
- Verified domain integration by logging in with a domain user account.<br>
- Confirmed authentication and access control through the domain controller.<br><br>

<p align="center"> <strong>Phase III: Group Policy Management</strong> </p><br>

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

  <p align="center"> <strong>Phase IV: File Server and Permissions</strong></p><br>

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

<p align="center"> <strong>Creating and Configuring Shared Folders</strong></p><br>
<p align="center">


