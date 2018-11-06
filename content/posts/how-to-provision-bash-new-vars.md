+++
title = "How to add new vars to bash environment when provisioning"
date = 2018-11-06T09:42:04+01:00
featured_image = ""
description = ""
tags = ['vagrant']
categories = ['bash']
+++

Source: https://stackoverflow.com/a/28852365

    source ~/.profile && [ -z "$VAR" ] && echo "export VAR=value" >> ~/.profile

Breakdown:

- `source ~/.bashrc`: load the current `.bashrc` file
- `[ -z "$VAR"]`: check whether or not VAR is set, if not:
- `echo "export VAR=value" >> ~/.bashrc`: add the export line to `.bashrc`

Putting it all together:

    #!/usr/bin/env bash
    source ~/.bashrc
    if [ -z "$VAR" ]; then # only checks if VAR is set, regardless of its value
        echo "export VAR=value" >> ~/.bashrc
    fi
    #other env variables and bashrc stuff here

If you want to set the environment variables to a specific value, even if they're set, you can replace `[ -z "$VAR" ]` with this:

    if [ -z "$VAR" ] || [ "$VAR" != "value" ]; then
        #same as before
    fi

Then simply add this to your Vagrantfile:

    config.vm.provision "shell", path: "your-script.sh"
