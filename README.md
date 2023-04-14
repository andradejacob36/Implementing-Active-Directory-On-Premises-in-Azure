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

1. Click on the search bar and type "Virtual Machines".
2. Click on the "+ Create" button located on the top left-corner by "Switch to classic".
3. Choose the option "Azure virtual machine", enter the following information:
    <ol type="a">
      <li>Choose your subscription (For Ex: Azure Subscription 1).</li>
      <li>Create a new name for the resource group (Use: AD-Lab).</li>
      <li>Enter a unique name for the virtual machine (Use: DC-1).</li>
      <li>For "Region" use: (US) West US 3.</li>
      <li>For "Image" use: Windows Server 2022 Datacenter: Azure Edition Gen 2 (free services eligible).</li>
      <li>For "Size" use: Standard_E2s_v3 - 2 vcpus, 16 GiB memory. </li>
      <li>For "Username" use: labuser.</li>
      <li>For "Password" make sure to make up one.</li>
      <li>For "Public inbound ports" click on "Allow selected ports".</li>
      <li>For "Select inbound ports" use: RDP 3389.</li>
    </ol>

- Note: Remember to keep your username and password you created in your notepad, as you will need them later. Also, verify that your information is correct!

4. Click on the "Create" button to create the virtual machine.

<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Note: The processing of the VM is expected to take approximately 1-2 minutes. Once the process is complete, we will have successfully deployed the Domain Controller VM, which will act as the dedicated server for our Active Directory.

5. Create a client VM. 
      <ol type="a">
      <li>Choose your subscription (For Ex: Azure Subscription 1).</li>
      <li>Create a new name for the resource group (Use: AD-Lab).</li>
      <li>Enter a unique name for the virtual machine (Use: Client-1).</li>
      <li>For "Region" use: (US) West US 3.</li>
      <li>For "Image" use: - Windows 10 Pro, version 21H2 (free services eligible).</li>
      <li>For "Size" use: Standard_E2s_v3 - 2 vcpus, 16 GiB memory. </li>
      <li>For "Username" use: labuser.</li>
      <li>For "Password" make sure to make up one.</li>
      <li>For "Public inbound ports" click on "Allow selected ports".</li>
      <li>For "Select inbound ports" use: RDP 3389.</li>
    </ol>

6. Click on the "Create" button to create the virtual machine.

<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

- Note: 

7. Set Domain Controller's NIC Private IP address to be static.
    <ol type="a">
      <li>On the search bar, type "Virtual Machines".</li>
      <li>Click the blue link "DC-1" located under "Name".</li>
      <li>Navigate to the "Networking" option in the settings section. From there, you can select your network interface.</li>
      <li>Under settings, click on "IP configurations". Click the following IP configuration:</li>
      <li>Go to the "Assignment" section and select the option to set the IP address as static.</li>
      <li> Then click "Save".</li>
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

8. To confirm that both the Client-1 and Domain Controller VM are located in the same region and virtual network, you can easily return to the Virtual Machines dashboard by typing "Virtual Machines" in the search bar.
 
    <ol type="a">
      <li>Open both "DC-1" and "Client-1" by clicking the blue link located under "Name".</li>
      <li>Under Essentials, verify both VMs have the same "Virtual Network/subnet".</li>
    </ol>

<h2>Step 3: Ensure Connectivity between the client and Domain Controller</h2>

1. Log-in to Client-1 (VM) and ping DC-1's Private IP address with ping -t (perpetual ping). 
2. Under Essentials, copy Client-1 Public IP address.
3. Click on the "Start" button (Windows logo), then search for "Remote Desktop Connection" and open it. For Mac users download the app "Microsoft Remote Desktop" from the App Store.
4. Paste the Public IP address(from your VM) in the computer name field and click "Connect". For Mac users paste the IP Address on "PC-name" and click "add".
5. Afterwards, make sure to log-in your credentials from Step 2 (Use Username: labuser/Password: Your unique password).
6. For Windows users click "Yes" to connect to your VM. Observe the following display. 
 
 <p>
<img src="https://i.imgur.com/So0Dn0n.png" height="80%" width="80%"/>
</p>
<p>  
 
<p>
<img src="https://i.imgur.com/xHG3t9h.png" height="80%" width="80%"/>
</p>
<p>  
 
7. Wait until your virtual machine logs you in.
8. Then choose the following options for "Choose privacy settings for your device": 
    <ol type="a">
      <li>Location: No </li>
      <li>Diagnostic Data: No</li>
      <li>Tailored experiences: No</li>
      <li>Find my device: No</li>
      <li>Inking and Typing: No</li>
      <li>Advertising ID: No</li>
    </ol>
