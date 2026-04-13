<h1>Active Directory User Management & Troubleshooting Home Lab</h1>

<h2>Description:</h2>
This lab covers core Microsoft Active Directory tasks. You create and manage user accounts, reset passwords, unlock and disable users, and control access with groups and permissions. You also troubleshoot issues like trust relationship errors, building key identity and access management skills.
<br />

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

**`Phase I: Environment Setup`**
<p align="center">
Server Setup & Domain Controller Configuration: <br />

**` Active Directory Purpose & Key Concepts `** 
  -  The server promoted to a domain controller is the most important system in a company’s network, as it hosts Active Directory.
  -  Active Directory provides centralized identity and access management.
  -  A domain controller hosts Active Directory and handles authentication and security policies across the network.
  -  Active Directory manages authentication, passwords, logins, users, groups, and computers, and controls access through permissions and group policies.
  -  Group Policy Objects enforce security settings and system configurations.
  -  Active Directory also supports least privilege access to protect network resources.<br><br>

**` Overview `** 
-  This step prepares the server for domain controller deployment. The process includes renaming the server, configuring network adapters, and assigning a static IP address. A static IP keeps the server address consistent, unlike dynamic IPs that change after lease renewal. This setup establishes reliable network communication for the domain controller. It supports Active Directory, DNS, and DHCP services and ensures stable connectivity across the network.<br><br>
  
<p align="center">
Renaming the Server:
 <p align="center">
 <img width="550" height="550" alt="Image" src="https://github.com/user-attachments/assets/64b0fe4f-d9cc-4e6d-9910-b6e19a07274e" /><br>
 
**` Steps `**
  - Open Server Manager.
  - Click Local Server on the left panel.
  - Locate the Computer Name field.
  - Click the current computer name and enter the System Properties window, click Change.
  - Enter the new Computer Name and click OK to apply changes.
  - Restart the server when prompted.<br><br>
    
<p align="center">
Configuring Network Adapters for the Server:
<p align="center">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/9a9e9c27-ea0a-4a1b-8402-d452e8960920" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/e559f4ec-497c-4b7f-900a-dac157b7a590" /> <br><br>

**` Configuring Network Adapter `**
  - The Host-Only Adapter is the isolated internal lab network where the Domain Controller and Windows 11 Clients will communicate with each other.
  - NAT gives the server internal access, but it's kept isolated from the home network.<br><br>

<p align="center">
Identifying IP Range for the Host-Only Network:
<p align="center">
 <img width="500" height="500" alt="Image" src="https://github.com/user-attachments/assets/6d28d6aa-75d2-4964-a4ba-4c5a54962c69" /><br>
 
**` Determining IP Address Range for Static Configuration `**
  - The domain controller must keep a static IP address because all network devices depend on it for communication. Computers, users, and printers connect to it for authentication and resource access.
  - It acts as the central control point for the home lab network.
  - VirtualBox Host-Only Network uses a defined IP range. The first step is identifying the subnet range assigned to the host-only adapter. This range determines valid IP addresses for the virtual machines and the host system within the isolated network.<br><br>

<p align="center">
Configuring DC with a Static IPv4 Address:
<p align="center">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/285b3c8a-160d-48ae-a5f5-cec6a8d68302" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="475" height="475" alt="Image" src="https://github.com/user-attachments/assets/090cb017-73dc-4815-9cb6-b528da504f6c" /><br><br>

 **` Assigning Static Configuration `**
  - Open IPv4 settings.
  - Select manual configuration.
  - Enter static IP address, subnet mask, default gateway, and DNS server.
  - Save settings and verify connectivity using ping.<br><br>

<p align="center">
 Installing AD DS & Promoting Server to DC:
 <p align="center">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/eab11d16-5ca2-4f2a-a3fe-3fd09b21c8b3" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/07c707b8-6917-42aa-9326-827b5c0877ec" /><br><br>

 **` Installing Active Directory `**
   - I installed Active Directory Domain Services and promoted the server to a domain controller, which converts the system from a standalone server into the core identity system of the network.
   - I added the AD DS role through Server Manager, then ran the promotion wizard.
   - During promotion, I created a new forest and domain, set the Directory Services Restore Mode password, and allowed the server to install and configure DNS.
   - After the restart, the server began handling authentication, domain logins, and directory services.
   - It now stores user accounts, computers, and security policies, and it controls access to resources across the environment.
   - This step establishes the foundation for centralized identity and access management in my lab.<br><br>

