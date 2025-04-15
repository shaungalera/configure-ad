<p a href="center">

![image](https://github.com/user-attachments/assets/241b9cf6-2960-4db8-b992-6e5bbf4aa963)
</p>
<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.

<h2>Environments and Technologies Used</h2>
. Microsoft Azure (Virtual Machines/Compute)<br>
. Remote Desktop<br>
. Active Directory Domain Services<br>
. PowerShell<br>

<h2>Operating Systems Used</h2>

. Windows Server 2022<br>
. Windows 10 (22H2)<br>

<h2>High-Level Deployment and Configuration Steps</h2>

. Preparing the AD Infrastructure in Azure<br>
. Deploying Active Directory<br>
. Creating Users with PowerShell<br>
. Group Policy and Managing Accounts<br>

<h1>Deployment and Configuration Steps</h1>

<h2>Part 1: Preparing the AD Infrastructure in Azure</h2>

<h3>Setup Domain Controller in Azure</h3>

. ***Create a Resource Group***:<br>
- Navigate to the Azure Portal and create a new Resource Group for the lab environment.<br>
![image](https://github.com/user-attachments/assets/e128152d-4143-4be7-8c18-a79ca62efa24)

. **Create a Virtual Network and Subnet**:<br>
- Set up a Virtual Network with a subnet to host your VMs.<br>

. **Create the Domain Controller VM (Windows Server 2022)**:<br>
- Name the VM: DC-1.<br>
- Ensure that the VM is on the Virtual Network created previously.<br>

![image](https://github.com/user-attachments/assets/909d20cd-b8bd-451f-9d0f-2261e564e4b4)

. **Set Static Private IP for DC-1**: - After the VM is created, navigate to its Network Interface Card (NIC) settings and set the private IP to static.<br>

- **Navigate to the Virtual Machines window and select the DC-1 VM**<br>

- **Navigate to the Network Setting to access the Network Interface Card**<br>

. **Set the Allocation to Static underneath the Private IP Address Settings**<br>

![image](https://github.com/user-attachments/assets/1ac051e9-d9e7-4528-85cb-0073199cb5db)

. **Disable Windows Firewall**:<br>

- Log in to DC-1 and disable the Windows Firewall for testing connectivity.<br>

![image](https://github.com/user-attachments/assets/62e1dac2-9411-4c68-9d50-5bcd855127fd)

**Setup Client-1 in Azure**

. **Create the Client VM (Windows 10 22H2)**:<br>
- Name the VM: Client-1.<br>

. **Attach Client-1 to the Same Region and Virtual Network**:<br>
- Ensure it is in the same Virtual Network and subnet as DC-1<br>

![image](https://github.com/user-attachments/assets/7915d0eb-2649-4268-a17c-ee0ab927a709)

. **Set DNS Settings**:<br>
- Update Client-1's DNS settings to point to DC-1's private IP address. (navigate to the vm's network interface card)<br>

![image](https://github.com/user-attachments/assets/941ef9ab-06e1-4e8b-ba0f-93e8351adb06)

. **Test Connectivity**:<br>

- Restart Client-1 from the Azure Portal.<br>
- Log into Client-1 and use the ping command to test connectivity with DC-1.<br>

![image](https://github.com/user-attachments/assets/d231def4-61ce-44ac-bf6e-902c22855731)

. **Verify DNS Settings**:<br>

- Run ipconfig /all in PowerShell on Client-1 to ensure the DNS points to DC-1.<br>

![image](https://github.com/user-attachments/assets/a31d76ec-9567-470e-8f8e-310f90dcfb33)

<h2>Part 2: Deploying Active Directory</h2>

<h3>Install Active Directory<h3></h3>

1.)Log in to DC-1.<br>
2.) Install Active Directory Domain Services (AD DS).<br>
3.) Promote DC-1 as a Domain Controller and set up a new forest (e.g., mydomain.com).<br>
4.) Restart DC-1 and log in as mydomain.com\labuser.<br>
5.) Open Server Manager then add roles and features<br>
6.) Add the features from the Active Directory Domain Services<br>
7.) Open the notification window and select "promote this server to a domain controller"<br>
8.) Add mydomain.com as a new forest<br>
9.) Deselect "Create DNS delegation<br>
10.) Finish the setup wizard and install<br>

