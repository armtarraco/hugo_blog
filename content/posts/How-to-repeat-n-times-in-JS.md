+++
title =  "How to Repeat N Times in JS"
date = 2018-07-22T19:49:20+02:00
featured_image = ""
description = ""
tags = []
categories = ['js']
+++

<!-- more -->

    let times = (n, f) => { while (n-->0) f() }

Usage:

    var value = function () { return calculatedValue }
    times(quantity, value)

