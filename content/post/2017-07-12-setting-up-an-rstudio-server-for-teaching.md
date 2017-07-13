---
title: Setting up an RStudio Server for teaching
author: Emil Tveden Bjerglund
date: '2017-07-12'
slug: setting-up-an-rstudio-server-for-teaching
categories: [R]
tags: []
draft: true
---

Installed ubuntu server 16.04 on an old laptop. After initial setup, login with the user created during install.

For good measure, install the updates. Well that didn't work, since Ubuntu 16.04 apparantly has problems with my network card (https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1586875). I did not find a way to resolve that. 

Well, tried again with Ubuntu Server 17.04, which also did not help - at this point it is probably my knowledge on Linux systems that is the limiting factor. I instead installed the normal Ubuntu Desktop 17.04, and then both Wired and Wireless connections worked just fine. 

I then followed this guide: https://www.digitalocean.com/community/tutorials/how-to-set-up-r-on-ubuntu-14-04

`sudo nano /etc/apt/sources.list` and added the line `deb https://cran.rstudio.com/bin/linux/ubuntu zesty/` to the bottom. Add key by `sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9` (https://cran.rstudio.com/bin/linux/ubuntu/README.html). Save and run `sudo apt-get update`.

`sudo apt-get install r-base`
```
sudo apt-get install gdebi-core
wget https://download2.rstudio.org/rstudio-server-1.0.143-amd64.deb
sudo gdebi rstudio-server-1.0.143-amd64.deb
```

After that, the Rstudio server was installed and running. I could go to <ip>:8787 on another computer on the local network and log in with my user.

As I wish to use this server for teaching, I need to make a bunch of packages, e.g. the tidyverse available for all users. So I start R on the server as a superuser by `sudo R` and then run `install.packages('tidyverse', lib = '/usr/local/lib/R/site-library/)`. This takes a while, and the first times I did this some packages failed (rvest, httr, xml2, curl, openssl) due to missing dependencies. These were installed by 
```
sudo apt-get install libxml2-dev
sudo apt-get install libcurl4-openssl-dev
```
After which the install of tidyverse progressed without errors

```
sudo useradd -m andschmidt
sudo passwd andsmidt # Supply 123abc
```
To delete a user `sudo userdel andschmidt`