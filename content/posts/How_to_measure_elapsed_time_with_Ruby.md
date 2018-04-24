---
title: "How_to_measure_elapsed_time_with_Ruby"
date: 2018-04-23T10:19:11+02:00
draft: false
---

```
starting = Process.clock_gettime(Process::CLOCK_MONOTONIC)
# ... time consuming operation
ending = Process.clock_gettime(Process::CLOCK_MONOTONIC)
elapsed = ending - starting
# => 9.183449000120163 seconds
```
