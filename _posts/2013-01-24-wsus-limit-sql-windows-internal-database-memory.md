---
id: 335
title: 'WSUS - Limit SQL (Windows Internal Database) memory'
date: 2013-01-24T16:42:11+00:00
author: nickstugr
layout: post
guid: http://www.stugr.com/?p=335
permalink: /2013/01/24/wsus-limit-sql-windows-internal-database-memory/
categories:
  - Technology
tags:
  - Microsoft
  - SQL
  - Windows Server 2012
  - WSUS
---
[](/wp-content/uploads/2013/01/sql-memory-limit-1.png)You may discover your WSUS servers SQL instance using most of the available memory &#8211; to limit its maximum memory usage, you can either do it via command line or via gui.

#### Configure via GUI

  1. Open SQL Management Studio
  2. Connect to `\\.\pipe\Microsoft##WID\tsql\query` for WSUS 4 (Server 2012) or `\\.\pipe\mssql$microsoft##ssee\sql\query` for WSUS 3 using Windows Authentication
  3. Right click the server in Object Explorer and choose Properties  
    [<img class="aligncenter size-full wp-image-338" title="sql-properties" src="/wp-content/uploads/2013/01/sql-properties-1.png" alt="" width="262" height="542" srcset="/wp-content/uploads/2013/01/sql-properties-1.png 262w, /wp-content/uploads/2013/01/sql-properties-1-145x300.png 145w" sizes="(max-width: 262px) 100vw, 262px" />](/wp-content/uploads/2013/01/sql-properties-1.png)
    <!--more-->
  4. Select Memory from the left hand side, then specify Maximum server memory (in MB) to whatever you want.  
    [<img class="aligncenter size-full wp-image-339" title="sql-memory-limit" src="/wp-content/uploads/2013/01/sql-memory-limit-1.png" alt="" width="704" height="272" srcset="/wp-content/uploads/2013/01/sql-memory-limit-1.png 704w, /wp-content/uploads/2013/01/sql-memory-limit-1-300x116.png 300w" sizes="(max-width: 704px) 100vw, 704px" />](/wp-content/uploads/2013/01/sql-memory-limit-1.png)
  5. Hit OK and then restart the sql service

#### Configure via command line

  1. Open a cmd window
  2. Enter the following command depending on version: 
      * For WSUS 4 (Server 2012: 
        <pre>osql -E -S \\.\pipe\Microsoft##WID\tsql\query</pre>
    
      * For WSUS 3: 
        <pre>osql -E -S \\.\pipe\mssql$microsoft##ssee\sql\query</pre>
  3. Enter the following commands: 
      <pre>exec sp_configure 'show advanced option', '1';
reconfigure;</pre>

  4. To view currently set max server memory: 
      <pre>exec sp_configure;
go</pre>
  5. To reconfigure: 
      <pre>exec sp_configure 'max server memory', 2048;
reconfigure with override;
go</pre>
  6. <pre>quit</pre>
  7. Restart the SQL service