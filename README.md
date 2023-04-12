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
      <li>To access the networking settings, simply navigate to the "Networking" option in the settings section. From there, you can select your    network interface, which is located next to "Effective Security Rules".</li>
      <li>Under settings, click on "IP configurations". Click the following IP configuration:</li>
      <li>To change the IP address assignment method from dynamic to static, go to the "Assignment" section and select the option to set the IP
          address as static.</li>
      <li>On the upper-left corner click "Save".  </li>
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

1. Login to Client-1 and ping DC-1's IP address with ping -t (perpetual ping). 
2. Under Essentials, copy "Client-1" Public IP address.
3. Navigate to the bottom-left corner and click on the "Start" button (Windows logo), then search for "Remote Desktop Connection" and open it. For Mac users download the app "remote- Microsoft Remote Desktop" from the App Store.
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
10. Return to Azure to find "DC-1" Private IP address and copy it. 
11. Go back to your virtual machine (VM) in order to ping the private IP address of "DC-1". This will allow you to check the connectivity and communication between Client-1 and DC-1.    

    <ol type="a">
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
      <li></li>
    </ol>
