---
layout: post
title:  Get original Windows install date and upgrade history
author: nickstugr
---

If you use `systeminfo | find "Install Date"` you'll find that it will only return the date of the last Windows 10 build that has been installed, instead of the original install date.
```
Original Install Date:     30/10/2019, 2:45:19 AM
```

Instead, you'll need to hit up the registry.

Using powershell you are able to get the date for each upgrade build along with the very first install date.

Expanded longhand:
```powershell
# get old versions
(Get-ChildItem -Path HKLM:\System\Setup\Source* | ForEach-Object {Get-ItemProperty -Path Registry::$_}) +
# append current version
(Get-ItemProperty 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion') |
# select the fields we care about
Select-Object ProductName, ReleaseId, CurrentBuild, 
# make date readable and convert from UTC
@{n='InstallDate'; e={([DateTime]'1/1/1970').AddSeconds($_.InstallDate).ToLocalTime()}} |
# sort
Sort-Object 'InstallDate'
```
Collapsed shorthand:
```powershell
(gci HKLM:\System\Setup\Source* | % {gp -Path Registry::$_}) + (gp 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion') | select ProductName, ReleaseId, CurrentBuild, @{n='InstallDate'; e={([DateTime]'1/1/1970').AddSeconds($_.InstallDate).ToLocalTime()}} | sort 'InstallDate'
```
Example output from an old computer that's been upgraded all the way since Windows 8.1:
```
ProductName     ReleaseId CurrentBuild InstallDate           
-----------     --------- ------------ -----------           
Windows 8.1 Pro           9600         16/09/2013 9:49:28 PM 
Windows 10 Pro            10240        1/08/2015 12:19:42 AM 
Windows 10 Pro  1511      10586        13/01/2016 3:47:41 AM 
Windows 10 Pro  1607      14393        9/10/2016 5:53:53 AM  
Windows 10 Pro  1703      15063        26/04/2017 10:50:15 PM
Windows 10 Pro  1709      16299        10/03/2018 1:32:55 PM 
Windows 10 Pro  1803      17134        17/05/2018 3:45:10 AM 
Windows 10 Pro  1903      18362        30/10/2019 3:45:19 AM 
```

NB. You may notice an hour discrepancy between the systeminfo and powershell outputs for the last install, likely due to daylight savings and how epoch time is stored by Windows and subsequently converted to current timezone. I've not needed this information to be absolutely precise so haven't installed a fresh computer to see which one is the correct value.