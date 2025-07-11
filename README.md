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

<img width="1000" alt="image" src="https://i.imgur.com/KFblpRX.png">

* **Verification:** Reran Tenable scan — STIG marked **compliant**

<img width="1000" alt="image" src="https://i.imgur.com/KFblpRX.png">

---

## ↺ 3. Undo Manual Fix (Test Reversion)

* Re-checked "Password never expires" box for test user.
* Reran scan to confirm STIG fails again.


<img width="1000" alt="image" src="https://i.imgur.com/KFblpRX.png">
<img width="1000" alt="image" src="https://i.imgur.com/KFblpRX.png">
---

## 💪 4. PowerShell Remediation

```powershell
# Get all local users except built-in disabled ones
$users = Get-LocalUser | Where-Object { $_.Enabled -eq $true -and $_.Name -ne "Guest" }

# Set password expiration required
foreach ($user in $users) {
    wmic UserAccount where "Name='$($user.Name)'" set PasswordExpires=TRUE
}
```

* **Script Name:** `remediate-WN10-00-000090.ps1`
* **Run:** As Administrator in PowerShell

<img width="1000" alt="image" src="https://i.imgur.com/EdVdtYa.png">

---

## 📊 Before & After

| State      | Screenshot        |
| ---------- | ----------------- |
| **Before** | `initialfail.png` |
| **After**  | `scriptpass.png`  |

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
