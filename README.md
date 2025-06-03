# Windows Patch Automation Script

**Author:** Bharath Devulapalli  
**Date:** 2025-06-03  
**Purpose:** Automate Windows patch management using PowerShell with logging, reboot handling, and optional email reporting.

---

## 📜 Overview

This PowerShell script uses the `PSWindowsUpdate` module to:
- Install all available Windows updates
- Reboot automatically if required
- Log all actions for audit purposes
- Optionally send an email report (configurable section included)

---

## ⚙️ Script

```powershell
# -------------------------------------------
# PowerShell Script: PatchAutomation.ps1
# Author: Bharath Devulapalli
# Purpose: Automate Windows Updates with Logging and Reboot
# -------------------------------------------

# Step 1: Import the PSWindowsUpdate module (installs if not found)
if (-not (Get-Module -ListAvailable -Name PSWindowsUpdate)) {
    Write-Output "Installing PSWindowsUpdate module..."
    Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
    Install-Module -Name PSWindowsUpdate -Force -Confirm:$false
}
Import-Module PSWindowsUpdate

# Step 2: Create log directory
$logPath = "C:\PatchLogs"
if (-not (Test-Path $logPath)) {
    New-Item -ItemType Directory -Path $logPath
}
$logFile = "$logPath\PatchLog_$(Get-Date -Format 'yyyyMMdd_HHmmss').txt"

# Step 3: Check for available updates and install
Write-Output "Checking for updates..." | Tee-Object -FilePath $logFile -Append
Get-WindowsUpdate -AcceptAll -Install -AutoReboot -Verbose | Tee-Object -FilePath $logFile -Append

# Step 4 (Optional): Send summary email (configure SMTP if needed)
<# 
$smtpServer = "smtp.yourdomain.com"
$from = "patch-report@yourdomain.com"
$to = "admin@yourdomain.com"
$subject = "Patch Report for $env:COMPUTERNAME - $(Get-Date)"
$body = Get-Content $logFile | Out-String
Send-MailMessage -From $from -To $to -Subject $subject -Body $body -SmtpServer $smtpServer
#>

# End
Write-Output "Patch automation completed. Log saved to $logFile"
```

---

## ✅ Requirements

- Run PowerShell as Administrator
- Internet connection
- PowerShell 5.1 or later

---

## 📄 License

MIT License

Copyright (c) 2025 Bharath Devulapalli

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:
...
