+++
title =  "Rounding in JS"
date = 2018-10-26T09:51:44+02:00
featured_image = ""
description = ""
tags = []
categories = ['js']
+++

```
function round(number, precision) {
  var shift = function (number, precision, reverseShift) {
    if (reverseShift) {
      precision = -precision;
    }
    var numArray = ("" + number).split("e");
    return +(numArray[0] + "e" + (numArray[1] ? (+numArray[1] + precision) : precision));
  };
  return shift(Math.round(shift(number, precision, false)), precision, true);
}
```
