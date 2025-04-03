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
- Ensure Connectivity between Client-1 and Domain Controller
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

Next open Windows Firewall Settings. (You can type "wf.msc" in run to open it.) Inside the Firewall Settings, we're going to enable ICMPv4. </p>

Alternatively, for testing purposes you could disable the Firewall. However, this isn't recommended to do in real life.

![image](https://github.com/user-attachments/assets/d88fa59a-3cb8-4ef4-be4e-16aeff715237)

Next remote into client-1 using the username and pass you chose when creating the VM.

![image](https://github.com/user-attachments/assets/6e3b6723-21f8-47e6-a040-85d9ff176582)

Open CMD, and ping dc-1's private IP Address to ensure connection. 

![image](https://github.com/user-attachments/assets/a35c2f3a-2074-4167-8a99-7a4eccaef99f)

# Installing Active Directory </h2>

Log into dc-1, and install Active Directory Domain Services (ADDS). 

![image](https://github.com/user-attachments/assets/7ab06228-7e90-4032-9ba4-023144bd3859)

After ADDS has been installed, we're going to promte the server to a Domain Controller.

![image](https://github.com/user-attachments/assets/e6001a0a-7838-4d14-9481-7ad7b518bb80)

We're going to add a new forest, and choose a root domain name. It can be anything as long as you'll remember it.

![image](https://github.com/user-attachments/assets/f7423671-178d-42d6-84bf-b1a16f140866)

After installation, our server is going to restart. Login with your domain.</p>

In my case, "lothlorien.com\labuser".

![image](https://github.com/user-attachments/assets/77f2b73e-d9fb-4f38-9d59-cd662bdb4c63)

After logging in, we need to open Active Directory Users and Computers (ADUC). Add an Organization Unit (OU), name it "**_EMPLOYEES**". Then create another and name it "**_ADMINS**".

![image](https://github.com/user-attachments/assets/5bf4f244-d792-4c87-9d40-6c9d36519a72)

![image](https://github.com/user-attachments/assets/b683e33b-d12f-44c3-850d-3aa81dba6ae7)

Now create a user in the admins folder, name it something you'll remember. This is going to be our admin account. </p>

For testing purposes, I chose to have a password that never expires. This is bad to do in real life.

![image](https://github.com/user-attachments/assets/e025d2f0-8322-4787-9f65-d0b572d94066)

![image](https://github.com/user-attachments/assets/ef8b2b4c-ca69-4eff-889f-82afebec7ec3)

Make "Ryan Doe" a member of the Domain Admins Security Group.

![image](https://github.com/user-attachments/assets/3add6b99-b8ac-4661-9281-6ca70601110d)

Logout of dc-1, and from now on, login using the admin account you created.</p>

In my case, "ryan_admin".

![image](https://github.com/user-attachments/assets/49b6ba21-b1ec-4803-8df8-96e8d43f4986)

# Joining the Domain </h2>

Log into client-1 using the original local admin account, and join the domain.

![image](https://github.com/user-attachments/assets/efc92937-bb93-475e-a37c-f3c1885d0ceb)

![image](https://github.com/user-attachments/assets/d8e9b91f-4ca4-4a8b-bafc-cc6ed23b9083)

Now back inside of dc-1, verify that client-1 shows up in your forest under the computers container.

![image](https://github.com/user-attachments/assets/a84ca318-bcb6-4602-8962-bcb972225699)

For organizational purposes, create a new OU and name it "**_CLIENTS**", then drag client-1 into that OU.

![image](https://github.com/user-attachments/assets/cd51e03c-7819-4323-bbee-9fc6286b6320)

# Setup Remote Desktop for Non-admin users on client-1</h2>

Log into client-1 as "ryan_admin" and open system properties.</p>

Click "Remote Desktop", allow "Domain Users" access.

We're now able to login as normal, non-admin users now.

![image](https://github.com/user-attachments/assets/60b10384-5f97-4b12-aef4-fb66683d8c2d)

# Creating Users using Powershell</h2>

Using our admin account in dc-1, open Powershell_ise as an admin.</p>

Create a new file, and paste the contents of [this script.](https://github.com/noahp-prog/configure-active-directory/blob/main/ad-script)

![image](https://github.com/user-attachments/assets/1b5d394d-5e42-427e-8f74-c0fb648df93f)


























