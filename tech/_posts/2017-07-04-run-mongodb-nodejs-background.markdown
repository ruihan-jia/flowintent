---
layout: post
title:  "Run mongodb and nodejs in background"
date:   2017-07-04 17:13:33 +0000
categories: tech
---

self notes.
when running server on a remote VM, i needed to have nodejs and mongodb to be running even when im not connected to the VM.

<br>

#### run mongodb
mongod --fork --logpath /var/log/mongod.log
in my case: (not enough space)
mongod --smallfiles --fork --logpath /var/log/mongod.log
<br>
log can be checked at var/log/

<br>

#### run nodejs
https://seanmcgary.com/posts/deploying-nodejs-applications-with-systemd

create the following file
/etc/systemd/system/nodejs.service

{% highlight ruby %}
[Unit]
Description=Nodejs

[Service]
ExecStart=/usr/local/bin/nodemon
WorkingDirectory=/home/ubuntu/prj416/course-api
Type=simple

[Install]
WantedBy=multi-user.target
{% endhighlight %}


systemctl start nodejs.service
<br>
to check log:
journalctl -u nodejs -e



