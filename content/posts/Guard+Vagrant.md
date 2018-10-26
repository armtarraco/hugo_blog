+++
title =  "Guard & Vagrant"
date = 2018-10-26T09:19:39+02:00
featured_image = ""
description = ""
tags = []
categories = ['testing']
+++

Having problems with developing and testing using Vagrant?
Rsync-auto is slower than fs-notify. Therefore, guard is receiving the notification before rsync has finished its job. To avoid this issue start guard with a delay like this

    guard --wait-for-delay 4
