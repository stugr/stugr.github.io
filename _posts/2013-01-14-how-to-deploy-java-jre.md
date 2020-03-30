---
id: 324
title: How to deploy Java (JRE)
date: 2013-01-14T14:59:54+00:00
author: nickstugr
layout: post
guid: http://www.stugr.com/?p=324
permalink: /2013/01/14/how-to-deploy-java-jre/
categories:
  - Technology
tags:
  - Deploy
  - GPO
  - Group Policy
  - Java
  - JRE
  - MSI
  - SCCM
  - Security
---
  1. Download offline installers: _<http://www.java.com/en/download/manual.jsp>_
  2. Start the exe
  3. In explorer, navigate to: _C:\Users\%username%\AppData\LocalLow\Sun\Java\  
    [<img class="aligncenter size-full wp-image-325" title="JRE-locallow" src="/wp-content/uploads/2013/01/JRE-locallow-1.png" alt="" width="617" height="120" srcset="/wp-content/uploads/2013/01/JRE-locallow-1.png 617w, /wp-content/uploads/2013/01/JRE-locallow-1-300x58.png 300w" sizes="(max-width: 617px) 100vw, 617px" />](/wp-content/uploads/2013/01/JRE-locallow-1.png)_
  4. Grab a copy of the .cab and .msi
  5. Cancel the install
  6. [Download this .mst](/wp-content/uploads/2013/01/jre7-1.zip) (The .mst will agree to licence, disable updates etc)
  7. Deploy using your system of choice such as Group Policy or SCCM, making sure to apply the .mst  
    [<img class="aligncenter size-full wp-image-328" title="JRE-grouppolicy" src="/wp-content/uploads/2013/01/JRE-grouppolicy-1.png" alt="" width="336" height="60" srcset="/wp-content/uploads/2013/01/JRE-grouppolicy-1.png 336w, /wp-content/uploads/2013/01/JRE-grouppolicy-1-300x54.png 300w" sizes="(max-width: 336px) 100vw, 336px" />](/wp-content/uploads/2013/01/JRE-grouppolicy-1.png)