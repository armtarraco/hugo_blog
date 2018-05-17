+++
date = "2018-05-17T11:04:31+02:00"
title = "Install Gems locally to project in Rails"

+++
<!--more-->

    bundle install --path vendor/bundle

The `--path` parameter tells the `bundle install` command to install gems in the `myapp/vendor/bundle` directory. If you leave off the parameter, gems will be installed globally. It's a good practice to keep gems installed locally - especially if you are working on more than one Ruby application on the development machine.