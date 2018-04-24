---
title: How_to_measure_elapsed_time_with_Ruby
date: 2018-04-23 10:19:11 +0200

---
When measuring elapsed time between events or execution duration, be careful with which system function you use. It can drive you to accuracy errors.

<!--more-->

```
starting = Process.clock_gettime(Process::CLOCK_MONOTONIC)
# ... time consuming operation
ending = Process.clock_gettime(Process::CLOCK_MONOTONIC)
elapsed = ending - starting
# => 9.183449000120163 seconds
```