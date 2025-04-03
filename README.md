<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This guide outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop (RDP)
- Active Directory Domain Services (ADDS)
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (22H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Create Resources
- Ensure Connectivity between Client and Domain Controller
- Install Active Directory
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into Client-1 with one of the users

<h2>Deployment and Configuration Steps</h2>

The first step is to create a resource group inside azure.

![image](https://github.com/user-attachments/assets/bf3c33af-1a67-4bc5-95fd-fff5027f07df)

We're then going to create a Virtual Network (VNet) inside that resource group.

![image](https://github.com/user-attachments/assets/0833969b-12cd-49b8-a2d2-36d672a6986e)

Now we'll create our Domain Controller. Name it "**dc-1**". When creating, make sure to select "**Windows Server 2022**".

![image](https://github.com/user-attachments/assets/11912d0a-48c9-4d9f-9de9-62c2598ffff6)

Now create the client vm. Name it "**client-1**". Select "**Windows 10 Pro**".

![image](https://github.com/user-attachments/assets/43296530-1929-42ac-b7fb-ad3e035c2009)

Set the Domain Controllers NIC Private IP to be static.

![image](https://github.com/user-attachments/assets/1daa691e-e6b2-425e-b9a8-057181919973)

Set the clients DNS settings to point to dc-1's Private IP Address.

![image](https://github.com/user-attachments/assets/9803e958-a005-44c0-89ee-db1b88e430c5)

Remote into dc-1 using the username and password you chose when making the VM.

![image](https://github.com/user-attachments/assets/5b5dd756-31d6-4c64-b194-4af2fdcbaad1)

Next open Windows Firewall Settings. (You can type "wf.msc" in run to open it.) Inside the Firewall Settings, we're going to enable ICMP. </p>
Alternatively, for testing purposes you could disable the Firewall. However, this isn't recommended to do in real life.

![image](https://github.com/user-attachments/assets/d88fa59a-3cb8-4ef4-be4e-16aeff715237)

Next remote into client-1 using the username and pass you chose when creating the VM.

![image](https://github.com/user-attachments/assets/6e3b6723-21f8-47e6-a040-85d9ff176582)

Open CMD, and ping dc-1's private IP Address to ensure connection. 

![image](https://github.com/user-attachments/assets/a35c2f3a-2074-4167-8a99-7a4eccaef99f)













