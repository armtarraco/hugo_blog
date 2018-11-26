+++
title =  "How to Build Custom Project With Sublime"
date = 2018-10-29T09:38:38+01:00
featured_image = ""
description = ""
tags = ['sublime']
categories = ['hugo', 'devops']
+++

# Build system to deploy a blog based on Hugo https://gohugo.io/

Create a new Build system for this particular project going to `Tools/Build system/New Build system` and edit like:

    {
      "shell_cmd": "cd $folder && hugo && rsync -avz -e 'ssh -pPORT' --delete-after SOURCE_FOLDER/ USER@REMOTE_HOST:DESTINATION_FOLDER/"
    }

Save it as `hugo-blog.sublime-build`.

Select this new build system for the project in `Tools/Build system` menu.

De-select `Automatic` and select `hugo-blog`

**NOTE:**

`$folder`: The full path to the first folder open in the side bar.

More options in http://www.sublimetext.com/docs/3/build_systems.html#variables


# Restart service on remote host after transfer files

    {
      "shell_cmd": "... && ssh -pPORT USER@REMOTE_HOST 'sudo service SERVICE restart'"
    }
