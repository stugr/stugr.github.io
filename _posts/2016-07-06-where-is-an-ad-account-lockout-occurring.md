---
id: 466
title: Where is an AD Account Lockout Occurring?
date: 2016-07-06T14:54:52+00:00
author: nickstugr
layout: post
guid: http://www.stugr.com/?p=466
redirect_from: /2016/07/06/where-is-an-ad-account-lockout-occurring/
categories:
  - Technology
tags:
  - AD
  - Admin
  - Microsoft
  - Powershell
---
How often do you hear &#8220;my account keeps getting locked out&#8221; followed by &#8220;I&#8217;m not typing my password wrong&#8221;?

I&#8217;ve found the easiest and quickest way to determine where the account lockout occurred is to query the PDC Emulator with Powershell. You may require a domain admin to do this.

<pre class="brush: powershell; title: ; notranslate" title=""># Get PDC Emulator from the domain
$pdcEmulator = (Get-ADDomainController -Filter * | ? {$_.OperationMasterRoles -contains "PDCEmulator"}).hostname

# Run command against PDC Emulator
$lockouts = Invoke-Command -Credential (Get-Credential "DOMAIN\AdminAccount") -ComputerName $pdcEmulator -ScriptBlock
{
    # Filter on event id 4740 for the past 24 hours
    Get-WinEvent -FilterHashtable @{logname='security'; id=4740; StartTime=(Get-Date).AddHours(-24)} | select timecreated, @{N='who';E={$_.properties[0].value }}, @{N='where';E={$_.properties[1].value }}
}

# Search results for the user
$lockouts | ? {$_.who -eq "User.Locking" } | select timecreated, who, where | ft -auto</pre>

<pre class="brush: powershell; first-line: 13; title: ; notranslate" title="">TimeCreated             who               where            
-----------             ---               -----            
5/07/2016 1:34:12 PM    User.Locking      computerName
5/07/2016 12:34:12 PM   User.Locking      computerName
5/07/2016 10:46:47 AM   User.Locking      UAGserver
5/07/2016 10:01:56 AM   User.Locking      CASserver</pre>

Depending on your environment, if you are seeing lockouts occurring on a workstation, it will generally be typing a password wrong or saved credentials somewhere. If you are seeing them at a UAG or CAS server, investigate their activesync devices.

<pre class="brush: powershell; title: ; notranslate" title="">Get-ActiveSyncDevice -mailbox 'User.Locking' | ft guid, friendlyname, devicetype, whencreated -AutoSize</pre>

<pre class="brush: powershell; first-line: 2; title: ; notranslate" title="">Guid                     FriendlyName                 DeviceType    WhenCreated           
----                     ------------                 ----------    -----------           
28236d04-478a-433a-9af6  Outlook for iOS and Android  Outlook       15/06/2016 12:24:14 PM
4abe3b7c-cba3-45db-ade2  computerName                 WindowsMail   2/12/2015 4:19:10 PM</pre>

If you need to remove these ActiveSync devices:

<pre class="brush: powershell; title: ; notranslate" title="">$devices = Get-ActiveSyncDevice -mailbox 'User.Locking'

$devices | % { Remove-ActiveSyncDevice -Identity $_.identity }</pre>

If you get an error like `An unexpected error has occurred and a Watson dump is being generated: Object reference not set to an instance` then set this before running remove:

<pre class="brush: powershell; title: ; notranslate" title="">Set-ADServerSettings -ViewEntireForest $true</pre>