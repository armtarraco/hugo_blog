+++
title =  "Duplicate Hash as an Independent One in Ruby"
date = 2018-10-26T09:56:33+02:00
featured_image = ""
description = ""
tags = []
categories = ['ruby']
+++


https://gist.github.com/Integralist/9041051

    def deep_copy(o)
      Marshal.load(Marshal.dump(o))
    end
