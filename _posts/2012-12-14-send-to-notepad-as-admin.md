---
id: 296
title: Send to Notepad as Admin
date: 2012-12-14T10:48:03+00:00
author: nickstugr
layout: post
guid: http://www.stugr.com/?p=296
redirect_from: /2012/12/14/send-to-notepad-as-admin/
categories:
  - Technology
tags:
  - Admin
  - Microsoft
  - Notepad
  - SendTo
  - Tips
  - UAC
  - Windows
---
As an administrator, how often do you open a config file located in a protected directory, like _\Windows_ or _\Program Files_, edit it, then hit save and get this lovely message?

<div id="attachment_297" style="width: 404px" class="wp-caption aligncenter">
  <a href="/wp-content/uploads/2012/12/donthavepermissiontosave-1.png"><img class="size-full wp-image-297" title="No I would not like to save in My Documents instead" src="/wp-content/uploads/2012/12/donthavepermissiontosave-1.png" alt="" width="394" height="185" srcset="/wp-content/uploads/2012/12/donthavepermissiontosave-1.png 394w, /wp-content/uploads/2012/12/donthavepermissiontosave-1-300x141.png 300w" sizes="(max-width: 394px) 100vw, 394px" /></a>
  
  <p class="wp-caption-text">
    No I would not like to save in My Documents instead
  </p>
</div>

There is a pretty easy workaround for this, however it will require you to think about how you open the file before you actually open it.

Type &#8220;_shell:sendto_&#8221; into an explorer address bar.

<div id="attachment_300" style="width: 362px" class="wp-caption aligncenter">
  <a href="/wp-content/uploads/2012/12/shell-sendto-1.png"><img class="size-full wp-image-300" title="Type shell:sendto into the address bar" src="/wp-content/uploads/2012/12/shell-sendto-1.png" alt="Type shell:sendto into the address bar" width="352" height="185" srcset="/wp-content/uploads/2012/12/shell-sendto-1.png 352w, /wp-content/uploads/2012/12/shell-sendto-1-300x158.png 300w" sizes="(max-width: 352px) 100vw, 352px" /></a>
  
  <p class="wp-caption-text">
    Type shell:sendto into the address bar
  </p>
</div>

You will end up at _&#8220;%appdata%\Microsoft\Windows\SendTo_&#8221;

<!--more-->

<div id="attachment_301" style="width: 702px" class="wp-caption aligncenter">
  <a href="/wp-content/uploads/2012/12/sendto-1.png"><img class="size-full wp-image-301" title="%appdata%\Microsoft\Windows\SendTo" src="/wp-content/uploads/2012/12/sendto-1.png" alt="%appdata%MicrosoftWindowsSendTo" width="692" height="245" srcset="/wp-content/uploads/2012/12/sendto-1.png 692w, /wp-content/uploads/2012/12/sendto-1-300x106.png 300w" sizes="(max-width: 692px) 100vw, 692px" /></a>
  
  <p class="wp-caption-text">
    %appdata%\Microsoft\Windows\SendTo
  </p>
</div>

Create a new shortcut to notepad in this folder pointing at &#8220;%windir%\system32\notepad.exe&#8221; (or your editor of choice)

<div id="attachment_303" style="width: 638px" class="wp-caption aligncenter">
  <a href="/wp-content/uploads/2012/12/create-shortcut-1.png"><img class="size-full wp-image-303" title="Create shortcut pointing at %windir%\system32\notepad.exe" src="/wp-content/uploads/2012/12/create-shortcut-1.png" alt="Create shortcut pointing at %windir%\system32\notepad.exe" width="628" height="466" srcset="/wp-content/uploads/2012/12/create-shortcut-1.png 628w, /wp-content/uploads/2012/12/create-shortcut-1-300x223.png 300w" sizes="(max-width: 628px) 100vw, 628px" /></a>
  
  <p class="wp-caption-text">
    Create shortcut pointing at %windir%\system32\notepad.exe
  </p>
</div>

Call this shortcut &#8220;Notepad as Admin&#8221;

Once created, go into the shortcuts properties, go to the shortcut tab, click advanced&#8230; and tick &#8220;Run as administrator&#8221;.

<div id="attachment_307" style="width: 535px" class="wp-caption aligncenter">
  <a href="/wp-content/uploads/2012/12/shortcut-properties-1.png"><img class="size-full wp-image-307" title="Run as administrator" src="/wp-content/uploads/2012/12/shortcut-properties-1.png" alt="Run as administrator" width="525" height="386" srcset="/wp-content/uploads/2012/12/shortcut-properties-1.png 525w, /wp-content/uploads/2012/12/shortcut-properties-1-300x221.png 300w" sizes="(max-width: 525px) 100vw, 525px" /></a>
  
  <p class="wp-caption-text">
    Run as administrator
  </p>
</div>

Now when you want to edit a file that your non-elevated user cannot edit, right click the file, choose _send to_, choose _Notepad as Admin_ and UAC will kick in and elevate for you!

<div id="attachment_308" style="width: 632px" class="wp-caption aligncenter">
  <a href="/wp-content/uploads/2012/12/sendto-notepadasadmin-1.png"><img class="size-full wp-image-308" title="Send to Notepad as Admin" src="/wp-content/uploads/2012/12/sendto-notepadasadmin-1.png" alt="Send to Notepad as Admin" width="622" height="379" srcset="/wp-content/uploads/2012/12/sendto-notepadasadmin-1.png 622w, /wp-content/uploads/2012/12/sendto-notepadasadmin-1-300x183.png 300w" sizes="(max-width: 622px) 100vw, 622px" /></a>
  
  <p class="wp-caption-text">
    Send to Notepad as Admin
  </p>
</div>