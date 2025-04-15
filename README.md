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
