+++
categories = ["rails"]
date = "2018-05-11T09:22:32+02:00"
tags = ["devise"]
title = "Include helper in Devise Custom Mailer"

+++
<!--more-->

    Devise::Mailer.class_eval do helper
    	:email # include "EmailHelper", adjust to suit your needs
    end