---
layout: post
title: '전역 상수는 나쁜 것일까?'
author: [Dan]
tags: ['Python']
image: img/welcome-to-ghost.jpg
date: '2021-05-06'
draft: false
excerpt: why?
---

> [https://stackoverflow.com/questions/1263954/is-global-constants-an-anti-pattern](https://stackoverflow.com/questions/1263954/is-global-constants-an-anti-pattern)

### not that bad

전역 상수는 나쁘지 않다.

`Java` 에서는 마치 `python` 의 스크립트에 선언된 전역변수처럼 설정하는 방법

```python
# some_script.py

VARIABLE_1 = 1
VARIABLE_2 = 'YES'
...
```

> 오직 상수에 접근하는 것만 허용한다.

```Java
public final class C {
	// raise error
    private C() { throw new AssertionError("C is uninstantiable"); }
    public static final int OMGHAX = 0x539;
}
```
