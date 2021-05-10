---
layout: post
title: 'Python regex for digit with position'
author: [Dan]
tags: ['python', 'builtIn', 'regex']
image: img/welcome-to-ghost.jpg
date: '2021-05-10'
draft: false
excerpt: get digit and its position using regex
---

> [https://stackoverflow.com/questions/250271/python-regex-how-to-get-positions-and-values-of-matches](https://stackoverflow.com/questions/250271/python-regex-how-to-get-positions-and-values-of-matches)

## single digit

```python
import re

# p = re.compile(r'\d')
# for m in p.finditer('a1b2c3d4'):
#    ...
# 
# both codes are same work.
# but below code is more shorter.

for m in re.finditer(r'\d', 'a1b2c3d4'):
    print(m.start(), m.group())

# 6 (6, 7)
# 8 (8, 10)
# 11 (11, 12)
```

## single and multiple digits

```python
for m in re.finditer(r'[\d]+', 'sr_pg_1_11a3222'):
    print(m.start(), m.group(), m.span())

# 6 1 (6, 7)
# 8 11 (8, 10)
# 11 3222 (11, 15)
```