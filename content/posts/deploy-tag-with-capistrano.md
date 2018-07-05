+++
title =  "Deploy Tag or branch With Capistrano"
date = 2018-07-05T07:55:48+02:00
featured_image = ""
description = ""
tags = ['capistrano']
categories = ['devops']
+++

<!--more-->

Capistrano uses the parameter `branch` to choose which commit to deploy.

Therefore, this will do the task

    branch=<tag> cap <environment> deploy

You can put in your script the following

    set :branch do
      default_tag = `git tag --sort=creatordate`.split("\n").last

      ask(:tag, default_tag)
      tag
    end

If like me, you select branches to deploy with

    set :branch, ENV['BRANCH'] || :master

you can combine both strategies with

    set :branch do
      default_branch = ENV['BRANCH'] || :master
      last_tag = `git tag --sort=creatordate`.split("\n").last

      ask(:tag_to_deploy, default_branch)
      fetch(:tag_to_deploy, default_branch)
    end

Maybe you have to update your Git to support --sort option.

Also, do not exclude `.git` from your Vagrantfile `config.vm.synced_folder`.
