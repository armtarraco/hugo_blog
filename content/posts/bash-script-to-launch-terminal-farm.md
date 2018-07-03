+++
title =  "Bash Script to Launch Terminal Farm"
date = 2018-06-25T09:36:03+02:00
featured_image = ""
description = ""
tags = []
categories = ['bash']
+++

This script opens several tabs in gnome-terminal, selecting different profiles to easyly distinguish them.

To customize the tab title, go to profile preferences, command tab and choose among replacing or adding to current title. I personnaly prefer replacing the title with my own one.

The sintax each as follows:

    gnome-terminal -t "<tab-title>" --tab-with-profile=<Profile-name> --working-directory=<working folder path> \

I use it to open connections to my remote servers, launch and connect to my development vagrant box.
Notice that when finishing the command that launches the tab, the tab gets closed but it appears a button to relaunch it.


    #!/bin/bash
    # create and personalize profiles with colors and customize behaviour in command tab
    gnome-terminal  --tab-with-profile=Solar --working-directory=/home/alfredo/eelp/eelp_environment/web \
        --tab-with-profile=StayOpen -t "vagrant" -e "/bin/bash -c 'cd /home/alfredo/eelp/eelp_environment/ && vagrant ssh '" \
                    --tab-with-profile=Oscuro -t "STG" -e "/bin/bash -c 'ssh -t stg'" \
        --tab-with-profile=Oscuro -t "PRE" -e "/bin/bash -c 'ssh -t pre'" \
          --tab-with-profile=Green -t "PRODUCTION" -e "/bin/bash -c 'ssh -t production'" \
          --window --profile=Green -t "RSYN" --zoom=0.8 -e "/bin/bash -c 'cd /home/alfredo/eelp/eelp_environment/ && vagrant up && vagrant rsync-auto '" \
