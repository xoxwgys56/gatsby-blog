---
layout: post
title: 'Python Define Constants'
author: [Dan]
tags: ['python', 'designPattern']
image: img/python-default.jpeg
date: '2021-05-06'
draft: false
excerpt: 파이썬에서 어떻게하면 상수를 선언할 수 있는지 알아본다.
---

> [https://stackoverflow.com/questions/2682745/how-do-i-create-a-constant-in-python/2682752](https://stackoverflow.com/questions/2682745/how-do-i-create-a-constant-in-python/2682752)
>
> > 원본 글 번역

# Question

`Java`에서는 아래의 방법으로 상수를 선언할 수 있는데, `Python`에서는 어떤 방법으로 상수를 선언할 수 있니?

```java
public static final String CONST_NAME = "Name";
```

`Python`에서 위의 `Java`의 상수 선언과 동일한 것은 무엇이니?

# Answer

없어. 넌 `Python`에서 상수로서 변수를 선언할 수 없어. 그냥 바꾸지 않아야해.

만약 네가 클래스 내에서, 동일하게 선언하고자 한다면:

```python
class Foo(object):
    CONST_NAME = "Name"
```

아니라면, 그냥

```python
CONST_NAME = "Name"
```

But you might want to have a look at the code snippet [Constants in Python](http://web.archive.org/web/20100523132518/http://code.activestate.com/recipes/65207-constants-in-python/?in=user-97991) by Alex Martelli.

---

As of Python 3.8, there's a `[typing.Final](https://docs.python.org/3/library/typing.html#typing.Final)` variable annotation that will tell static type checkers (like mypy) that your variable shouldn't be reassigned. This is the closest equivalent to Java's `final`. However, it **does not actually prevent reassignment**:

```python
from typing import Final

a: Final = 1

# Executes fine, but mypy will report an error if you run mypy on this:
a = 2
```

### note

- 번역은 위의 url 읽고 마저 진행하겠음.
