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

- Step 1: You need to authenticate and authorize yourself by logging into the Azure portal.
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

- Note: We have successfully deployed the Domain Controller VM, which will serve as the server for our Active Directory.

<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>


