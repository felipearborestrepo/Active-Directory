# 🛠️ Active Directory Troubleshooting Lab (1-VM Azure Setup)

## 🔍 Overview
This project simulates a real-world Active Directory (AD) issue where a domain user is unable to log in due to a Group Policy Object (GPO) misconfiguration. The goal is to demonstrate your ability to troubleshoot, fix, and document AD-related login problems using a single Windows Server VM on Azure.

- **Environment**: Single Windows Server 2019 VM on Azure
- **Domain**: `mydomain.local`
- **Users**: `admin.support` (Domain Admin), `john.doe` (Standard User)
- **Goal**: Block login via Group Policy, identify the issue, and fix it

---

## 📄 Step-by-Step Documentation

### 1. ✨ Azure VM Creation
- **Action**: Create a Windows Server 2019 Datacenter VM in Azure.

![Screenshot 2025-06-22 220906](https://github.com/user-attachments/assets/f5624053-a96c-4e27-a737-27429a339461)

---

### 2. 🚀 Install Active Directory
- **Action**: Use Server Manager to add the **Active Directory Domain Services** role.

![Capture1 (3)](https://github.com/user-attachments/assets/873eb186-3198-49ec-a531-01579e99abc7)

---

### 3. ⚖️ Promote Server to Domain Controller
- **Action**: Create a new forest: `mydomain.local` and restart when prompted.

![2](https://github.com/user-attachments/assets/764db466-999c-43cb-a11d-9439569e4ec7)

---

### 4. 📂 Create Users and Organizational Unit (OU)
- **Action**:
  - Open **Active Directory Users and Computers**
  - Create an OU named `TestOU`
  - Create two users:
    - `john.doe` → move into `TestOU`
    - `admin.support` → add to Domain Admins group

![Capture3 (1)](https://github.com/user-attachments/assets/05cf9e93-2500-41d3-9b63-cf8d99e832c5)

---

### 5. ⛔ Create GPO to Block Login
- **Action**:
  - Open **Group Policy Management**
  - Create GPO: `BlockLoginPolicy`, link it to `TestOU`
  - Set:
    ```
    Deny log on locally → Domain Users
    ```
![Capture4 (2)](https://github.com/user-attachments/assets/a1eb9441-5f5d-4a24-b7a3-0e8881ae4560)

---

### 6. ⏱️ Apply the GPO
- **Action**: Run in PowerShell:
  ```powershell
  gpupdate /force
## 7. 🔐 Simulate Login Failure  
**Action**: Attempt to log in as john.doe

**Expected Result**: Login fails due to GPO restrictions

**Note**: Only one VM was used, so a second VM wasn't available to test RDP as john.doe. This would work if you added a second client VM and joined it to the domain.

![Screenshot 2025-06-22 224030](https://github.com/user-attachments/assets/b4756836-48e2-4d26-80dc-eacc48edf131)

---

## 🛠️ Troubleshooting Phase

## 8. 🕵️ Check Effective Policy for john.doe (RSOP)  
**Action**:
- Open MMC (Start > Run > mmc)  
- Add Resultant Set of Policy snap-in  
- Right-click > Generate RSOP for john.doe  
- Navigate to:

Computer Configuration → Windows Settings → Security Settings → Local Policies → User Rights Assignment

yaml
Copy
Edit

- Confirm that Deny log on locally is applied

**(No screenshot was taken for this step, but it's good to mention in the documentation.)**

---

## 9. 📝 Remove Login Block  
**Action**: Edit BlockLoginPolicy and remove Domain Users from Deny log on locally  
**Screenshot**: (Insert screenshot here)

![Capture6](https://github.com/user-attachments/assets/4fce866f-ecc7-45a5-bcb6-fbfb539542db)

---

## 10. 🔓 Authorize Remote Login  
**Action**:
- Right-click This PC → Properties → Remote Settings  
- Add john.doe to Remote Desktop Users  
**Screenshot**: (Insert screenshot here)

![Capture7](https://github.com/user-attachments/assets/0a68b4bb-0e32-4271-88d1-3447b1ba83e0)

---

## 11. 🎯 Configure GPO to Allow RDP  
**Action**: Edit BlockLoginPolicy or a new GPO  
**Set**:
**Screenshot**: (Insert screenshot here)

![Capture8](https://github.com/user-attachments/assets/f513ccef-f585-488c-bff1-8aae76555000)

---

## 12. 🧪 Simulate Successful Login (If 2nd VM Used)  
**Action**: On a second VM joined to the domain, log in as john.doe  

![Screenshot 2025-06-22 235531](https://github.com/user-attachments/assets/70563e64-96b4-4ec9-aeeb-1c44d7064205)

---

## 📖 Bonus: Audit with Event Viewer (Optional for extra credit)  
**Action**: On the server, open Event Viewer  
Navigate to: Windows Logs → Security  

- Filter by Event ID 4625 (failed logon)  
- Filter by Event ID 4624 (successful logon)  
- Filter by Event ID 4740 (account lockout)  

![Capture9](https://github.com/user-attachments/assets/29d9f0b6-be0f-4053-ad42-0cc775ce1d7c)

---

## ✅ Project Outcome  
This lab demonstrates your ability to:

- Deploy and promote a Windows Server to a domain controller  
- Manage Active Directory users, OUs, and group memberships  
- Create and link Group Policy Objects (GPOs)  
- Use RSOP, MMC, and Event Viewer to investigate policies  
- Resolve login failures caused by Group Policy  
- Configure Remote Desktop permissions securely  
