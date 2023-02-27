---
id: 419
title: Out of office not replying
date: 2014-05-23T13:19:49+00:00
author: nickstugr
layout: post
guid: http://www.stugr.com/?p=419
redirect_from: /2014/05/23/out-of-office-not-replying/
categories:
  - Technology
---
I had a report of a user (Bob McUser) turning on their OOF but when people emailed them no auto response was sent. After some basic troubleshooting such as &#8220;is the OOF actually turned on?&#8221; it was time to dig deeper. Bob had over 100 rules so this is where I focused my attention as Exchange has a rule quota (default: 64KB), and ultimately OOF is a rule.

You can see what a users rules quota is set to using:

    Get-Mailbox Bob.McUser | Select RulesQuota
    
    RulesQuota
    ----------
    64 KB (65,536 bytes)

<!--more-->

  
Unfortunately there is no way to determine how much of the rules quota a user is using, but we _can_ see how many rules they have. Keep in mind that the size of a rule is based on the name of the rule and it&#8217;s complexity &#8211; someone with 50 rules with long names that are super complex may fill their quota more quickly than someone with 200 rules that have short names and are simple &#8211; but at least this count is a good indication of a possible problem.

The following powershell will enumerate through all your users and count how many mailbox rules they have set-up (you could obviously filter it to only certain users, but I wanted to get an idea of how many rules most people had)

    Get-Mailbox -resultsize unlimited | Add-Member ScriptProperty RuleCount {@(Get-InboxRule -mailbox $this.identity -erroraction silentlycontinue -warningaction silentlycontinue).count} -passthru | sort rulecount | select name, rulecountName
    
    Name            RuleCount
    ----            ---------
    Jess Wright            10
    Alan Wood              16
    Bob McUser            113

Bob didn&#8217;t have the most rules, but was definitely in the top 1-2%.

Luckily when a user sets their OOF and it fails, the mailbox server logs an error under event id _3004_ with the message: _The Rules quota of mailbox bob.mcuser@domain.com has been reached and the automatic reply rules can&#8217;t be enabled or updated. Delete some existing rules or increase the user&#8217;s rule quota and try to set the automatic reply again. You can use the Set-Mailbox cmdlet to increase a user&#8217;s rules quota._

We can search all mailbox servers for this error, and return the users affected using:

    Get-MailboxServer | %{ Get-WinEvent -FilterHashtable @{logname='application'; id=3004} -computername $_.name -erroraction silentlycontinue | select -expand properties | sort -unique }
    
    Value
    -----
    bob.mcuser@domain.com

Now we have definite proof of the issue, there are 2 options. Get the user to remove some rules, or expand their rules quota using the below powershell (256KB is the maximum allowed):

    Set-Mailbox Bob.McUser -RulesQuota 256kb