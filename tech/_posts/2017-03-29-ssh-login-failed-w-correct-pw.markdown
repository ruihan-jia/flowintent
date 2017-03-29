---
layout: post
title:  "SSH failed with correct password"
date:   2017-03-29 00:49:33 +0000
categories: tech
---


login with multiple users including root with correct password failed.
access denied.

if connection still exists

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
add your user names.

/etc/init.d/sshd restart


