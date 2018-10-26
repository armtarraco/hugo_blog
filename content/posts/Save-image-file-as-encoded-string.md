+++
title = "Save Image File as Encoded String"
date = 2018-10-26T09:15:10+02:00
featured_image = ""
description = ""
tags = []
categories = ['rails']
+++

```
encoded_string = Base64.strict_encode64(File.open(Rails.root.join("public/images/cirujano.jpg").open, "rb").read)
```