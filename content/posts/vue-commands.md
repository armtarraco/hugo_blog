+++
title =  "Vue-cli 3 commands"
date = 2018-10-05T11:53:14+02:00
featured_image = ""
description = ""
tags = []
categories = ['vue']
+++

<!-- more -->

Install node and npm

    sudo curl -sL https://rpm.nodesource.com/setup_8.x | sudo bash -
    sudo yum install -y nodejs
    node -v
    => v8.11.2
    npm -v
    => 5.6.0

Install version 3 of vue-cli

    sudo npm install -g @vue/cli

    vue --version


Note: upgrading vue-cli requires upgrading the app as well manually

  vue create is a Vue CLI 3 only command and you are using Vue CLI 2.9.6.
  You may want to run the following to upgrade to Vue CLI 3:

  npm uninstall -g vue-cli
  npm install -g @vue/cli

Bootstrap the application with vue-cli

    vue create app-name
    cd app-name
    npm run serve

Add the router

    vue add router