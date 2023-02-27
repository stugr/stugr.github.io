---
id: 453
title: Create New Device in SCCM Using Powershell
date: 2015-08-17T10:12:40+00:00
author: nickstugr
layout: post
guid: http://www.stugr.com/?p=453
redirect_from: /2015/08/17/create-new-device-in-sccm-using-powershell/
categories:
  - Technology
tags:
  - ConfigMgr
  - Microsoft
  - SCCM
  - WMI
---
Creating a new device in SCCM using powershell with 2012 R1 requires using WMI as there is no cmdlet to do so, but thankfully this is easier now using the CIM cmdlets.

Of course if you have 2012 R2 then use [Import-CMComputerInformation](https://technet.microsoft.com/en-us/library/jj821991(v=sc.20).aspx) (thanks [@david_obrien](http://www.twitter.com/david_obrien))

<pre>$SCCMSiteServer = 'SiteServer.FQDN.com'
$SCCMNamespace = 'Root\SMS\Site_AAA'

$MACAddress = '00:11:22:33:44:55'
$DeviceName = 'PCNAME001'

Invoke-CimMethod -ClassName "SMS_Site" -Namespace $SCCMNamespace -ComputerName $SCCMSiteServer -MethodName 'ImportMachineEntry' -Arguments @{ MACAddress = $MACAddress; NetbiosName = $DeviceName }</pre>

As a bonus, here are some functions to create your devices along with basic validation of mac address

_NB. The “Stugr” in the function names is simply to avoid any clashes with official cmdlets_

<pre>function Format-StugrMACAddress {
    Param(
       [Parameter(Mandatory=$true)][string]$MACAddress
    )

    # strip possible variations of dividers
    $MACAddress = $MACAddress.Replace(' ','').Replace(':','').Replace('-','')

    # mac address must be 12 characters (with no dividers) and hexadecimal
    if($MACAddress.Length -eq 12 -and $MACAddress -match "^[A-Fa-f0-9]+$") {
        return ($MACAddress -replace "(..(?!$))",'$1:') # insert : every 2 characters except if they are the last 2 characters
    } else {
        Throw "$MACAddress is not a valid MAC address"
    }
}

function New-StugrSCCMComputer {
    Param(
        [Parameter(Mandatory=$true)][ValidateLength(7,15)][string]$DeviceName,
        [Parameter(Mandatory=$true)][ValidateScript({Format-StugrMACAddress $_})][string]$MACAddress
    )

    $SCCMSiteServer = 'SiteServer.FQDN.com'
    $SCCMNamespace = 'Root\SMS\Site_AAA'

    $MACAddress = (Format-StugrMACAddress $MACAddress)

    # device with that name already exists
    if (Get-CMDevice -Name $DeviceName) {
        Throw "$DeviceName already exists"
    }

    # create device
    Invoke-CimMethod -ClassName "SMS_Site" -Namespace $SCCMNamespace -ComputerName $SCCMSiteServer -MethodName 'ImportMachineEntry' -Arguments @{ MACAddress = $MACAddress; NetbiosName = $DeviceName }
}
</pre>

So now you can just create a new computer with:

<pre>New-StugrSCCMComputer -DeviceName 'PCNAME001' -MACAddress '00:11:22:33:44:55'</pre>

It will also handle various other mac address formats:

<pre>New-StugrSCCMComputer -DeviceName 'PCNAME001' -MACAddress '00 11 22 33 44 55'
New-StugrSCCMComputer -DeviceName 'PCNAME001' -MACAddress '00-11-22-33-44-55'
New-StugrSCCMComputer -DeviceName 'PCNAME001' -MACAddress '001122334455'</pre>

At the moment this doesn’t check to make sure there isn’t another device with the same mac address first (so it may not actually create a new device), but this extra validation is a job for another day.