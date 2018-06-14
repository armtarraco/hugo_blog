+++
title =  "Object memory usage in Rails"
date = 2018-06-14T06:28:57+02:00
featured_image = ""
description = ""
tags = ['memory']
categories = ['rails']
+++

A simple way is

    require 'objspace'

    ObjectSpace.memsize_of(object)

Example:

    a = []
    p ObjectSpace.memsize_of(a)
    => 40
    a[1000] = "pepe"
    p ObjectSpace.memsize_of(a)
    => 8168