![image](https://github.com/user-attachments/assets/d5d03b90-5983-47a6-bb9e-e9f683ca5e22)

.**The DC-1 will automatically restart**<br>
. **DC-1 is a domain now, in order to complete the next steps, we will have to login using the proper domain context (mydomain.com\labuser will be our username - same passwoord)**<br>


<h3>Create a Domain User</h3>

1.) Open Active Directory Users and Computers (ADUC).<br>
2.) Create an Organizational Unit (OU) named _EMPLOYEES.<br>

![image](https://github.com/user-attachments/assets/4cf24435-4c58-4c57-9688-9d9cc7271b50)

3.) Create another OU named _ADMINS.<br>

![image](https://github.com/user-attachments/assets/b86aa3aa-f0ce-4ee4-b5ec-70ad3f2201f0)

4.) Add a new user:<br>
- Name: Jane Doe<br>
- Username: jane_admin<br>
- Password: Cyberlab123!<br>

5.) Add jane_admin to the Domain Admins security group.<br>
6.) Log out and log back in as mydomain.com\jane_admin.<br>

![image](https://github.com/user-attachments/assets/ae4364aa-ae36-47c7-b151-1b8e01d63bdc)

**Join Client-1 to the Domain**<br />

1.) Log in as the local admin and join Client-1 to the domain.<br>
2.) Create a new OU titled '_CLIENTS' & add Client-1 in ADUC to _CLIENTS.<br>
3.) Log into DC-1 as Jane the Admin<br>
4.) Log into client-1 as labuser<br>
5.) Navigate to the system window by right clicking the windows button<br>

. **Join Client-1 to the domain by using the 'rename this pc' tool**<br>
![image](https://github.com/user-attachments/assets/662ac25a-48f7-46d5-af73-58eea7ce02fa)

. Verify that Client-1 has joined the domain<br>
. Create a new folder named '_CLIENTS' and drag/drop the Client-1 computer into it<br>

<h2>Part 3: Creating Users with Powershell</h2>

**Setup Remote Desktop for Domain Users**<br>

1.) Log into Client-1 as mydomain\jane_admin.<br>
2.) Open System Properties and enable Remote Desktop.<br>
3.) Allow "domain users" access to Remote Desktop.<br>

![image](https://github.com/user-attachments/assets/e28f953d-d131-4350-9253-fbfb9696cdad)


**Create Users with PowerShell**<br>

1.) Log in to DC-1 as jane_admin.<br>
2.) Open PowerShell ISE as an administrator.<br>
3.)Create multiple new users using a script (script link: https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1).<br>
4.) Verify users appear in the _EMPLOYEES OU in ADUC.<br>
5.) Attempt to log into Client-1 with one of the created accounts.<br>

![image](https://github.com/user-attachments/assets/2a0d03a0-3eaa-4779-8ee5-e1cf62949f19)
![image](https://github.com/user-attachments/assets/1fcb9fd7-661f-4390-83aa-c093e81fee08)
![image](https://github.com/user-attachments/assets/e0a0365d-33d8-44dc-815b-5e26a07afcba)
![image](https://github.com/user-attachments/assets/4a1f0bce-c683-4c5f-849e-e8c010895845)

. **Log into Client 1 using one of the created accounts**<br>

<h2>Part 4: Group Policy and Managing Accounts</h2>

Account Lockout Configuration

Log in to DC-1.
Open Group Policy Management.
Edit the Default Domain Policy:
Set account lockout threshold to 5 invalid attempts.
Attempt to log in with a user account using incorrect passwords. Observe the account lockout behavior.
Unlock the account in ADUC and reset the password.