<p align="center">
Confirmation of Creation of the DC:
<p align="center">
<img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/a77db9d3-e4df-471a-a3fe-0504288d91b9" /> &nbsp;&nbsp;&nbsp;<img width="500" height="650" alt="Image" src="https://github.com/user-attachments/assets/683029ae-ab6c-40f9-9367-5ece9824c468" /><br><br>
 
**` Tasks Completed `**
- Server renamed to **DC02** to match the domain controller role and rebooted successfully.
- VirtualBox networking configured with Host-Only and NAT adapters to isolate lab traffic and allow controlled external access.
- Host-Only IP range identified for static addressing.
- Domain controller assigned a static IP address **192.168.56.10**, subnet mask **255.255.255.0**, and DNS **192.168.56.10** with manual IPv4 configuration.
- No default gateway was configured because the lab uses an isolated internal network. All communication occurs within the local environment between the Domain Controller and the client system.
- Network settings applied and verified for connectivity.
- Installed Active Directory and promoted the server to a DC.<br><br>


**`Phase II: Organizational Unit (OU) Design & User Provisioning`**
<p align="center">
Active Directory Infrastructure Setup:  <br/>
 
 **` Organizational Unit (OU) Design & User Provisioning Key Concepts `** 
  -  OUs organize Active Directory objects like users and computers into structured groups for easier management.
  -  OU design follows business structure, such as departments, roles, or locations.
  -  User provisioning includes creating and configuring user accounts with the correct attributes and settings.
  -  Security groups are used to assign permissions and control access to resources.
  -  OU structure supports delegation of administration and simplifies policy application through Group Policy Objects.<br><br>

**` Overview `** 
-  This phase focuses on building and managing the Active Directory structure. Organizational Units are created to organize users and computers for structured administration. New user accounts are added and placed into appropriate security groups, including administrator accounts for elevated privileges. A Windows 10 system is then joined to the domain to integrate it with Active Directory. After the join process, a newly created domain user account is used to log in and verify authentication and access control functionality.<br><br>
  
<p align="center">
 Creating OUs: 
<p align="center">
  <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/44a17e7f-d772-4b59-bee7-0db5bd3420f4" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/885f9994-7b3a-4247-a0d6-8169263b087e" /><br><br>
  <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/4d1b5e38-a3e9-4599-8450-66f123605b18" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/c7b5f4ba-83ba-47c2-90d8-01dee3115750" />

**` OU & User Account Structure in Active Directory `**
- Organizational Units in Active Directory are not meant for built-in groups or default users.
- Built-in groups exist for system functions, and adding standard users to them creates security and management issues.
- New OUs are created to keep Active Directory clean and organized. They allow structured management of users and computers and support future Group Policy applications.
- To create an OU, open the domain, right-click the domain name, such as LAB.local, select New, then select Organizational Unit, enter a name, and select OK.
- Inside Organizational Units, accounts are created and organized based on function or department. OUs can contain user accounts like Jim and Patricia, as well as groups such as HR or Finance.
- Separating users into OUs improves control, simplifies administration, and supports scalable Group Policy management across the domain.<br><br>

<p align="center">
Joining a Windows Client to an Active Directory Domain:<br/>
<p align="center">
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/3a3307e2-8412-49f6-8ad9-ab680308cef5" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/9cb42af5-5a08-415a-8f18-e8f4c54fc304" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/deb2cf54-a301-4f5a-9184-ea6df373cc8a" /><br><br>

**` Domain Integration of a Windows Client with Active Directory `**
   - Adding a client machine to the domain connects it to Active Directory so it can authenticate users through the domain controller.
   - This allows centralized login, consistent security policies, and controlled access to network resources.
   - It ensures users sign in with domain accounts instead of local accounts, which improves security and simplifies management for administrators.<br><br>
   
