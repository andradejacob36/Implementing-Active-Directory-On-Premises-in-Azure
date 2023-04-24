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

1.Go to the Azure Portal website (https://portal.azure.com/) and sign-in with your Azure account credentials. 

2.Once you have successfully logged-in, you will be redirected to the Azure portal dashboard where you can create and manage your resources. 

- Note: If you do not have an Azure account, you will need to sign up for one before you can log-in.

<h2>Step 2: Setup Resources in Azure</h2>

1.Create the Domain Controller VM (Windows Server 2022) named “DC-1”.
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

- Note: Remember to keep your username and password you created in your notepad, as you will need them later. Also, verify that your information is correct! Once the process is complete, we will have successfully deployed the Domain Controller VM, which will act as the dedicated server for our Active Directory.

Image Display of Step 2: 1N
<p>
<img src="https://i.imgur.com/3vLXclp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
       
2.Set Domain Controller’s NIC Private IP address to be static
    <ol type="a">
      <li>On the search bar, type "Virtual Machines".</li>
      <li>Click the blue link "DC-1" located under "Name".</li>
      <li>Navigate to the "Networking" option in the settings section. From there, you can select your network interface.</li>
      <li>Under settings, click on "IP configurations". Click the following IP configuration:</li>
      <li>Go to the "Assignment" section and select the option to set the IP address as static.</li>
      <li>Then click "Save".</li>
    </ol>

- Note: Changing the IP address assignment method from dynamic to static ensures that the IP address remains fixed and does not change over time.

Image Display of Step 2: 2B-C
<p>
<img src="https://i.imgur.com/Yz8bvhG.png" height="80%" width="80%"/>
</p>
<p>  

Image Display of Step 2: 2D
<p>
<img src="https://i.imgur.com/D8RApze.png" height="80%" width="80%"/>
</p>
<p>   

Image Display of Step 2: 2E-F
<p>
<img src="https://i.imgur.com/JgvvZTZ.png" height="80%" width="80%"/>
</p>
<p>   
   
3.Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet. 
    <ol type="a">
      <li>Click on the search bar and type "Virtual Machines".</li>
      <li>Click on the "+ Create" button located on the top left-corner by "Switch to classic".</li>
      <li>Choose the option "Azure virtual machine".</li>
      <li>Choose your subscription (For Ex: Azure Subscription 1).</li>
      <li>Create a name for the resource group (Use: AD-Lab).</li>
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
<img src="https://i.imgur.com/dHrRnV9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>    
        
4.Ensure that both VMs are in the same Vnet.
    <ol type="a">
      <li>To confirm that both VMs are located in the same region/virtual network, in the search bar type "Virtual Machines".</li>
      <li>Open both "DC-1" and "Client-1" by clicking the blue link located under "Name".</li>
      <li>Under Essentials, verify both VMs have the same "Virtual Network/subnet".</li>
    </ol>

Image Display of Step 2: 4C
<p>
<img src="https://i.imgur.com/eICyJny.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>  
  
<h2>Step 3: Ensure Connectivity between the client and Domain Controller</h2>

1.Log-in to Client-1 (VM) and ping DC-1's Private IP address with ping -t (perpetual ping). 
  <ol type="a">
      <li>Under Essentials, copy Client-1 Public IP address.</li>
      <li>Click on the "Start" button (Windows logo), then search for "Remote Desktop Connection" and open it. For Mac users download the app                 "Microsoft Remote Desktop" from the App Store.</li>
      <li>Paste the Public IP address (from your VM) in the computer name field and click "Connect". For Mac users paste the IP Address on "PC-               name" and click "add".</li>
      <li>Afterwards, make sure to log-in your credentials from Step 2 (Use Username: labuser/Password: Your unique password).</li>
      <li>For Windows users click "Yes" to connect to your VM. Observe the following display.</li>
      <li>Wait until your virtual machine logs you in.</li>
      <li>Then choose the following options for "Choose privacy settings for your device": </li>
      <li>Location: No, Diagnostic Data: No, Tailored experiences: No, Find my device: No, Inking and Typing: No, & Advertising ID: No.</li>
      <li>Click "Accept".</li>
      <li>Return to Azure (VM). Under Properties, copy DC-1's Private IP address.</li>
      <li>To ping the Private IP address of DC-1 within Client-1 (VM), go back to Client-1 (VM).</li>
      <li>After returning to Client-1 (VM), click on the "Start" button (Windows logo), then search for "Command Line" and open it.</li>
      <li>On the Command Line, type "ping -t DC-1 Private IP Address.</li>
      <li>The command line should display "Request Time Out" as a response, which could be attributed to the Windows Firewall settings of DC-1               blocking the incoming ping requests.</li>
    </ol>

Image Display of Step 3: 1A-C
<p>
<img src="https://i.imgur.com/iuqpEqv.png" height="80%" width="80%"/>
</p>
<p>  
 
Image Display of Step 3: 1D
<p>
<img src="https://i.imgur.com/iIvgaPu.png" height="80%" width="80%"/>
</p>
<p>  
 
Image Display of Step 3: 1J
<p>
<img src="https://i.imgur.com/dUPYiQp.png " height="80%" width="80%"/>
</p>
<p>  
    
Image Display of Step 3: 1L-M
<p>
<img src="https://i.imgur.com/EtXXIza.png" height="80%" width="80%"/>
</p>
<p>    
   
2.Login to the Domain Controller and enable ICMPv4 in on the local windows Firewall.
    <ol type="a">
      <li>While DC-1 keeps blocking incoming ping request, switch back to your Azure dashboard and initiate "DC-1" VM.</li> 
      <li>Under Essentials, copy DC-1 Public IP address.</li> 
      <li>Click on the "Start" button (Windows logo), then search for "Remote Desktop Connection" and open it.</li>
      <li>Paste the Public IP address(from DC-1) in the computer name field and click "Connect".</li>
      <li>Afterwards, make sure to log-in your credentials from Step 2 (Use Username: labuser/Password: Your unique password).</li>
      <li>On DC-1 (VM), navigate to the bottom-left corner and click on the Windows logo, then search for "wf.msc" and open it.</li>
      <li>Select "Inbound Rules" and expand the "Inbound Rules" panel for a better view.</li> 
      <li>Click on Protocol and find ICMPv4.</li> 
      <li>To enable the "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)" rule, right-click on it and select "Enable" for both the "Private" and "Domain"             profiles.</li>
    </ol>

- Note: Now you have two active Virtual Machines, Client-1 and DC-1. wf.msc stands for windows firewall. ICMP is used by the "ping" protocol. 

Image Display of Step 3: 2B-D
<p>
<img src="https://i.imgur.com/dJSI8G9.png" height="80%" width="80%"/>
</p>
<p>  

Image Display of Step 3: 2G-I
<p>
<img src="https://i.imgur.com/Z0AdAGK.png" height="80%" width="80%"/>
</p>
<p> 

3.Check back at Client-1 to see the ping succeed.
    <ol type="a"> 
      <li>After minimizing the DC-1 VM, return to the Client-1 VM. You will observe that the command line on Client-1 VM responds with "Reply from 10.0.0.4". This is             possible because we have enabled ICMPv4 communication from the DC-1 VM, allowing for the successful reply.</li> 
      <li>Stop the ping from happening by holding CTRL + C.</li>
    </ol>

Image Display of Step 3: 2G-I
<p>
<img src="https://i.imgur.com/ycPdkfP.png" height="80%" width="80%"/>
</p>
<p>

<h2>Step 4: Install Active Directory</h2>

1.Log-in to DC-1 and install Active Directory Domain Services.
    <ol type="a"> 
      <li>Log-in to DC-1.</li>
      <li>If your server manager is not open then click the Windows Logo button and search for "server manager".</li>
      <li>To install Active Directory, go on Server Manager and click "Add roles and features".</li>
      <li>Click "Next" for "Before You Begin", "Installation Type", and "Server Selection".</li> 
      <li>On Server Roles, allow "Active Directory Domain Services". Then click on "Add Features".</li> 
      <li>Click "Next" for "Server Roles", "Features", and "AD DS".</li>
      <li>Afterwards, click "Install".</li>
      <li>You should see an exclamation mark located at the top left-corner. Click that, and click again on "Promote this server to a domain controller".</li>
      <li>Under Select the deployment operation, click "Add a new forest".</li>
      <li>For Root domain name, use: mydomain.com </li> 
      <li>Click "Next" on "Deployment Configuration".</li> 
      <li>On Domain Controller Options, create/confirm you password (Use: Password1).</li>
      <li>Click "Next" on "Domain Controller Options", "DNS Options", "Additional Options", "Paths", "Review Options", and "Prerequisites Check".</li>
      <li>On the Installation section, click "Install".</li>
      <li>Reminder, when you launch the Remote Desktop Connection. with the context of the domain Make sure to log-in your credentials (Use Username:                             mydomain.com\labuser, Password: Your unique password).</li>
    </ol>

- Note: If you don't know what VM you are logged into, go to command line and enter "hostname". The command line should respond with "DC-1". After installing AD DS, it will not work properly until you setup your domain controller. You don't have to use "mydomain.com" as the root domain name. You can name it whatever you want. Your DC-1 (VM) will restart after the installation. If you get logged-out of your VM then log back to it.  

Image Display of Step 4: 1C
<p>
<img src="https://i.imgur.com/ChRcUMb.png" height="80%" width="80%"/>
</p>
<p>

Image Display of Step 4: 1E
<p>
<img src="https://i.imgur.com/HdHHk3W.png" height="80%" width="80%"/>
</p>
<p>

Image Display of Step 4: 1H
<p>
<img src="https://i.imgur.com/ma6GjTQ.png" height="80%" width="80%"/>
</p>
<p>
  
Image Display of Step 4: 1I
<p>
<img src="https://i.imgur.com/ZYIIRe5.png" height="80%" width="80%"/>
</p>
<p>
    
2.Promote as a DC: Setup a new forest as mydomain.com
    <ol type="a"> 
      <li>Under Select the deployment operation, click "Add a new forest".</li>
      <li>For Root domain name, use: mydomain.com </li> 
      <li>Click "Next" on "Deployment Configuration".</li> 
      <li>On Domain Controller Options, create/confirm you password (Use: Password1).</li>
      <li>Click "Next" on "Domain Controller Options", "DNS Options", "Additional Options", "Paths", "Review Options", and "Prerequisites Check".
          </li>
      <li>On the Installation section, click "Install".</li>
      <li>Reminder, when you launch the Remote Desktop Connection. with the context of the domain make sure to log-in your credentials (Use Username:                             mydomain.com\labuser, Password: Your unique password).</li>
    </ol>
    
3.Restart and then log back into DC-1 as user: mydomain.com\labuser








<h2>Step 5: Create an Admin and Normal User Account in AD</h2>

1.In Active Directory Users & Computers(ADUC), create an Organizational Unit(OU) called "_EMPLOYEES".
    <ol type="a"> 
     <li>  To open Active Directory, click the Windows Logo and search "Active Directory Administrative Center".</li>
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
    
<h2>Step 6: Join Client-1 to your domain (mydomain.com)</h2>

1. Log-in to Client-1 as the original local admin (use: labuser) & join it to the domain.
    <ol type="a"> 
      <li>If your Client-1 (VM) is closed, then initiate it from your Azure VM.</li> 
      <li>To verify the name of the VM, go to command line and type "hostname", it should respond as "Client-1".</li>     
      <li>Right-click the Windows logo button and click "System".</li>     
      <li>Under Related settings, click "Rename this PC (Advanced)".</li>  
      <li>Click "Change", it is located besides "To rename this computer or change its domain or workgroup click Change".</li>  
      <li>Click Domain and type "mydomain.com". Click "OK".</li>  
      <li>You will receive an error stating that "domain.com" could not be contacted.</li>
    </ol>

2. From the Azure Portal, set Client-1's DNS settings to the DC's Private IP address.
    <ol type="a"> 
      <li>Go back to Azure; Virtual Machines; DC-1. Under Settings, click on the "Networking" tab.</li> 
      <li>Look for your NIC Private IP and copy your private IP address.</li>     
      <li>Repeat 2a for Client-1 (VM). Click your client's network interface.</li>     
      <li>Under Settings, click "DNS servers". Also, click "Custom" to paste DC-1's private IP address in "Add DNS server".</li>  
      <li>Make sure to save.</li>  
    </ol>

3. From the Azure Portal, restart Client-1.
    <ol type="a"> 
      <li>Return to Azure; Virtual Machines; Client-1. Click the "Restart" button.</li> 
    </ol>

4. Login to Client-1 as the original local admin (labuser) and join it to the domain (computer will restart).
    <ol type="a"> 
      <li>Initiate remote connection with Client-1 (VM).</li>
      <li>Login as labuser.</li>
      <li>To check your username, hostname, & DNS settings, go to command line. Type "whoami", "hostname", & "ipconfig /all". If your command line           responds back with "client-1\labuser", "Client-1", "Edit". Then you are in the right VM.</li>
      <li>Right-click the Windows logo button and choose Systems. Click on "Rename this PC (advanced).</li>
      <li>Under Computer Name, hit "Change...". Click Domain and type "mydomain.com".</li>
      <li>After clicking "OK". Enter the name & password that has permission to join the domain (Use: mydomain.com\Terminator_admin).</li>
      <li>Afterwards your VM should restart.</li>
    </ol>

<h2>Step 7: Set-up Remote Desktop for non-administrative users on Client-1</h2>
1. Log into Client-1 as mydomain.com\jane_admin and open system properties
    <ol type="a"> 
      <li>Log-in as mydomain.com\Terminator_admin</li>
      <li>Login as labuser.</li>
    </ol>

2. Click “Remote Desktop”
    <ol type="a"> 
      <li>Right-click the Windows Logo and click on "System"</li>
      <li>Click "Remote Desktop"</li>
    </ol>

3. Allow “domain users” access to remote desktop
    <ol type="a"> 
      <li>Under User Accounts, click "Select users that can remotely access this PC.</li>
      <li>Click "Add". Under "Enter the object names to select", type "domain users" and click "Check Names". Then proceed by clicking "Okay".           </li>
      <li>Click "Okay" again.</li>
      <li>Log back to DC-1 (VM). Click the Windows Logo and search for "Active Directory Users and Computers. Once you open it, expand the panel.         </li>
      <li>Click mydomain.com; "Users". Find "Domain Users". Double-click it. Under Members, you can observe the following users to have access to             Client-1.</li>
    </ol>
    
4. You can now log into Client-1 as a normal, non-administrative user now

<h2>Step 8: Create a bunch of additional users and attempt to log into client-1 with one of the users</h2>

1. Login to DC-1 as Terminator_admin.
    <ol type="a"> 
      <li>To verify your VM, go to command line and type "whoami", & "hostname"</li>
      <li>It shoud respond back with "mydomain\Terminator_admin" & "DC-1".</li>
    </ol>

2. Open PowerShell_ise as an administrator.
    <ol type="a"> 
      <li>On your DC-1 (VM) click the Windows Logo and search for "Windows PowerShell ISE". Right-click it and choose "Run as administrator".</li>
    </ol>

3. Create a new File and paste the contents of the script into it.
    <ol type="a"> 
      <li>Click the link: https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1</li>
      <li>Then copy the entire code. Go back to PowerShell ISE, open a new file and paste the content of the script.</li>
      <li>Delete the following code line 47 & 48.</li>
    </ol>

- Note: This script will create 10,000 account users and they all will have the same password as "Password1". You will also notice that all the created accounts will be hosted under the "_EMPLOYEES" folder. Also these code will also randomize names.

4. Run the script and observe the accounts being created.
    <ol type="a"> 
      <li>Click the "Run Script" button.</li>
    </ol>

5. When finished, open ADUC and observe the accounts in the appropriate OU.
    <ol type="a"> 
      <li>Go back to "Active Directory Users and Computers" panel and right-click "_EMPLOYEES". Then click "Refresh". You should see the created             accounts.</li>
    </ol>

6. Attempt to log into Client-1 with one of the accounts (take note of the password in the script).
    <ol type="a"> 
      <li>Double-click a random account and under Account copy the employee's user logon name:</li>
      <li>Minimize DC-1 (VM) and go to Client-1 (VM).</li>
      <li>Log-out from Client-1 (VM). </li>
      <li>Sign-in to Client-1 (VM) with the employee's user logon name and use password as Password1.</li>
      <li>Open commmand-line and type "whoami" & "hostname"</li>
      <li>It should respond back with "mydomain\employee's user name" and "Client-1". </li>
    </ol>

7. Lock/unlock the employee's account.
    <ol type="a"> 
      <li>Use the same random username you created.</li>
      <li>Log-out from Client-1 (VM).</li>
      <li>Afterwards attempt to log-in backto Client-1 but make sure to use the wrong password 10 times so the system can lock the account.</li>
      <li>Go back to DC-1 and right-click the employee's username. Choose "Properties".</li>
      <li>Under Account, click the check-box "Unlock account". At the bottom click "Okay".</li>
      <li>You should be able to log-in if you input the correct password.</li>
      <li>If the user forgot the password, go back to DC-1 (Vm), right-click it, and click on "Reset Password" to create a new password for the               user.</li>
    </ol>

   
