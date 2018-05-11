+++
date = "2018-05-11T09:21:28+02:00"
draft = true
title = "Callback conditional to only some fields changes"

+++
<!--more-->

    after_save :update_stripe, on: :update, if: proc { |object| object.changes.include?('trial_period_days') }