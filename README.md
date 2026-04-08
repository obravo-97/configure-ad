<p align="center">
  <img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

# On-Premises Active Directory Deployed in the Cloud (Azure)

This project documents the deployment of a traditional **on-premises Active Directory environment** hosted within **Microsoft Azure Virtual Machines**.  
The goal is to demonstrate foundational identity, authentication, and domain management concepts using cloud-based infrastructure while preserving classic on-prem AD architecture.

---

## Environments and Technologies Used

- Microsoft Azure (Virtual Machines / Compute)
- Azure Virtual Network
- Remote Desktop (RDP)
- Active Directory Domain Services (AD DS)
- DNS
- PowerShell

---

## Operating Systems Used

- Windows Server 2022 (Domain Controller)
- Windows 10 (21H2) (Domain-joined client)

---

## 1) Create Azure Virtual Machines

Create two Azure virtual machines.

### Domain Controller (DC)

- OS: Windows Server 2022
- VM Size: Minimum 2 vCPUs
- Name: DC-01
- Virtual Network: Allow Azure to create a new Virtual Network during VM creation
- Private IP: Static (important)

### Client Machine

- OS: Windows 10 (21H2)
- Name: CLIENT-01
- Virtual Network: Select the existing Virtual Network created with DC-01

> When creating the client machine, ensure it is placed in the same Virtual Network and subnet as the domain controller.

Both virtual machines must reside on the same virtual network to allow proper Active Directory domain communication.

<img width="1871" height="647" alt="image" src="https://github.com/user-attachments/assets/c537aa05-5c5f-4d70-8ac2-ae6742e07cb5" />

<img width="1687" height="554" alt="image" src="https://github.com/user-attachments/assets/e88321c1-d6e5-400a-bc29-6d80b285bcc4" />


---

## 2) Configure the Domain Controller

### Install Active Directory Domain Services

1. Log into DC-01 via Remote Desktop
2. Open Server Manager
3. Select add Roles and Features
4. Install:
   - Active Directory Domain Services
   - DNS Server 
5. Complete the installation

<img width="979" height="696" alt="image" src="https://github.com/user-attachments/assets/19659751-aaed-483d-ae59-62a0b156ac9c" />

---

## 3) Promote Server to Domain Controller

1. In Server Manager, click the AD DS notification flag
2. Select Promote this server to a domain controller
3. Choose Add a new forest
4. Set the domain name to corp.local
5. Set the Directory Services Restore Mode (DSRM) password
6. Complete the wizard and reboot the server

<img width="1898" height="934" alt="image" src="https://github.com/user-attachments/assets/4451c1bf-aa2b-407b-9fa7-fdfe2b8a6eb5" />
<img width="951" height="700" alt="image" src="https://github.com/user-attachments/assets/87001bcf-3500-494e-aef7-4a0600fb74e6" />
<img width="951" height="698" alt="image" src="https://github.com/user-attachments/assets/89db7d01-b51e-48b2-81f1-efabe6d641f9" />

After reboot, DC-01 is now a domain controller.

---

## 4) Create Organizational Units, Users, and Groups

1. Open Active Directory Users and Computers
2. Create Organizational Units (OUs), for example:
   - IT
   - Employees
   - Group Computers
<img width="954" height="667" alt="image" src="https://github.com/user-attachments/assets/445093b1-38b5-4153-b127-c965c1875a4b" />

3. Create a small set of test domain users, for example:
   - jdoe (standard user)
   - asmith (standard user)
   - itadmin (IT role)
     
4. Create security groups and assign users
<img width="1200" height="702" alt="image" src="https://github.com/user-attachments/assets/74914dc3-36e5-4704-9b40-73416f8f6e86" />

   - In Active Directory Users and Computers, create security groups within the appropriate OUs (for example, within the IT or Employees OU).
   - Create groups such as:
     - IT-Admins
     - Domain-Users
   - Assign users to groups based on their role:
     - Add jdoe and asmith to Domain-Users
     - Add itadmin to IT-Admins
   - Use group membership rather than individual user permissions to model enterprise access control best practices.

<img width="940" height="662" alt="image" src="https://github.com/user-attachments/assets/89b89117-d4bd-4242-96f8-a78df13bd130" />

> A small number of users is sufficient to demonstrate directory structure, group membership, and policy application without unnecessary complexity.

Default Active Directory containers are left intact, as they are system-managed and required for proper domain functionality. Custom Organizational Units are used instead to model enterprise directory structure and management practices.

---

## 5) Configure Client DNS and Join the Domain

### Configure DNS on CLIENT-01

1. Log into CLIENT-01
2. Open Network Adapter Settings
3. Set the Preferred DNS Server to the private IP address of DC-01

### Join CLIENT-01 to the Domain

1. Open System Properties
2. Click Change settings
3. Select Domain
4. Enter the domain name corp.local
5. Authenticate using a domain admin account
6. Reboot when prompted
<img width="1143" height="797" alt="image" src="https://github.com/user-attachments/assets/3087510b-2251-4eb5-9824-dcf837638c96" />

---

## 6) Validate Domain Functionality

After reboot:

1. Log into CLIENT-01 using a domain user account
2. Confirm:
   - Successful domain authentication
   - Group membership is applied
   - Client appears in Active Directory under Group Computers
3. Test name resolution and connectivity to the domain controller

<img width="938" height="658" alt="image" src="https://github.com/user-attachments/assets/71f96355-3b38-4225-98dc-fbd9e4f875b4" />
<img width="1415" height="639" alt="image" src="https://github.com/user-attachments/assets/fe093312-752b-4cff-825f-1e790378cdaf" />

---

## Final Notes

This lab demonstrates practical understanding of:

- Identity and access management fundamentals
- Windows Server administration
- Cloud-hosted infrastructure with on-prem architecture
- Enterprise directory services
