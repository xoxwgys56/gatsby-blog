---
layout: post
title: 'Should `import` statements always at the top?'
author: [Dan]
tags: ['python']
image: img/welcome-to-ghost.jpg
date: '2021-05-06'
draft: false
excerpt: why import statement always at the top of code?
---

> [https://stackoverflow.com/questions/128478/should-import-statements-always-be-at-the-top-of-a-module](https://stackoverflow.com/questions/128478/should-import-statements-always-be-at-the-top-of-a-module)

## Question

[PEP 8](http://www.python.org/dev/peps/pep-0008/) states:

> Imports are always put at the top of the file, just after any module comments and docstrings, and before module globals and constants.

However if the class/method/function that I am importing is only used in rare cases, surely it is more efficient to do the import when it is needed?

Isn't this:

```python
class SomeClass(object):

    def not_often_called(self)
        from datetime import datetime # here
        self.datetime = datetime.now()

```

more efficient than this?

```python
from datetime import datetime # here

class SomeClass(object):

    def not_often_called(self)
        self.datetime = datetime.now()

```

## Answer

Module importing is quite fast, but not instant. This means that:

- Putting the imports at the top of the module is fine, because it's a trivial cost that's only paid once.
- Putting the imports within a function will cause calls to that function to take longer.

So if you care about efficiency, put the imports at the top. Only move them into a function if your profiling shows that would help (you **did** profile to see where best to improve performance, right??)

---

The best reasons I've seen to perform lazy imports are:

- Optional library support. If your code has multiple paths that use different libraries, don't break if an optional library is not installed.
- In the `__init__.py` of a plugin, which might be imported but not actually used. Examples are Bazaar plugins, which use `bzrlib`'s lazy-loading framework.

> 최상단에 위치하면, 모듈의 전역으로 임포트되고, 임포트되는 비용을 1회만 지출할 수 있다.

> 최상단에서 임포트하지 않는 것을 `lazy import` 라고 부르는 모양.
