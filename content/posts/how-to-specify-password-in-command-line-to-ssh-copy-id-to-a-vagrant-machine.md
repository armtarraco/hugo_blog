+++
title =  "How to Specify Password in Command Line to Ssh Copy Id to a Vagrant Machine"
date = 2018-10-26T09:05:42+02:00
featured_image = ""
description = ""
tags = ['ssh', 'vagrant']
categories = ['devops']
+++

```
sshpass -p PW ssh-copy-id -i /home/vagrant/.ssh/id_rsa.pub  USER@SERVER -pPORT
```