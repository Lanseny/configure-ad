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

- Step 1: Set Up Your Azure Environment
- Step 2: Install Active Directory Domain Services (AD DS)
- Step 3: Configure DNS and Networking
- Step 4: Join Additional VMs to the Domain (Optional)
- Step 5: Configure Group Policies and Organizational Units (OUs)
- Step 6: Verify and Test

<h2>Deployment and Configuration Steps</h2>

<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  <h2>Step 1: Set Up Your Azure Environment</h2>

1. **Log in to the Azure Portal:**
   - Navigate to [Azure Portal](https://portal.azure.com) and log in with your credentials.

2. **Create a Resource Group:**
   - Go to **Resource Groups** > **Add**.
   - Enter a name for your resource group, select a region, and click **Review + Create**.

3. **Create a Virtual Network:**
   - Go to **Virtual Networks** > **Add**.
   - Enter the name, address space (e.g., 10.0.0.0/16), and the same region as your resource group.
   - Configure the subnet (e.g., 10.0.0.0/24) and click **Review + Create**.

4. **Create Network Security Group (NSG):**
   - Go to **Network Security Groups** > **Add**.
   - Name your NSG, associate it with your resource group, and click **Review + Create**.

5. **Create a Virtual Machine:**
   - Go to **Virtual Machines** > **Add**.
   - Configure the VM (e.g., Windows Server 2019 Datacenter), choose the same resource group, and ensure it's in the same virtual network and subnet.
   - Set an appropriate VM size, administrator username, and password.
   - Under the **Management** tab, disable boot diagnostics if not needed.
   - Click **Review + Create**.
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h2> Step 2: Install Active Directory Domain Services (AD DS)</h2>

1. **Connect to the VM:**
   - Use Remote Desktop Protocol (RDP) to connect to the VM with the administrator credentials.

2. **Install AD DS:**
   - Open **Server Manager**.
   - Click on **Add Roles and Features**.
   - In the wizard, select **Role-based or feature-based installation** and choose your VM.
   - Select **Active Directory Domain Services** and add required features.
   - Continue through the wizard and install.

3. **Promote the Server to a Domain Controller:**
   - After installation, click the **Promote this server to a domain controller** link.
   - Choose **Add a new forest** and specify the root domain name (e.g., `yourdomain.local`).
   - Set the Directory Services Restore Mode (DSRM) password.
   - Continue through the wizard, accepting default paths, and complete the installation.
   - The VM will restart after configuration.
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h2> Step 3: Configure DNS and Networking</h2>

1. **Update DNS Settings:**
   - Go to **Azure Portal** > **Virtual Machines** > your VM > **Networking** > **NIC (Network Interface)**.
   - Set the DNS server to the IP address of your domain controller (typically the VM's IP).

2. **Configure Firewall Rules:**
   - Go to **Network Security Groups** > your NSG > **Inbound security rules** > **Add**.
   - Add rules to allow traffic on ports required by Active Directory (e.g., 53 for DNS, 88 for Kerberos, 389 for LDAP, 636 for LDAPS, 3268 for Global Catalog).
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h2>Step 4: Join Additional VMs to the Domain (Optional)</h2>

1. **Create Additional VMs:**
   - Follow the same steps as Step 1 to create more VMs within the same virtual network and subnet.

2. **Join VMs to the Domain:**
   - Connect to the additional VMs via RDP.
   - Right-click **This PC** > **Properties** > **Change settings** > **Change**.
   - Select **Domain** and enter your domain name (e.g., `yourdomain.local`).
   - Enter domain administrator credentials when prompted.
   - Restart the VMs after joining the domain.
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h2>Step 5: Configure Group Policies and Organizational Units (OUs)</h2>

1. **Open Group Policy Management:**
   - On the domain controller, open **Server Manager** > **Tools** > **Group Policy Management**.

2. **Create Organizational Units (OUs):**
   - In Group Policy Management, right-click your domain > **New Organizational Unit**.
   - Name the OU and click OK.

3. **Create and Link Group Policies:**
   - Right-click the OU > **Create a GPO in this domain, and Link it here**.
   - Name the GPO and click OK.
   - Configure the GPO settings as needed (e.g., security settings, software deployment).
<img src="https://i.imgur.com/DJmEXEB.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<h2>Step 6: Verify and Test</h2>

1. **Verify Domain Controller Functionality:**
   - Ensure the domain controller is running properly by opening **Active Directory Users and Computers** and checking the domain structure.

2. **Test Domain Join:**
   - Log in to the additional VMs with domain credentials to verify they have successfully joined the domain.

3. **Check Group Policy Application:**
   - On a domain-joined VM, run `gpresult /r` to check the applied group policies.

<h2>Conclusion<h2>

By following these steps, you have successfully configured an on-premises Active Directory environment within Azure Virtual Machines. This setup allows for centralized management of users, groups, and resources, leveraging the scalability and flexibility of Azure. This guide serves as a comprehensive demonstration of your ability to manage Active Directory services in a cloud environment, suitable for inclusion in your professional portfolio.
