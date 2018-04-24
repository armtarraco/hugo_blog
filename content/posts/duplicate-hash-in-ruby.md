+++
date = "2018-04-24T10:24:03+02:00"
draft = true
title = "Duplicate hash in Ruby"

+++
To copy and create a new independent hash in Ruby, use the following snippet.
<!--more-->

source: [https://gist.github.com/Integralist/9041051](https://gist.github.com/Integralist/9041051 "https://gist.github.com/Integralist/9041051")

```
def deep_copy(o)  
  Marshal.load(Marshal.dump(o))  
end
```