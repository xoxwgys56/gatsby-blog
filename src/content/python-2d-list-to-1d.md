---
layout: post
title: 'Python 2d list to 1d'
author: [Dan]
tags: ['python', 'builtIn']
image: img/welcome-to-ghost.jpg
date: '2021-05-10'
draft: false
excerpt: 2차원 리스트를 1차원으로 줄이자.
---

2d dict list를 key값에 맞게 1d list로 변경하는 작업이 필요했다. 우선 1d list로 줄이고자 했다.

> [https://stackoverflow.com/questions/29244286/how-to-flatten-a-2d-list-to-1d-without-using-numpy](https://stackoverflow.com/questions/29244286/how-to-flatten-a-2d-list-to-1d-without-using-numpy)

# Answer

##### itertools.chain

Without numpy ( `[ndarray.flatten](http://docs.scipy.org/doc/numpy/reference/generated/numpy.ndarray.flatten.html)` ) one way would be using `[chain.from_iterable](https://docs.python.org/2/library/itertools.html#itertools.chain.from_iterable)` which is an alternate constructor for `itertools.chain` :

```python
>>> list(chain.from_iterable([[1,2,3],[1,2],[1,4,5,6,7]]))
[1, 2, 3, 1, 2, 1, 4, 5, 6, 7]
```

##### list comprehension

Or as another yet Pythonic approach you can use a **list comprehension** :

```python
[j for sub in [[1,2,3],[1,2],[1,4,5,6,7]] for j in sub]
```

> list comprehension이면 충분하다.

Another functional approach very suitable for short lists could also be `reduce` in Python2 and `functools.reduce` in Python3 (don't use it for long lists):

```python
In [4]: from functools import reduce # Python3

In [5]: reduce(lambda x,y :x+y ,[[1,2,3],[1,2],[1,4,5,6,7]])
Out[5]: [1, 2, 3, 1, 2, 1, 4, 5, 6, 7]
```

To make it slightly faster you could can use `operator.add`, which is built-in, instead of `lambda`:

```python
In [6]: from operator import add

In [7]: reduce(add ,[[1,2,3],[1,2],[1,4,5,6,7]])
Out[7]: [1, 2, 3, 1, 2, 1, 4, 5, 6, 7]

In [8]: %timeit reduce(lambda x,y :x+y ,[[1,2,3],[1,2],[1,4,5,6,7]])
789 ns ± 7.3 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

In [9]: %timeit reduce(add ,[[1,2,3],[1,2],[1,4,5,6,7]])
635 ns ± 4.38 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

```

benchmark:

```python
:~$ python -m timeit "from itertools import chain;chain.from_iterable([[1,2,3],[1,2],[1,4,5,6,7]])"
1000000 loops, best of 3: 1.58 usec per loop
:~$ python -m timeit "reduce(lambda x,y :x+y ,[[1,2,3],[1,2],[1,4,5,6,7]])"
1000000 loops, best of 3: 0.791 usec per loop
:~$ python -m timeit "[j for i in [[1,2,3],[1,2],[1,4,5,6,7]] for j in i]"
1000000 loops, best of 3: 0.784 usec per loop
```

A benchmark on @Will's answer that used `sum` (its fast for short list but not for long list) :

```python
:~$ python -m timeit "sum([[1,2,3],[4,5,6],[7,8,9]], [])"
1000000 loops, best of 3: 0.575 usec per loop
:~$ python -m timeit "sum([range(100),range(100)], [])"
100000 loops, best of 3: 2.27 usec per loop
:~$ python -m timeit "reduce(lambda x,y :x+y ,[range(100),range(100)])"
100000 loops, best of 3: 2.1 usec per loop
```
