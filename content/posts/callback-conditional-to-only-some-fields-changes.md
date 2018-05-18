+++
date = "2018-05-11T09:21:28+02:00"
draft = false
title = "Callback conditional to only some fields changes"
tags = [
  "callbacks",
]
categories = [
  "rails",
]

+++
<!--more-->

    after_save :update_stripe, on: :update, if: proc { |object| object.changes.include?('trial_period_days') }