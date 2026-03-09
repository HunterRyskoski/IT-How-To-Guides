# Install Active Directory Users and Computers with PowerShell

Active Directory Users and Computers (ADUC) can be installed on supported Windows devices by adding the RSAT Active Directory tools through PowerShell.

## When to Use This

Use this when you need to install Active Directory Users and Computers on a Windows workstation for domain administration tasks.

## Prerequisites

- Windows Pro, Enterprise, or another edition that supports RSAT
- PowerShell opened as Administrator
- Internet access or access to your organization's Windows update source

## Install Command

Run the following command in an elevated PowerShell window:

```powershell
Add-WindowsCapability -Online -Name Rsat.ActiveDirectory.DS-LDS.Tools~~~~0.0.1.0
