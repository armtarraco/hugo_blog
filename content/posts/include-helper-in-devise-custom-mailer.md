+++
date = "2018-05-11T09:22:32+02:00"
draft = false
title = "Include helper in Devise Custom Mailer"

+++
<!--more-->

    Devise::Mailer.class_eval do  helper :email # include "EmailHelper", adjust to suit your needsend