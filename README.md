# 🧠 Active Directory Lab 1  
  
**Windows Server 2025 · Azure Free Tier · Identity & Access Management**  
  
---  
  
## 📌 Overview  
  
This lab walks through building a fully functional **Active Directory (AD) environment** from scratch using Windows Server 2025 and Azure.    
  
It focuses on real-world identity and access management tasks such as:  
- Creating and managing users  
- Designing organizational structures  
- Applying security policies  
- Performing help desk operations  
  
---  
  
## 🎯 Objectives  
  
By completing this lab, you will learn how to:  
  
- Promote a Windows Server to a **Domain Controller**  
- Create and manage **Organizational Units (OUs)**  
- Configure **users, groups, and permissions**  
- Implement **Group Policy Objects (GPOs)**  
- Join machines to a domain  
- Perform common **IT support tasks** (password resets, unlocks, etc.)  
  
---  
  
## 🧰 Technologies Used  
  
- Windows Server 2025 (Evaluation)  
- Microsoft Azure (Free Tier)  
- Active Directory Domain Services (AD DS)  
- Group Policy Management Console (GPMC)  
- PowerShell  
  
---  
  
## ⏱️ Lab Details  
  
- **Time to Complete:** 3–5 hours    
- **Cost:** $0 (Azure free credits + evaluation licenses)    
- **Career Relevance:**    
  - IT Support / Help Desk    
  - System Administration    
  - Cloud Engineering    
  - Security Analysis    
  
---  
  
## 💼 Business Problem  
  
Organizations rely on Active Directory to answer:  
  
> **Who is allowed to do what?**  
  
AD controls:  
- User authentication  
- Access to systems and resources  
- Security policy enforcement  
  
This lab simulates a real enterprise environment where:  
- New users are onboarded via groups  
- Access is managed centrally  
- Policies are enforced automatically  
  
---  
  
## 🏗️ Lab Architecture  
## 🏛️ Architecture Diagrams

### Active Directory Structure
![AD Architecture](./diagrams/ad-architecture.svg)

### Lab Setup Flow
![Lab Setup Flow](./diagrams/lab-setup-flow.svg)
  
- 1 Windows Server VM (Domain Controller)  
- Domain: `lab.local`  
- Organizational Units:  
  - IT  
  - Finance  
  - HR  
  - Sales  
  - Computers  
  
---  
  
## 🚀 Step 1 — Setup (Azure)  
  
1. Create a free Azure account: https://azure.microsoft.com/free    
2. Deploy a **Windows Server 2025 VM**  
3. Recommended configuration:  
   - Region: West US  
   - Size: 2 vCPU, 4GB RAM  
   - Disk: Standard SSD  
   - Ports: RDP (3389)  
  
✅ Tip: Stop the VM when not in use to conserve credits.  
  
---  
  
## 🖥️ Step 2 — Install Active Directory  
  
### Using Server Manager  
- Add role: **Active Directory Domain Services**  
  
### PowerShell:  
```powershell  
Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools  
Install-WindowsFeature -Name GPMC  

🌐 Step 3 — Promote to Domain Controller

    Create a new forest:

        Domain: lab.local

PowerShell:

powershell
Copy
Install-ADDSForest -DomainName "lab.local"  

✅ Result:

    Server becomes Domain Controller

    DNS configured automatically

🗂️ Step 4 — Build AD Structure
Create Organizational Units

powershell
Copy
New-ADOrganizationalUnit -Name "IT" -Path "DC=lab,DC=local"  
New-ADOrganizationalUnit -Name "Finance" -Path "DC=lab,DC=local"  
New-ADOrganizationalUnit -Name "HR" -Path "DC=lab,DC=local"  
New-ADOrganizationalUnit -Name "Sales" -Path "DC=lab,DC=local"  
New-ADOrganizationalUnit -Name "Computers" -Path "DC=lab,DC=local"  

Create Security Groups

powershell
Copy
New-ADGroup -Name "IT_Admins" -GroupScope Global -GroupCategory Security -Path "OU=IT,DC=lab,DC=local"  
New-ADGroup -Name "Finance_Users" -GroupScope Global -GroupCategory Security -Path "OU=Finance,DC=lab,DC=local"  
New-ADGroup -Name "HR_Users" -GroupScope Global -GroupCategory Security -Path "OU=HR,DC=lab,DC=local"  
New-ADGroup -Name "Sales_Users" -GroupScope Global -GroupCategory Security -Path "OU=Sales,DC=lab,DC=local"  

Create Users

powershell
Copy
$password = ConvertTo-SecureString "Welcome@2026!" -AsPlainText -Force  
  
New-ADUser -Name "alice.chen" -SamAccountName "alice.chen" -Path "OU=IT,DC=lab,DC=local" -AccountPassword $password -Enabled $true  
New-ADUser -Name "bob.patel" -SamAccountName "bob.patel" -Path "OU=Finance,DC=lab,DC=local" -AccountPassword $password -Enabled $true  
New-ADUser -Name "carol.jones" -SamAccountName "carol.jones" -Path "OU=HR,DC=lab,DC=local" -AccountPassword $password -Enabled $true  
New-ADUser -Name "david.smith" -SamAccountName "david.smith" -Path "OU=Sales,DC=lab,DC=local" -AccountPassword $password -Enabled $true  

Assign Group Memberships

powershell
Copy
Add-ADGroupMember -Identity "IT_Admins" -Members "alice.chen"  
Add-ADGroupMember -Identity "Finance_Users" -Members "bob.patel"  
Add-ADGroupMember -Identity "HR_Users" -Members "carol.jones"  
Add-ADGroupMember -Identity "Sales_Users" -Members "david.smith"  

🔐 Step 5 — Configure Group Policy

Create a GPO: "IT Security Policy"
Key Policies:

    Minimum password length: 12

    Password complexity: Enabled

    Screen lock: 15 minutes

    USB access: Blocked

✅ Apply GPO to the IT OU
🛠️ Step 6 — Help Desk Tasks
Reset Password

powershell
Copy
Set-ADAccountPassword -Identity "bob.patel" -Reset -NewPassword (ConvertTo-SecureString "NewPass@2026!" -AsPlainText -Force)  
Set-ADUser -Identity "bob.patel" -ChangePasswordAtLogon $true  

Unlock Account

powershell
Copy
Unlock-ADAccount -Identity "carol.jones"  

Disable Account

powershell
Copy
Disable-ADAccount -Identity "david.smith"  

✅ Verification
Check	Command
Domain Controller	Get-ADDomainController
OUs	Get-ADOrganizationalUnit -Filter *
Users	Get-ADUser -Filter *
Groups	Get-ADGroupMember -Identity IT_Admins
🐞 Troubleshooting

Common issues and fixes:

    User creation fails

        Ensure $password variable is defined first

    Cannot copy/paste into VM

        Enable clipboard in RDP settings

    GPO not applying

    powershell
    Copy
    gpupdate /force  

    Login issues

        Ensure account is enabled

        Use LAB\username format

📚 Key Concepts Learned

    Identity & Access Management (IAM)

    Role-Based Access Control (RBAC)

    Domain Controllers & Forests

    Group Policy enforcement

    Enterprise user lifecycle management

🚀 Portfolio Value

This lab demonstrates hands-on experience with:

    Enterprise identity systems

    Real-world IT support workflows

    Security policy implementation

    Cloud + on-prem hybrid concepts

💡 This is directly applicable to roles like:

    IT Support Specialist

    System Administrator

    Cloud Engineer

    Security Analyst
