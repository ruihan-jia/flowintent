---
layout: post
title:  "Node.js Installation"
date:   2017-03-29 00:47:33 +0000
categories: tech
---

wget https://nodejs.org/dist/v6.10.1/node-v6.10.1-linux-x64.tar.gz --no-check-certificate

tar xvfz node-v6.10.1-linux-x64.tar.gz

mkdir -p /usr/local/nodejs

mv node-v6.10.1-linux-x64/* /usr/local/nodejs

export PATH=$PATH:/usr/local/nodejs/bin




curl --silent --location https://rpm.nodesource.com/setup_6.x | bash -

yum -y install nodejs




create main.js

node main.js

make sure port is free.


npm ls

cd to folder

npm install

npm install express

npm install mysql



