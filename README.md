# STIG Remediation Lab: WN10-00-000090 - Require Password Expiration

## ✨ Overview

**STIG ID:** WN10-00-000090
**Title:** Accounts must be configured to require password expiration.
**Category:** Account Policies
**Severity:** Medium
**Tools Used:** Tenable, Azure Windows 10 VM, PowerShell

This project demonstrates how to remediate the STIG finding WN10-00-000090, which requires all local user accounts to be configured to require password expiration.

---

## 🔎 1. Initial Scan

* **Scanner:** Tenable (STIG Plugin Enabled)
* **VM:** Azure-hosted Windows 10 VM
* **Result:** Finding `WN10-00-000090` flagged as **non-compliant**



<img width="1000" alt="image" src="https://i.imgur.com/EdVdtYa.png">

## ✅ 2. Manual Remediation Steps

### Step-by-Step:

1. Open **Computer Management** (`compmgmt.msc`)
2. Go to **Local Users and Groups > Users**
3. For each local user (except built-in disabled accounts like Guest):

   * Right-click and choose **Properties**
   * Uncheck **"Password never expires"**

<img width="1000" alt="image" src="https://i.imgur.com/m2EkGGF.png">

* **Verification:** Reran Tenable scan — STIG marked **compliant**

<img width="1000" alt="image" src="https://i.imgur.com/oGKaUYG.png">

---

## ↺ 3. Undo Manual Fix (Test Reversion)


  
* Re-checked "Password never expires" box for test user. This will purposely
  fail the scan on policy
  
<img width="1000" alt="image" src="https://i.imgur.com/7oW9a7Q.png">

🔄 Why the User Is Prompted to Change Password
During this STIG remediation lab, you may be prompted to change your password after enforcement. This is expected behavior and confirms the policy was applied correctly.

Explanation:
The STIG WN10-00-000090 requires that user accounts be configured with password expiration. When this setting is applied — particularly in a test environment where the password may be outdated or never set to expire — Windows will prompt the user to update their password on next login.

This ensures:

The policy is actively being enforced

Passwords are not left unchanged indefinitely

The system is compliant with best practices for password hygiene

This password prompt can be considered visual confirmation that the policy has taken effect.


<img width="1000" alt="image" src="https://i.imgur.com/7v55J58.png">



---

## 💪 4. PowerShell Remediation
```🔧 PowerShell Remediation Script
To bring the system into compliance with STIG ID: WN10-00-000090 (Accounts must be configured to require password expiration), the following PowerShell script was used:

# Run this script in an elevated PowerShell session (Run as Administrator)

# Force all local user accounts (except Administrator) to require password expiration
wmic useraccount where "LocalAccount=True and Disabled=False and Name!='Administrator'" set PasswordExpires=True

# Set the maximum password age to 60 days
net accounts /maxpwage:60

# Force Group Policy to apply the new settings
gpupdate /force

📌 What it does:

✅ Ensures local user accounts are set to require password expiration

🕒 Configures maximum password age to 60 days as required

🔄 Applies changes immediately using gpupdate 
```

<img width="1000" alt="image" src="https://i.imgur.com/I8FoB9n.jpeg">

---

## 📊 Before & After

| State      | Screenshot                                                           |
| ---------- | ---------------------------------------------------------------------                                                    |
| **Before** | <img width="1000" alt="image" src="https://i.imgur.com/EdVdtYa.png">|
| **After**  | <img width="1000" alt="image" src="https://i.imgur.com/LQUhaiF.png">|

---

## 💡 Lessons Learned

* Accounts with non-expiring passwords pose long-term security risks.
* GUI and PowerShell can both achieve remediation; automation preferred for scale.
* Scanning pre- and post-remediation verifies results and closes the audit loop.

---

## 📂 Files Included

* `manual-steps.txt` – Notes for manual remediation
* `remediate-WN10-00-000090.ps1` – Script for automation
* `images/` – Screenshots before/after remediation
* `scan-before.nessus`, `scan-after.nessus`

---

## 📅 Completion Info

* **Date Completed:** 7/10/2025
* **Author:** Cesar Arias
* **Environment:** Azure Windows 10 VM
* **Audit Tool:** Tenable
