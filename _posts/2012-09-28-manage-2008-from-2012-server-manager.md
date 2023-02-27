---
id: 275
title: Manage 2008 from 2012 Server Manager
date: 2012-09-28T12:30:39+00:00
author: nickstugr
layout: post
guid: http://www.stugr.com/?p=275
redirect_from: /2012/09/28/manage-2008-from-2012-server-manager/
categories:
  - Technology
tags:
  - Group Policy
  - Windows
  - Windows Server 2008 R2
  - Windows Server 2012
  - WinRM
---
##### NB. Don&#8217;t install this on Exchange or SharePoint servers &#8211; see comment from Chris below

One of the biggest improvements to Windows Server 2012 is the (almost) all encompassing Server Manager. From any 2012 server you can now manage any other 2012 server through WinRM &#8211; but if you add a 2008 or 2008 R2 server to Server Manager, you will get a manageability warning of &#8220;_Online &#8211; Verify WinRM 3.0 service is installed, running, and required firewall ports are open_&#8220;.

<div id="attachment_291" style="width: 677px" class="wp-caption aligncenter">
  <a href="/wp-content/uploads/2012/09/WinRM3-1.jpg"><img class="size-full wp-image-291" style="border-width: 0px;" title="WinRM3" src="/wp-content/uploads/2012/09/WinRM3-1.jpg" alt="" width="667" height="60" srcset="/wp-content/uploads/2012/09/WinRM3-1.jpg 667w, /wp-content/uploads/2012/09/WinRM3-1-300x27.jpg 300w" sizes="(max-width: 667px) 100vw, 667px" /></a>
  
  <p class="wp-caption-text">
    Online - Verify WinRM 3.0 service is installed, running, and required firewall ports are open
  </p>
</div>

To get this working, there are 3 main steps to perform:

  1. Install Windows Management Framework 3.0
  2. Allow remote server management through WinRM (preferred method is via Group Policy)
  3. Create firewall rules

### 1. Install Windows Management Framework 3.0

Go and get the appropriate .msu from here: <http://www.microsoft.com/en-us/download/details.aspx?id=34595>

  * Windows Server 2008 R2 SP1 
      * WINDOWS6.1-KB2506143-x64.MSU
  * Windows Server 2008 SP2 
      * 64-bit versions: WINDOWS6.0-KB2506146-x64.MSU
      * 32-bit versions: WINDOWS6.0-KB2506146-x86.MSU

As you can see, there are some slight caveats. 2008 R2 requires SP1 while 2008 requires SP2 to be installed.

### 2. Allow remote server management through WinRM

There are 2 ways you can do this. I would recommend use Group Policy wherever possible, so go to:

_Computer Configuration > Policies > Administrative Templates > Windows Components > Windows Remote Management (WinRM) > WinRM Service > Allow remote server management through WinRM_

Set it to enabled and if you want it to listen on all addresses, put a * in IPv4 and IPv6 filter boxes.

Alternatively, you can run the command &#8220;_winrm quickconfig_&#8221; to enable remote access

### 3. Create firewall rules

Do this again through Group Policy, allowing port 5985.

&nbsp;

_Updated: 10th January 2013 &#8211; Added note about Exchange_