9. Click "Accept"
10. Return to Azure to find DC-1 Private IP address and copy it. 
11. Go back to your VM in order to ping the private IP address of DC-1. 

    <ol type="a">
      <li>Navigate to the bottom-left corner and click on the "Start" button (Windows logo), then search for "Command Line" and open it.</li>
      <li>On the Command Line type "ping -t DC-1 Private IP Address.</li>
      <li>The Command Line should display "Request Time Out" as a response, which could be attributed to the Windows Firewall settings of DC-1               blocking the incoming ping requests.</li>
      <li>Switch back to your Azure dashboard and initiate "DC-1" VM. </li>
    </ol>

12. While DC-1 keeps blocking incoming ping request, switch back to your Azure dashboard and initiate "DC-1" VM.
    <ol type="a">
      <li>Under Essentials, copy DC-1 Public IP address.</li>
      <li>Click on the "Start" button (Windows logo), then search for "Remote Desktop Connection" and open it.</li>
      <li>Paste the Public IP address(from DC-1) in the computer name field and click "Connect".</li>
      <li>Afterwards, make sure to log-in your credentials from Step 2 (Use Username: labuser/Password: Your unique password).</li>
    </ol>

- Note: Now you have two active Virtual Machines, Client-1 and DC-1.

13. On DC-1 (VM), navigate to the bottom-left corner and click on the Windows logo, then search for "wf.msc" and open it.

- Note: wf.msc stands for windows firewall. 

14. Please select "Inbound Rules" and be sure to expand the "Inbound Rules" panel for a better view.
15. Click on Protocol and find ICMPv4

-Note: ICMP is used by the "ping" protocol. 

16. To enable the "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)" rule, right-click on it and select "Enable" for both the "Private" and "Domain" profiles.

17. After minimizing the DC-1 VM, return to the Client-1 VM. You will observe that the command line on Client-1 VM responds with "Reply from ...". This is possible because we have enabled ICMPv4 communication from the DC-1 VM, allowing for the successful reply.

18. Stop the ping from happening by holding CTRL + C. 

<h2>Step 4: Install Active Directory</h2>

1. Log-in to DC-1 and install Active Directory Domain Services 
2. Promote as a DC: Setup a new forest as mydomain.com
3. Restart and then log back into DC-1 as user: mydomain.com\labuser
4. Create organizational units inside of Active Directory


Log-in to DC-1 

- Note: If you don't know what VM you are loged into, go to command line and enter "hostname". The command line should either respond with DC-1 or Client-1. 

If your server manager is not open then click the Windows Logo button and search for "server manager". 

To install Active Directory, go on Server Manager and click "Add roles and features".

Click "Next" for "Before You Begin", "Installation Type", and "Server Selection". 

On Server Roles, allow "Active Directory Domain Services". Then click on "Add Features".

Click "Next" for "Server Roles", "Features", and "AD DS". 

Afterwards, click "Install" .

- Note: After installing AD DS, it will not work properly until you setup your domain controller.

You should see an exclamation mark located at the top left-corner. Click that, and click again on "Promote this server to a domain contoller".

Under Select the deployment operation, click "Add a new forest". 

For Root domain name, use: mydomain.com

- Note: You don't have to use "mydomain.com" as the root domain name. You can name it whatever you want.

Click "Next" on "Deployment Configuration".

On Domain Controller Options, create/confirm you password (Use: Password1).

Click "Next" on "Domain Controller Options", "DNS Options", "Additional Options", "Paths", "Review Options", and "Prerequisites Check".

On the Installation section, click "Install". 

- Note: Your DC-1 (VM) will restart after the installation. If you get logged-out of your VM connect back to it.  

Reminder, when you launch the Remote Desktop Connection. with the context  of the domain Make sure to log-in your credentials (Use Username: mydomain.com\labuser, Password: Your unique password). 

To open Active Directory, click the Windows Logo and search "Active Directory Administrative Center". 

<h2>Step 5: Create an Admin and Normal User Account in AD</h2>


Right-click on mydomain.com, click on "New" and "Organizational Unit".

Create a folder by naming it "_EMPLOYEES" and click "Okay". 

Do the same for "_ADMINS". Right-click on mydomain.com and click "Refresh".  

To create our own admin account, right-click on "_ADMINS" and click on "New" and "User".

- Note: You can make up your own name/last name.

I inputed the following information as First Name: Terminator, Last Name: 3000, Full Name: Terminator 3000, User logon name: Terminator_admin. Afterwards click "Next" 

Create/Confirm your Password (Use your unique Password). Uncheck "User must change password at next logon", and check on "Password never expires".  

-Note: Remember to keep your "User logon name" in your notepad, as you will need them later.

Right-click on your created account and choose "Properties". Go to "Member Of" section.    

Click "ADD". Under Enter the object names to select, type "domain" and click on "Check Names".

Pick the "Domain Admins" group. Then click "OK", "Apply", and "OK". 

36:00
