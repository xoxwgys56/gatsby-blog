---
layout: post
title: 'Python mapping'
author: [Dan]
tags: ['python', 'builtIn']
image: img/welcome-to-ghost.jpg
date: '2021-05-10'
draft: true
excerpt: 파이썬 내부 클래스에 대해
---

> [https://stackoverflow.com/questions/56344997/what-is-a-python-mapping](https://stackoverflow.com/questions/56344997/what-is-a-python-mapping)

The term "mapping" is described [here](https://docs.python.org/3/glossary.html#term-mapping) as:

> A container object that supports arbitrary key lookups and implements the methods specified in the Mapping or MutableMapping abstract base classes. Examples include dict, collections.defaultdict, collections.OrderedDict and collections.Counter.

The requirements to subclass `collections.abc.Mapping` are described in its docstring:

> """A Mapping is a generic container for associating key/value
pairs.

This class provides concrete generic implementations of all
methods except for __getitem__, __iter__, and __len__.

"""

So you can define a new mapping type by subclassing `collections.abc.Mapping`, and implementing three methods: `__len__`, `__getitem__`, and `__iter__`.

```python
>>> from collections.abc import Mapping
>>> def func(**kwargs):
...   print(kwargs)
...
>>> class MyMapping(Mapping):
...   def __len__(self):
...     return 1
...   def __getitem__(self, k):
...     return 'bananas'
...   def __iter__(self):
...     return iter(['custard'])
...
>>> func(**MyMapping())
{'custard': 'bananas'}
```