+++
date = "2018-04-25T12:05:08+02:00"
title = "How to get a class instance from a variable in Ruby"

+++
Everything in Ruby is an Object
<!--more-->

```
a = 'User'  
Object.const_get(a)...  
=> User(...)
```
