<h1>Active Directory User Management & Troubleshooting Home Lab</h1>

<h2>Description:</h2>
This lab builds a simulated enterprise network using Active Directory. A Domain Controller was configured with DNS and DHCP, a client machine was joined to the domain, and PowerShell was used to automate user creation. It demonstrates real-world skills in system administration, network management, and identity control.
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

<h2>Network Diagram:</h2>
<img width="1239" height="749" alt="Image" src="https://github.com/user-attachments/assets/5700374f-8276-4054-962e-7421f61d373d" />

<h2>Lab Walk-Through:</h2>

<p align="center">
IP Addressing and Network Configuration: <br />

<p align="center">
 <img src="https://github.com/user-attachments/assets/697e368a-3ad4-41b8-af02-2df31b9223ac" width="260"/>
 &nbsp;&nbsp;&nbsp;&nbsp; <img src="https://github.com/user-attachments/assets/47e9cff0-67ae-411c-9b32-6ae93d4f1d7e" width="260"/>

**` Tasks Completed `**
- Configured static IP on Windows Server (Domain Controller). 
- Assigned IP address **172.16.0.1**, subnet mask **255.255.255.0**, and DNS **127.0.0.1**.  
- Set DNS to point to the Domain Controller.
- No default gateway was configured because the lab uses an isolated internal network. All communication occurs within the local environment between the Domain Controller and the client system.  

**` Overview `** 
-  This step establishes network communication by assigning a static IP address to the Domain Controller. Proper IP configuration ensures reliable connectivity and supports Active Directory, DNS, and DHCP services.
  
<p align="center">
Active Directory Installation and Domain Setup:  <br/>
 
<p align="left">
  <img src="https://github.com/user-attachments/assets/c9c572fd-f1a9-4c6b-8115-f0cc490e332f" width="400"/>
  &nbsp;&nbsp;&nbsp;&nbsp; <img src="https://github.com/user-attachments/assets/9a33d328-8601-42cd-94dd-91bf3d161bc0" width="400"/> 
 <br><br>
  <img src="https://github.com/user-attachments/assets/7b33cc25-9f0b-4ff7-ad99-bc2cfaf7321b" width="400"/>
</p>

  **` Tasks Completed `**
- Installed Active Directory Domain Services (AD DS) role.  
- Promoted Windows Server to Domain Controller.  
- Created a new domain (mydomain.com).  
- Configured DNS during domain setup.  
- Verified successful domain deployment.  

**` Overview `** 
-  This step establishes centralized identity and access management by configuring the server as a Domain Controller. Active Directory allows administrators to manage users, systems, and resources within a domain, which is a core function in enterprise IT environments.
  
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
