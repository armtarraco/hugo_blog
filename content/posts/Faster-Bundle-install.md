+++
title =  "Faster Bundle Install"
date = 2018-10-26T09:24:55+02:00
featured_image = ""
description = ""
tags = []
categories = ['rails']
+++

```
bundle config --global jobs 3
```

To avoid installing unused documentation, create a file `~/.gemrc` with

    gem: --no-ri --no-rdoc
