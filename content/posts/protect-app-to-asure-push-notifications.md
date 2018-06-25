+++
title =  "Protect App to asure Push Notifications"
date = 2018-06-22T12:56:47+02:00
featured_image = ""
description = ""
tags = ['push notifications', 'Huawei']
categories = ['misc']
+++

Mobiles with EMUI (Huawei, ...) do not show pushed notifications if the app is killed as part of the aggressive battery saving methods. To avoid this behaviour you must protect your app against being killed when screen is off.

Step 1: Go to Settings --> Advanced settings --> Battery manager --> Protected apps, then find the app you want to see notifications from, and protect it. This is "whitelisting" the app so Huawei's overeager software doesn't shut it down for no reason.

Step 2: Go to Settings --> Applications --> Advanced --> Ignore battery optimizations, then find the app and ignore it. Don't be tricked by the misleading wording, "ignoring" the app actually means to let it run, because you're telling the battery optimization function, aka Doze, to "ignore" that app.


Step 3: Go to Settings -> Notification panel & status bar -> Notification centre, then find the app, then activate "allow notifications" and also "priority display". You have to activate the priority part too to make sure you get notifications.


For my P8lite, only steps 1 and 3 have been necessary.

Source: 

https://www.forbes.com/sites/bensin/2016/07/04/push-notifications-not-coming-through-to-your-huawei-phone-heres-how-to-fix-it/#4c0bc7881ccc

https://2nwiki.2n.cz/pages/viewpage.action?pageId=68223777

