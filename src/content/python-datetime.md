---
layout: post
title: Python datetime
image: img/testimg-cover.jpg
author: [Dan]
date: 2021-05-02
tags:
  - python
  - builtIn
excerpt: python built in function datetime
---

> [https://stackoverflow.com/questions/30071886/how-to-get-current-time-in-python-and-break-up-into-year-month-day-hour-minu](https://stackoverflow.com/questions/30071886/how-to-get-current-time-in-python-and-break-up-into-year-month-day-hour-minu)

```python
import datetime


now = datetime.datetime.now()
print(now.year, now.month, now.day, now.hour, now.minute, now.second)
# 2015 5 6 8 53 40
```

### Related

- [https://docs.python.org/ko/3/library/datetime.html](https://docs.python.org/ko/3/library/datetime.html)
