+++
title =  "List Git tags by date"
date = 2018-07-05T07:55:01+02:00
featured_image = ""
description = ""
tags = []
categories = ['git', 'devops']
+++

<!--more-->

List tags by creation date. Notice the minus sign to list first the most recent one.

    git tag --sort=-creatordate


This one-liner displays dates & tags ordered by date

    git tag --sort=-creatordate --format='%(creatordate:short)%09%(refname:strip=2)'

    =>
    2018-07-02  1.0.10
    2018-06-28  1.0.9
    2018-06-28  1.0.8
    2018-06-25  1.0.7
    2018-04-23  1.0.6
    2018-04-17  1.0.5
    2018-03-06  1.0.4
    2018-03-05  1.0.3
    2018-02-23  1.0.2
    2018-02-23  0.9.9

Maybe you hace to update your git to support --sort option.
