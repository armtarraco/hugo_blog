+++
title = "Increasing the amount of inotify watchers"
date = 2018-05-19T01:14:12+02:00
featured_image = ""
description = ""
tags = []
categories = ['devops']
+++

https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers

If you are not interested in the technical details and only want to get Listen to work:

If you are running Debian, RedHat, or another similar Linux distribution, run the following in a terminal:

    echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p

If you are running ArchLinux, run the following command instead (see here for why):

    echo fs.inotify.max_user_watches=524288 | sudo tee /etc/sysctl.d/40-max-user-watches.conf && sudo sysctl --system

Then paste it in your terminal and press on enter to run it.