**` Steps `**
   - Set the client DNS to the domain controller IP address.
   - Open system settings on the Windows 10 client.
   - Go to About or System Properties.
   - Select Rename this PC (advanced).
   - Choose the Domain option.
   - Enter the domain name. Provide domain admin credentials.
   - Restart the client machine.
   - After reboot, sign in using a domain user account.<br><br>

<p align="center">
Enabling Domain User Login on Client Machines
<p align="center">
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/d1cc46eb-d728-4301-8b85-d142596e5d3a" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/245491b6-0802-43c6-82be-1d7a41d801c9" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/dbcbd970-22d5-40c2-9749-c7a8dc3cccb9" />

**` Tasks Completed `**
- Created Organizational Units to structure Active Directory by department and role.
- Provisioned user accounts **(Patricia Johnson & Jim Watkins)** and security groups **(HR & Finance)** within appropriate OUs.
- Followed best practices by avoiding default containers and using dedicated OUs.
- Joined Windows 10 client machine **(CLIENT02)** to the domain **(LAB.local)** using domain credentials.
- Configured CLIENT02 DNS **(192.168.56.10)** to point to the domain controller **(LAB.local)** for proper communication.
- Verified domain integration by logging in with a domain user account.
- Confirmed authentication and access control through the domain controller.

  
<p align="center">
RAS/NAT: <br/>

<p align="left">
 <img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/0af40b6b-c50a-42f3-846c-0f799495c9b5" /> 
 &nbsp;&nbsp;&nbsp;&nbsp; <img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/566f74c0-8ec3-43bb-9d1f-0c1cd6f3f2b4" />
 <br><br>
 <img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/b6adedc0-1835-4dcf-8f3c-1cace81b3fc6" />
 &nbsp;&nbsp;&nbsp;&nbsp; <img width="450" height="450" alt="Image" src="https://github.com/user-attachments/assets/2ba4ead2-73c0-4436-9fd3-d97d8aa496d7" />
</p>

**` Tasks Completed `**
- Installed the Remote Access role on Windows Server.  
- Enabled Routing and Remote Access Service (RRAS).  
- Configured NAT to allow internal network access to the Internet.  
- Selected the external network interface for Internet access.  
- Verified client connectivity to external networks.  

**` Overview `** 
-  This step allows devices on the internal network to access external networks through Network Address Translation. NAT enables multiple systems to share a single external connection, which is commonly used in enterprise environments to manage and secure outbound traffic.

<p align="center">
Internet Access Configuration:  <br/>

 <img width="409" height="445" alt="Image" src="https://github.com/user-attachments/assets/4bb79ddc-54cb-4b1c-928e-cb3c04e51eb4" />&nbsp;&nbsp;&nbsp;&nbsp;
<img width="409" height="445" alt="Image" src="https://github.com/user-attachments/assets/c1459a11-34f1-42a4-af84-e9d09cea9848" />
 
 **` Tasks Completed `**
- Opened Server Manager and accessed Local Server settings.  
- Disabled Internet Explorer Enhanced Security Configuration (IE ESC).  
- Verified internet access from the Domain Controller using a web browser.  

**` Overview `** 
-  This step enables internet access from the Domain Controller for testing. In a production environment, Domain Controllers are typically restricted from direct internet browsing to reduce security risks, but this is enabled in the lab to validate connectivity and functionality.
  
<p align="center">
PowerShell User Automation:  <br/>

 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/add3c286-d617-41ba-9462-35ef3c38b454" />&nbsp;&nbsp;&nbsp;&nbsp; <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/8e6a987d-ca5e-4e6e-9ef0-486cafb018c9" />
 <br><br>
 <img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/dd3d2fc9-e654-4b75-b909-646ba155321d" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/501c0561-3070-45b9-ab0d-f909c1f3e416" />

 - This script creates 1,000 user accounts to simulate a large enterprise environment.
 
