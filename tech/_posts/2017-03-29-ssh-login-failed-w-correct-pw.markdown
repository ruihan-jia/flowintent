---
layout: post
title:  "SSH failed with correct password"
date:   2017-03-29 00:49:33 +0000
categories: tech
---

A weird bug I ran into.

<div class="post-content2">

I had a putty session connect to a VM in private network. Between the span of a few hours when connecting to the VM again, it shows password invalid even tho it is the correct password. 

login with multiple users including root with correct password failed.

result shows access denied.

Fortunately I still had a connection to the VM.

access /var/log/secure or auth.log to troubleshoot.

example logs:
Mar 23 14:44:45 sms sshd[3956]: User root from 192.168.1.13 not allowed because not listed in AllowUsers
Mar 23 14:44:45 sms sshd[3957]: input_userauth_request: invalid user root
Mar 23 14:44:52 sms unix_chkpwd[3958]: password check failed for user (root)
Mar 23 14:44:52 sms sshd[3956]: pam_unix(sshd:auth): authentication failure; logname= uid=0 euid=0 tty=ssh ruser= rhost=192.168.1.13  user=root
Mar 23 14:44:55 sms sshd[3956]: Failed password for invalid user root from 192.168.1.13 port 51658 ssh2
Mar 23 14:45:01 sms unix_chkpwd[3959]: password check failed for user (root)

solution:
vi /etc/ssh/sshd_config
PermitRootLogin yes
AllowUsers ...
add your user names in AllowUsers.

/etc/init.d/sshd restart

I did a yum update in the span of the hours which might have caused this problem.

</div>
