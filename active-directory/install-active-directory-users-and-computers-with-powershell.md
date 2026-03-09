# Install Active Directory Users and Computers with PowerShell

Active Directory Users and Computers (ADUC) can be installed on supported Windows devices by adding the RSAT Active Directory tools through PowerShell.

## When to Use This

Use this when you need to install Active Directory Users and Computers on a Windows workstation for domain administration tasks.
You can expect the full process to take about 10 minutes in total.

## Prerequisites

- Windows Pro, Enterprise, or another edition that supports RSAT
- PowerShell opened as Administrator
- Internet access or access to your organization's Windows update source

## Install Command

Run the following command in an elevated PowerShell window:

```powershell
Add-WindowsCapability -Online -Name Rsat.ActiveDirectory.DS-LDS.Tools~~~~0.0.1.0
```

### Why does the command include `~~~~0.0.1.0`?

That part is simply built into the full Windows capability name. Windows uses the complete feature identifier when installing or removing optional capabilities, so the tildes and version number need to stay in the command.

## Launch ADUC

After installation, open Active Directory Users and Computers by running:

```powershell
dsa.msc
```
