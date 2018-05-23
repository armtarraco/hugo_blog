+++
categories = []
date = "2018-04-25T12:05:08+02:00"
tags = ["ruby"]
title = "How to get a class instance from a variable in Ruby"

+++
Everything in Ruby is an Object
<!--more-->

![](/uploads/2018/04/25/live_objects_10.jpg "Any object")

    a = 'User'  
    Object.const_get(a)...  
    => User(...)