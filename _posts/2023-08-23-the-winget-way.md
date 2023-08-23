---
layout: post
title:  "Installing Applications: The WINGET Way"
categories: [scripting,windows]
tags: [windows,powershell,script,winget]
---

# The Problem

I don't know about you, but I am always installing/reinstalling an OS somewhere. Sometimes I want a change and I'll move from Windows to Linux (or vice versa). Sometimes I just want a fresh start. No matter the reason, making sure I have all of my baseline applications installed can be a real pain.

In the past I managed a checklist that I would manually go through, but that was just so much work (and I am *very* lazy).

On Linux I started scripting everything, however Windows was still pretty tedious. Until [Winget](https://learn.microsoft.com/en-us/windows/package-manager/winget/) came along. Now Windows has a package manager like apt, dnf, yum, etc. 

# A Solution

There are plenty of solutions here. [Ninite](https://ninite.com), [Chocolatey](https://chocolatey.org), and the like. Or automation solutions like [Ansible](https://www.ansible.com), [Puppet](https://www.puppet.com), etc.

For now, a simple PowerShell script does what I need. Here is what that looks like.

# The Script

```powershell
# Install-Core-Applications.ps1
#
# Install basic applications after a fresh Windows deployment.

$AppList = @(
    "Microsoft.Powershell",
    "Microsoft.PowerToys",
    "Bitwarden.Bitwarden",
    "Git.Git",
    "9P7KNL5RWT25", # SysInternals Suite
    "9PMMSR1CGPWG", # HEIF Image Extension
    "JanDeDobbeleer.OhMyPosh",
    "Microsoft.VisualStudioCode",
    "VivaldiTechnologies.Vivaldi",
    "Discord.Discord",
    "SlackTechnologies.Slack",
    "Microsoft.PCManager",
    "Valve.Steam",
    "VideoLAN.VLC",
    "FlorianHoech.DisplayCAL",
    "Oracle.JavaRuntimeEnvironment",
    "XPDLPKWG9SW2WD" # Adobe Creative Cloud
    )

$TotalApps = $AppList.Count
$CurrentApp = 0

foreach ($App In $AppList) {
    $CurrentApp++
    Write-Progress -Id 0 -Activity "Installing Applications..." -PercentComplete ($CurrentApp/$TotalApps*100) -Status "Installing $App"
 
    winget install "$App"
}

# Enable WSL and install Ubuntu
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
wsl --install -d Ubuntu-22.04
```

# Conclusion

It's quick and dirty, but it gets the job done. For now.