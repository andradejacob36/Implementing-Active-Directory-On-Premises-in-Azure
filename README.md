<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Implementing Active Directory On Premises in Azure</h1>
This tutorial is a step-by-step guide that provides instructions on how to create and configure an on-premises Active Directory (AD) environment within the Microsoft Azure cloud computing platform<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used</h2>

- Windows 10 Pro, version 21H2 (free services eligible)
- Windows Server 2022 Datacenter: Azure Edition Gen 2 (free services eligible)

<h2>High-Level Deployment and Configuration Steps</h2>

- Step 1: You need to authenticate and authorize yourself by logging into the Azure portal
- Step 2: Setup Resources in Azure
- Step 3: Ensure Connectivity between the client and Domain Controller
- Step 4: Install Active Directory
- Step 5: Create an Admin and Normal User Account in AD
- Step 6: Join Client-1 to your domain (mydomain.com)
- Step 7: Setup Remote Desktop for non-administrative users on Client-1
- Step 8: Create a bunch of additional users and attempt to log into client-1 with one of the users 

<h2>Step 1: You need to authenticate and authorize yourself by logging into the Azure portal</h2>

1. Go to the Azure Portal website (https://portal.azure.com/) and sign-in with your Azure account credentials. 
- Note: If you do not have an Azure account, you will need to sign up for one before you can log-in.

2. Once you have successfully logged-in, you will be redirected to the Azure portal dashboard where you can create and manage your resources. 


<h2>Step 2: Setup Resources in Azure</h2>

1. Create the Domain Controller VM (Windows Server 2022) named “DC-1”
  <ol type="a">
      <li>Click on the search bar and type "Virtual Machines".</li>
      <li>Click on the "+ Create" button located on the top left-corner by "Switch to classic".</li>
      <li>Choose the option "Azure virtual machine".</li>
      <li>Choose your subscription (For Ex: Azure Subscription 1).</li>
      <li>Create a new name for the resource group (Use: AD-Lab).</li>
      <li>Enter a unique name for the virtual machine (Use: DC-1).</li>
      <li>For "Region" use: (US) West US 3.</li>
      <li>For "Image" use: Windows Server 2022 Datacenter: Azure Edition Gen 2 (free services eligible).</li>
      <li>For "Size" use: Standard_E2s_v3 - 2 vcpus, 16 GiB memory.</li>
      <li>For "Username" use: labuser.</li>
      <li>For "Password" make sure to make up one.</li>
      <li>For "Public inbound ports" click on "Allow selected ports".</li>
      <li>For "Select inbound ports" use: RDP 3389.</li>
      <li>Click on the "Create" button to create the virtual machine.</li>
   </ol>

- Note: Remember to keep your username and password you created in your notepad, as you will need them later. Also, verify that your information is correct! Also, the processing of the VM is expected to take approximately 1-2 minutes. Once the process is complete, we will have successfully deployed the Domain Controller VM, which will act as the dedicated server for our Active Directory.

<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
       
2. Set Domain Controller’s NIC Private IP address to be static
    <ol type="a">
      <li>On the search bar, type "Virtual Machines".</li>
      <li>Click the blue link "DC-1" located under "Name".</li>
      <li>Navigate to the "Networking" option in the settings section. From there, you can select your network interface.</li>
      <li>Under settings, click on "IP configurations". Click the following IP configuration:</li>
      <li>Go to the "Assignment" section and select the option to set the IP address as static.</li>
      <li>Then click "Save".</li>
    </ol>

- Note: Changing the IP address assignment method from dynamic to static ensures that the IP address remains fixed and does not change over time.

Image Display of Step 2: 7C
<p>
<img src="" height="80%" width="80%"/>
</p>
<p>  

Image Display of Step 2: 7D
<p>
<img src="" height="80%" width="80%"/>
</p>
<p>   
    
3. Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet. 
    <ol type="a">
      <li>Click on the search bar and type "Virtual Machines".</li>
      <li>Click on the "+ Create" button located on the top left-corner by "Switch to classic".</li>
      <li>Choose the option "Azure virtual machine".</li>
      <li>Choose your subscription (For Ex: Azure Subscription 1).</li>
      <li>Create a new name for the resource group (Use: AD-Lab).</li>
      <li>Enter a unique name for the virtual machine (Use: Client-1).</li>
      <li>For "Region" use: (US) West US 3.</li>
      <li>For "Image" use: - Windows 10 Pro, version 21H2 (free services eligible).</li>
      <li>For "Size" use: Standard_E2s_v3 - 2 vcpus, 16 GiB memory.</li>
      <li>For "Username" use: labuser.</li>
      <li>For "Password" make sure to make up one.</li>
      <li>For "Public inbound ports" click on "Allow selected ports".</li>
      <li>For "Select inbound ports" use: RDP 3389.</li>
      <li> Click on the "Create" button to create the virtual machine.</li>
    </ol>    
    
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>    
        
4. Ensure that both VMs are in the same Vnet.
    <ol type="a">
      <li>To confirm that both VMs are located in the same region/virtual network, in the search bar type "Virtual Machines".</li>
      <li>Open both "DC-1" and "Client-1" by clicking the blue link located under "Name".</li>
      <li>Under Essentials, verify both VMs have the same "Virtual Network/subnet".</li>
    </ol>

<h2>Step 3: Ensure Connectivity between the client and Domain Controller</h2>

1. Log-in to Client-1 (VM) and ping DC-1's Private IP address with ping -t (perpetual ping). 

    <ol type="a">
      <li>Under Essentials, copy Client-1 Public IP address.</li>
      <li>Click on the "Start" button (Windows logo), then search for "Remote Desktop Connection" and open it. For Mac users download the app                 "Microsoft Remote Desktop" from the App Store.</li>
      <li>Paste the Public IP address(from your VM) in the computer name field and click "Connect". For Mac users paste the IP Address on "PC-name"           and click "add".</li>
      <li>Afterwards, make sure to log-in your credentials from Step 2 (Use Username: labuser/Password: Your unique password).</li>
      <li>For Windows users click "Yes" to connect to your VM. Observe the following display.</li>
      <li>Wait until your virtual machine logs you in.</li>
      <li>Then choose the following options for "Choose privacy settings for your device": </li>
      <li>Location: No </li>
      <li>Diagnostic Data: No</li>
      <li>Tailored experiences: No</li>
      <li>Find my device: No</li>
      <li>Inking and Typing: No</li>
      <li>Advertising ID: No</li>   
      <li>Click "Accept".</li>
      <li>Return to Azure to find DC-1 Private IP address and copy it.</li>
      <li>Go back to your VM in order to ping the private IP address of DC-1.</li>
      <li>Navigate to the bottom-left corner and click on the "Start" button (Windows logo), then search for "Command Line" and open it.</li>
      <li>On the Command Line type "ping -t DC-1 Private IP Address.</li>
      <li>The Command Line should display "Request Time Out" as a response, which could be attributed to the Windows Firewall settings of DC-1               blocking the incoming ping requests.</li>
    </ol>
     
 <p>
<img src="https://i.imgur.com/So0Dn0n.png" height="80%" width="80%"/>
</p>
<p>  
 
<p>
<img src="https://i.imgur.com/xHG3t9h.png" height="80%" width="80%"/>
</p>
<p>  

2. Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall.
    <ol type="a">
      <li>While DC-1 keeps blocking incoming ping request, switch back to your Azure dashboard and initiate "DC-1" VM.</li> 
      <li>Under Essentials, copy DC-1 Public IP address.</li> 
      <li>Click on the "Start" button (Windows logo), then search for "Remote Desktop Connection" and open it.</li>
      <li>Paste the Public IP address(from DC-1) in the computer name field and click "Connect".</li>
      <li>Afterwards, make sure to log-in your credentials from Step 2 (Use Username: labuser/Password: Your unique password).</li>
      <li>On DC-1 (VM), navigate to the bottom-left corner and click on the Windows logo, then search for "wf.msc" and open it.</li>
      <li>Select "Inbound Rules" and expand the "Inbound Rules" panel for a better view.</li> 
      <li>Click on Protocol and find ICMPv4.</li> 
      <li>To enable the "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)" rule, right-click on it and select "Enable" for both the               "Private" and "Domain" profiles.</li>
    </ol>

- Note: Now you have two active Virtual Machines, Client-1 and DC-1. wf.msc stands for windows firewall. ICMP is used by the "ping" protocol. 

3. Check back at Client-1 to see the ping succeed

    <ol type="a"> 
      <li>After minimizing the DC-1 VM, return to the Client-1 VM. You will observe that the command line on Client-1 VM responds with "Reply from           ...". This is possible because we have enabled ICMPv4 communication from the DC-1 VM, allowing for the successful reply.</li> 
      <li>Stop the ping from happening by holding CTRL + C.</li>
      <li></li>
      <li></li>
      <li></li>
      <li></li> 
      <li></li> 
      <li></li>
    </ol>

<h2>Step 4: Install Active Directory</h2>

1. Log-in to DC-1 and install Active Directory Domain Services 

    <ol type="a"> 
      <li>Log-in to DC-1.</li>
      <li>If your server manager is not open then click the Windows Logo button and search for "server manager".</li>
      <li>To install Active Directory, go on Server Manager and click "Add roles and features".</li>
      <li>Click "Next" for "Before You Begin", "Installation Type", and "Server Selection".</li> 
      <li>On Server Roles, allow "Active Directory Domain Services". Then click on "Add Features".</li> 
      <li>Click "Next" for "Server Roles", "Features", and "AD DS".</li>
      <li>Afterwards, click "Install".</li>
      <li>You should see an exclamation mark located at the top left-corner. Click that, and click again on "Promote this server to a domain                 controller".</li>
      <li>Under Select the deployment operation, click "Add a new forest".</li>
      <li>For Root domain name, use: mydomain.com </li> 
      <li>Click "Next" on "Deployment Configuration".</li> 
      <li>On Domain Controller Options, create/confirm you password (Use: Password1).</li>
      <li>Click "Next" on "Domain Controller Options", "DNS Options", "Additional Options", "Paths", "Review Options", and "Prerequisites Check".
          </li>
      <li>On the Installation section, click "Install".</li>
      <li>Reminder, when you launch the Remote Desktop Connection. with the context of the domain Make sure to log-in your credentials (Use                   Username: mydomain.com\labuser, Password: Your unique password).</li>
      <li>To open Active Directory, click the Windows Logo and search "Active Directory Administrative Center". </li> 
      <li></li> 
      <li></li>
    </ol>

- Note: If you don't know what VM you are loged into, go to command line and enter "hostname". The command line should either respond with DC-1 or Client-1. After installing AD DS, it will not work properly until you setup your domain controller. You don't have to use "mydomain.com" as the root domain name. You can name it whatever you want. Your DC-1 (VM) will restart after the installation. If you get logged-out of your VM connect back to it.  

2. Promote as a DC: Setup a new forest as mydomain.com

    <ol type="a"> 
      <li>Under Select the deployment operation, click "Add a new forest".</li>
      <li>For Root domain name, use: mydomain.com </li> 
      <li>Click "Next" on "Deployment Configuration".</li> 
      <li>On Domain Controller Options, create/confirm you password (Use: Password1).</li>
      <li>Click "Next" on "Domain Controller Options", "DNS Options", "Additional Options", "Paths", "Review Options", and "Prerequisites Check".
          </li>
      <li>On the Installation section, click "Install".</li>
      <li>Reminder, when you launch the Remote Desktop Connection. with the context of the domain Make sure to log-in your credentials (Use                   Username: mydomain.com\labuser, Password: Your unique password).</li>
      <li>To open Active Directory, click the Windows Logo and search "Active Directory Administrative Center". </li> 
      <li></li> 
      <li></li>
    </ol>
    
3. Restart and then log back into DC-1 as user: mydomain.com\labuser

    <ol type="a"> 
      <li>Reminder, when you launch the Remote Desktop Connection. with the context of the domain Make sure to log-in your credentials (Use                   Username: mydomain.com\labuser, Password: Your unique password).</li>
      <li>To open Active Directory, click the Windows Logo and search "Active Directory Administrative Center". </li> 
    </ol>








<h2>Step 5: Create an Admin and Normal User Account in AD</h2>

1. In Active Directory Users & Computers(ADUC), create an Organizational Unit(OU) called "_EMPLOYEES".

    <ol type="a"> 
      <li>Right-click on mydomain.com, click on "New" and "Organizational Unit".</li>
      <li>Create a folder by naming it "_EMPLOYEES" and click "Okay".</li> 
    </ol>
 
2. Create a new OU named "_ADMINS".
    <ol type="a"> 
      <li>Do the same for "_ADMINS". Right-click on mydomain.com and click "Refresh".</li>
      <li>To create our own admin account, right-click on "_ADMINS" and click on "New" and "User".</li> 
    </ol>

3. Create a new employee named "Jane Doe" (Same password) with the username of "Terminator_admin".
     <ol type="a"> 
      <li>I input the following information as First Name: Terminator, Last Name: 3000, Full Name: Terminator 3000, User logon name:                         Terminator_admin. Afterwards click "Next".</li>
      <li>Create/Confirm your Password (Use your unique Password). Uncheck "User must change password at next logon", and check on "Password never           expires".</li> 
    </ol>
    
- Note: You can make up your own name/last name. Remember to keep your "User logon name" in your notepad, as you will need them later.

4. Add jane_admin to the "Domain Admins" Security Group.
     <ol type="a"> 
      <li>Right-click on your created account and choose "Properties". Go to "Member Of" section.</li>
      <li>Click "ADD". Under Enter the object names to select, type "domain" and click on "Check Names".</li> 
      <li>Pick the "Domain Admins" group. Then click "OK", "Apply", and "OK".</li>     
    </ol>

5. Log-out/ close the connection to DC-1 and log back in as "mydomain.com\Terminator_admin".
     <ol type="a"> 
      <li>To determine which VM you are log into, go to command line and type the following: "whoami", it will show you your username. If it                  responds "mydomain\labuser" then log-out from it.</li>
      <li>To initiate DC-1 (VM) from your Azure dashboard, copy "DC-1" Public IP Address and paste to Remote Desktop Connection.</li> 
      <li>Instead of logging-in with labuser, log-in with username: mydomain.com\Terminator_admin and your unique password.</li>     
      <li>To verify if you logged-in to the correct VM, go to command line and type "whoami". It must respond back as "mydomain\Terminator_admin         </li>     
    </ol>

6. Log-in to Client-1 as the original local admin (use: labuser) & join it to the domain.
    <ol type="a"> 
      <li>If your Client-1 (VM) is closed, then initiate it from your Azure VM.</li> 
      <li>To verify the name of the VM, go to command line and type "hostname", it should respond as "Client-1".</li>     
      <li>Right-click the Windows logo button and click "System".</li>     
      <li>Under Related settings, click "Rename this PC (Advanced)".</li>  
      <li>Click "Change", it is located besides "To rename this computer or change its domain or workgroup click Change".</li>  
      <li>Click Domain and type "mydomain.com". Click "OK".</li>  
      <li>You will receive an error stating that "domain.com" could not be contacted.</li>
    </ol>

7. From the Azure Portal, set Client-1's DNS settings to the DC's Private IP address.
    <ol type="a"> 
      <li>Go back to Azure; Virtual Machines; DC-1. Under Settings, click on the "Networking" tab.</li> 
      <li>Look for your NIC Private IP and copy your private IP address.</li>     
      <li>Repeat 7a for Client-1 (VM). Click your client's network interface.</li>     
      <li>Under Settings, click "DNS servers". Also, click "Custom" to paste DC-1's private IP address in "Add DNS server".</li>  
      <li>Make sure to save.</li>  
    </ol>

8. From the Azure Portal, restart Client-1.
    <ol type="a"> 
      <li>Return to Azure; Virtual Machines; Client-1. Click the "Restart" button.</li> 
    </ol>

9. Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart).


42:37

