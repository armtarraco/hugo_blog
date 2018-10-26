+++
title =  "How to Get a Class Instance From a Variable in Rails"
date = 2018-10-26T09:53:33+02:00
featured_image = ""
description = ""
tags = []
categories = ['rails']
+++

```
a = 'User'
Object.const_get(a)...
=> User(...)
```
