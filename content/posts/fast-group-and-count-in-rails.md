+++
date = "2018-04-26T22:25:09+02:00"
title = "Fast group and count in Rails"
tags = [
  "query",
]
categories = [
  "rails",
]

+++
For instance, count new users by month.

<!--more-->

    User.order("TO_CHAR(created_at, 'YYYY MM')").group("TO_CHAR(created_at, 'YYYY MM')").count