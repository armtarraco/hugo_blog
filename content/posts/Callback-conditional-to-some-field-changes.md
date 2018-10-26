+++
title =  "Callback Conditional to Some Field Changes"
date = 2018-10-26T09:57:53+02:00
featured_image = ""
description = ""
tags = []
categories = ['rails']
+++

```
after_save :update_stripe, on: :update, if: proc { |object| object.changes.include?('trial_period_days') }
```