**` Script Used `**
```powershell
$PASSWORD_FOR_USERS   = "Password1"
$NUMBER_OF_ACCOUNTS_TO_CREATE = 10000

Function generate-random-name() {
    $consonants = @('b','c','d','f','g','h','j','k','l','m','n','p','q','r','s','t','v','w','x','z')
    $vowels = @('a','e','i','o','u','y')
    $nameLength = Get-Random -Minimum 3 -Maximum 7
    $count = 0
    $name = ""

    while ($count -lt $nameLength) {
        if ($count % 2 -eq 0) {
            $name += $consonants[(Get-Random -Minimum 0 -Maximum ($consonants.Count - 1))]
        } else {
            $name += $vowels[(Get-Random -Minimum 0 -Maximum ($vowels.Count - 1))]
        }
        $count++
    }
    return $name
}

$count = 1
while ($count -lt $NUMBER_OF_ACCOUNTS_TO_CREATE) {
    $firstName = generate-random-name
    $lastName = generate-random-name
    $username = "$firstName.$lastName"
    $password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force

    Write-Host "Creating user: $username"

    New-ADUser -AccountPassword $password `
               -GivenName $firstName `
               -Surname $lastName `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "OU=_EMPLOYEES,$(([ADSI]``"").distinguishedName)" `
               -Enabled $true

    $count++
}
```
 **` Script Overview `** 
 - This script automates bulk user creation in Active Directory by generating random first and last names and creating unique usernames. It significantly reduces manual effort and demonstrates how scripting is used in enterprise environments to manage large numbers of user accounts efficiently.
   
 **` Tasks Completed `**
- Opened PowerShell with administrative privileges. 
- Imported Active Directory module. 
- Executed script to create multiple user accounts. 
- Verified users were successfully created in Active Directory. 
  
**` Overview `** 
-  This step uses PowerShell to automate user account creation in Active Directory. Automation reduces manual effort, improves consistency, and is commonly used in enterprise environments to manage large numbers of users efficiently.

<p align="center">
Client Machine Setup:  <br/>
 
<img width="400" height="500" alt="Image" src="https://github.com/user-attachments/assets/96368fa8-aed4-4996-88e4-9a3e5ff2850e" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="350" alt="Image" src="https://github.com/user-attachments/assets/d4a5bb7b-9200-4efd-82d3-998e43bb550b" />
<br /><br>
<img width="400" height="500" alt="Image" src="https://github.com/user-attachments/assets/1c9b475b-7d7e-4de2-ad2e-f9bee2aa8afa" />&nbsp;&nbsp;&nbsp;&nbsp;<img width="400" height="409" alt="Image" src="https://github.com/user-attachments/assets/c63d1d7b-29b8-4a66-9d5a-0ef34f73af43" />
<br><br>
<img width="400" height="400" alt="Image" src="https://github.com/user-attachments/assets/23663373-6f67-4d76-9064-bc9db9319b91" />
 
**` Tasks Completed `**
- Created Windows 10 virtual machine (Client 1) in VirtualBox.  
- Allocated system resources (RAM, storage, network adapter).  
- Installed Windows 10 operating system.  
- Connected the client machine to the internal network.  
- Verified network connectivity with the Domain Controller.   

**` Overview `**  
- This step sets up a client system within the lab environment to simulate a real user workstation. The client machine is used to test domain connectivity, user authentication, and network services provided by the Domain Controller.

<p align="center">
Domain User Login Validation:  <br/>
 
<img width="1016" height="868" alt="Image" src="https://github.com/user-attachments/assets/29ab288a-5811-425d-b62c-574269f28310" />
</p>

**` Tasks Completed `**
- Logged into Client 1 using Active Directory user accounts. 
- Verified successful domain authentication. 
- Confirmed user accounts created via PowerShell are functional. 
- Tested access to the domain environment from the client system.   

**` Overview `**  
- This step validates the full Active Directory setup by confirming that users created on the Domain Controller can authenticate and log into a client machine. It demonstrates successful integration of user management, domain services, and client connectivity within the network.
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
