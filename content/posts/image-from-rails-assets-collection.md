+++
date = "2018-05-11T09:20:25+02:00"
draft = false
title = "Image from Rails assets collection"
tags = [
  "assets",
]
categories = [
  "rails",
]

+++
<!--more-->

    image = ("#{Rails.root}/shared/public" + 
            ActionController::Base.helpers.asset_path("eelp_icon.png"))