+++
categories = ["rails"]
date = "2018-05-11T09:17:46+02:00"
tags = ["devise"]
title = "Devise mailer layout"

+++
<!--more-->

En application.rb
  
    config.to_prepare do  
      Devise::Mailer.layout "mailer" # mailer.haml or mailer.erb  
    end