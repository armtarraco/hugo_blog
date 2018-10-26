+++
title =  "Ruby Install Fails With Rvm Due Permission Denied"
date = 2018-10-26T09:34:55+02:00
featured_image = ""
description = ""
tags = ['rvm']
categories = ['devops']
+++

If you get this error

    Installing Ruby with rvm: Warning: Failed to create the file ruby-....tar.bz2.part: Permission denied

Solution in: https://stackoverflow.com/q/26242712

Your RVM was installed with `sudo`, so was installed in `/usr/local/rvm` - it's often called system installation, this requires that your user will be added to rvm group:

    rvm group add rvm "$USER"

$USER it will be replaced by your shell with your user name

then log out and log in, ensure with:

    id

that your user is in `rvm group`,

finally just in case update permissions for RVM:

    rvm fix-permissions

Another possibility is installing RVM for your user (no sudo)

To uninstall RVM do

    rvm implode --force
