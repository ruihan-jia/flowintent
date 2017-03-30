---
layout: post
title:  "Jekyll Installation"
date:   2017-03-29 00:48:33 +0000
categories: tech
---

Self Notes. Links and commands on installing jekyll on Ubuntu and deploy to aws s3.

<br>
<pre>

http://michaelchelen.net/81fa/install-jekyll-2-ubuntu-14-04/
//installing on rhel failed, error installing jekyll. so trying ubuntu instead.
sudo apt-get install ruby ruby-dev make gcc nodejs

gem install jekyll
gem install bundler
jekyll -v
jekyll sitename
cd sitename
jekyll serve
(home/web/flowintent)
http://danielwhyte.com/app/design/2014/10/05/creating-a-jekyll-s3-server.html


setting up s3
set up s3 bucket


gem install s3_website
create iam user


s3_website cfg create
vi s3_website.yml
jekyll build
(might need java at this point)
apt-get install default-jre
s3_website push

to deploy:
sh deploy.sh

to set up github:
https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/#platform-linux

</pre>


