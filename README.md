<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

1. Setting up virtual machines on Azure.
2. Configuring networking and connectivity between the virtual machines.
3. Installing and configuring AD operating system on the virtual machines.
4. Creating an Admin and normal user acount in AD.
5. Joining client machines to the virtualized Active Directory domain.


<h2>Deployment and Configuration Steps</h2>

<p>
<img width="80%"  Height="80%"alt="Step 1" src="https://github.com/davidrosa01/davidrosa01/assets/119774564/63f558dd-5c8f-4cc9-b861-d02cd33c8894">
</p>
<p>
In the initial step, we set up the necessary resources in Azure to implement an on-premises Active Directory infrastructure. This involved creating a Domain Controller VM (Windows Server 2022) named "DC-1" and noting the Resource Group and Virtual Network (VNet) that were automatically generated. Additionally, we ensured the Domain Controller's NIC had a static Private IP address. Next, we created a Client VM (Windows 10) named "Client-1" within the same Resource Group and VNet. To confirm the VMs were in the same VNet, we utilized Azure Network Watcher or checked the VNet settings in the Azure portal. This initial step lays the foundation for configuring the Domain Controller and connecting the client machine to the Active Directory domain in subsequent steps.
</p>
<br />

<p>
<img width="80%"  Height="80% alt="step 2" src="https://github.com/davidrosa01/davidrosa01/assets/119774564/d1796dee-10d3-4f1e-82de-6c518c36d05a">
</p>
<p>
In the next step, we focused on ensuring connectivity between the Client-1 and the Domain Controller (DC-1) in the Azure environment. First, we logged in to Client-1 using Remote Desktop and initiated a perpetual ping to DC-1's private IP address using the command "ping -t <ip address>". This allowed us to monitor the connection status continuously. Next, we accessed the Domain Controller and enabled ICMPv4 in the local Windows Firewall to allow ping requests. Returning to Client-1, we observed the successful completion of the ping, indicating a successful connection between the client and the Domain Controller. This step ensures that the client and the Domain Controller can communicate effectively, laying the groundwork for subsequent configuration tasks in the Active Directory setup.
</p>
<br />

<p>
<img width="80%" height="80%" alt="better step 3" src="https://github.com/davidrosa01/davidrosa01/assets/119774564/ba535803-9e1b-458e-9be1-f9fc6aa9d023">
</p>
<p>
In the following step, we proceeded with the installation of Active Directory on the DC-1 virtual machine:

1. We logged in to the DC-1 virtual machine using the appropriate credentials.
2. Once logged in, we initiated the installation process for Active Directory Domain Services. This involved accessing the Server Manager, selecting "Add roles and features," and selecting the Active Directory Domain Services role.
3. We then promoted DC-1 as a domain controller by selecting the option to "Add a new forest" and specifying a domain name, such as "mydomain.com" (note that you can choose any appropriate domain name).
4. After the promotion process was completed, we restarted the DC-1 virtual machine.
5. Following the restart, we logged back into DC-1 using the user account "mydomain.com\labuser."

By completing these steps, Active Directory Domain Services was successfully installed on the DC-1 virtual machine and promoted as a domain controller for the specified forest, facilitating the management of users, groups, and other resources within the domain.
</p>
<br />


<p>
<img width="80%" height="80%" alt="Step 4" src="https://github.com/davidrosa01/davidrosa01/assets/119774564/156ce878-8b47-47ba-a9d1-9243065a8c9a">
</p>
<p>
In this step, we created an administrative and normal user account within Active Directory. Using the Active Directory Users and Computers (ADUC) management console, we established an "_EMPLOYEES" Organizational Unit (OU) and an "_ADMINS" OU for administrative accounts. We added "Jane Doe" as an employee with the username "jane_admin" and assigned the same password. "jane_admin" was then added to the "Domain Admins" Security Group for administrative privileges. After logging out and logging back in as "mydomain.com\jane_admin," we utilized this account for administrative tasks within the Active Directory environment.
</p>
<br />


<p>
<img width="80%" height="80%"  alt="step 5" src="https://github.com/davidrosa01/davidrosa01/assets/119774564/c0aa6604-dd13-4fa5-903b-ebec830fcb46">
</p>
<p>
In this step, we joined Client-1 to the "mydomain.com" domain and organized it within the Active Directory environment. We configured Client-1's DNS settings to point to the DC's private IP address through the Azure Portal and restarted the machine. Using Remote Desktop, we logged in to Client-1 as the original local admin, "labuser," and joined it to the domain, which triggered a restart. Upon logging in to the Domain Controller, we verified Client-1's presence in the "Computers" container of the Active Directory Users and Computers (ADUC) tool at the root of the domain. Additionally, we created an "_CLIENTS" OU within ADUC and moved Client-1 into it for organizational purposes.
</p>
<br />

<p>
<img width="80%" height="80%" alt="step 6" src="https://github.com/davidrosa01/davidrosa01/assets/119774564/ae1a50d0-e3b5-4414-ab6d-d9a13a6d41c6">
</p>
<p>
In order to grant Remote Desktop access to non-administrative users on Client-1, we followed the following steps. Initially, we logged into Client-1 using the administrative account "mydomain.com\jane_admin" and accessed the system properties. Inside the system properties window, we navigated to the "Remote Desktop" tab. Subsequently, we allowed access to Remote Desktop for "domain users" by selecting the appropriate option. With this configuration in place, normal users who are not administrators can now utilize Remote Desktop to log into Client-1. It is important to note that in larger environments, it is recommended to utilize Group Policy for enabling Remote Desktop access across multiple systems simultaneously, providing a more efficient and centralized management approach. However, for the purposes of this lab, the manual configuration performed on Client-1 sufficed.
</p>
<br />
