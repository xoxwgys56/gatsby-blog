---
layout: post
title: 'Python append item without duplicates'
author: [Dan]
tags: ['python', 'builtIn']
image: img/python-default.jpeg
date: '2021-05-10'
draft: false
excerpt: 중복없이 리스트에 아이템을 추가하는 방법.
---

## 시작 계기

워커 내부에서 `parsel`, `bs4`, `selenium`을 함께 사용했는데
우선 순위에 따라 파싱되도록 구현하려고 했다.

## 결과

여러가지를 시도해보았지만, `list comprehension`이 제일 좋았다.

##### list and set ?

```python
current_elem_type = ['selenium']
temp_set = set()

elem_type_list = [x
                 for x in ['parsel', 'bs4', 'selenium']
                 if not (x in current_elem_type or temp_set.add(x))
                 ]

print(elem_type_list)
# ['parsel', 'bs4']
```

`bs4`가 제외된 결과가 출력된다.

> 나중에 생각해보니..
> `or temp_set.add(x)` 지우고, `current_elem_type.extend(elem_type_list)`하면 된다.

##### set union

```python
current = 'selenium'

elem_type_list = [current]
elem_list = ['parsel', 'bs4', 'selenium']

result = list(set(elem_type_list) | set(elem_list))
print(result)
# ['bs4', 'parsel', 'selenium']
```

리스트에 작성된 순서가 포함되지 않고, 알파벳 순서로 출력되었다.

##### list extend + enclose with set

```python
current = 'selenium'
elem_list = ['parsel', 'bs4', 'selenium']

elem_type_list = [current]
elem_type_list.extend(elem_list)

print(elem_type_list)
print(set(elem_type_list))
# ['selenium', 'parsel', 'bs4', 'selenium']
# {'bs4', 'selenium', 'parsel'}
```

똑같은 결과다.

##### set update

```python
current = 'selenium'
elem_list = ['parsel', 'bs4', 'selenium']
elem_type_list = {current}

result = {_type for _type in elem_list}
elem_type_list.update(result)
print(result)
# {'selenium', 'bs4', 'parsel'}
```

의도한대로 출력된다.

##### list comprehension + extend

```python
current = 'selenium'
elem_type_list = [current]
elem_list = ['parsel', 'bs4', 'selenium']

suffix = [_type for _type in elem_list if _type != current]
elem_type_list.extend(suffix)
print(elem_type_list)
# ['selenium', 'parsel', 'bs4']
```

같이 의도대로 동작한다.
리스트로 작성된 코드가 가장 알아보기 쉽다고 생각한다.
