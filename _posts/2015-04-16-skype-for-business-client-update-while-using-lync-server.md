---
id: 450
title: Skype for Business Client Update while using Lync Server
date: 2015-04-16T10:03:15+00:00
author: nickstugr
layout: post
guid: http://www.stugr.com/?p=450
redirect_from: /2015/04/16/skype-for-business-client-update-while-using-lync-server/
categories:
  - Technology
tags:
  - lync
  - skype
  - skype for business
---
Patch Tuesday has now pushed the Skype for Business client as an update:

  1. Skype for Business 2015 desktop client (lyncmso)  
    <http://support.microsoft.com/KB/2889923>
  2. Skype for Business 2015 desktop client (lynclochelp)  
    <http://support.microsoft.com/KB/2889853>

If your organisation is running Lync Server 2013 (or 2010) upon first launch of the Skype for Business client your users will be presented with this:

<div id="attachment_460" style="width: 361px" class="wp-caption aligncenter">
  <a href="/wp-content/uploads/2015/04/skypeForBusinessFirstRun-1.png"><img src="/wp-content/uploads/2015/04/skypeForBusinessFirstRun-1.png" alt="Restart Skype for Business You have the newer version of Lync called Skype for Business. However, your admin would like you to use Lync or it’s the only version your server supports. Please restart now to use Lync." width="351" height="237" class="size-full wp-image-460" srcset="/wp-content/uploads/2015/04/skypeForBusinessFirstRun-1.png 351w, /wp-content/uploads/2015/04/skypeForBusinessFirstRun-1-300x203.png 300w" sizes="(max-width: 351px) 100vw, 351px" /></a>
  
  <p class="wp-caption-text">
    Restart Skype for Business<br />You have the newer version of Lync called Skype for Business. However, your admin would like you to use Lync or it’s the only version your server supports. Please restart now to use Lync.
  </p>
</div>

Googling for that specific message was fruitless, however I did come across the following article: <http://blogs.technet.com/b/scottstu/archive/2015/04/13/controlling-the-client-experience-with-skype-for-business.aspx>

Which links to this powerpoint: [http://blogs.technet.com/cfs-filesystemfile.ashx/_\_key/telligent-evolution-components-attachments/01-6310-00-00-03-64-79-08/Controlling-the-Client-Experience-with-Skype-for-Business\_5F00_v1.2.pptx](http://blogs.technet.com/cfs-filesystemfile.ashx/__key/telligent-evolution-components-attachments/01-6310-00-00-03-64-79-08/Controlling-the-Client-Experience-with-Skype-for-Business_5F00_v1.2.pptx) 

Which details the client experience for various server versions:

<div id="attachment_461" style="width: 510px" class="wp-caption aligncenter">
  <a href="/wp-content/uploads/2015/04/skypeForBusinessMatrix-1.png"><img src="/wp-content/uploads/2015/04/skypeForBusinessMatrix-1.png" alt="Skype for Business Client Experience" width="500" height="172" class="size-full wp-image-461" srcset="/wp-content/uploads/2015/04/skypeForBusinessMatrix-1.png 500w, /wp-content/uploads/2015/04/skypeForBusinessMatrix-1-300x103.png 300w" sizes="(max-width: 500px) 100vw, 500px" /></a>
  
  <p class="wp-caption-text">
    Skype for Business Client Experience
  </p>
</div>

Here is the information for Lync Server 2013:

<div id="attachment_462" style="width: 610px" class="wp-caption aligncenter">
  <a href="/wp-content/uploads/2015/04/skypeForBusinessLyncServer2013-1.png"><img src="/wp-content/uploads/2015/04/skypeForBusinessLyncServer2013-1.png" alt="Skype for Business UI – Lync Server 2013" width="600" height="344" class="size-full wp-image-462" srcset="/wp-content/uploads/2015/04/skypeForBusinessLyncServer2013-1.png 600w, /wp-content/uploads/2015/04/skypeForBusinessLyncServer2013-1-300x172.png 300w" sizes="(max-width: 600px) 100vw, 600px" /></a>
  
  <p class="wp-caption-text">
    Skype for Business UI – Lync Server 2013
  </p>
</div>

As you can see 2 things are required before deploying the client update:

  1. Install at a minimum server build 5.0.8308.857 (December 2014) <https://support.microsoft.com/en-us/kb/2809243> 
  2. `Set-CsClientPolicy -EnableSkypeUI $true`