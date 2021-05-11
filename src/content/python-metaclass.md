---
layout: post
title: 'Python what is metaclass'
author: [Dan]
tags: ['python', 'designPattern']
image: img/welcome-to-ghost.jpg
date: '2021-05-11'
draft: false
excerpt: 파이썬 메타클래스는 무엇이고, 어떻게 쓰는 것일까
---

> [https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python/6581949#6581949](https://stackoverflow.com/questions/100003/what-are-metaclasses-in-python/6581949#6581949)

> [https://tech.ssut.me/understanding-python-metaclasses/](https://tech.ssut.me/understanding-python-metaclasses/)

## 내가 알던 것과 다른데..

메타클래스에 관한 좋은 글을 찾았다. 읽어보았는데, 내가 알고 있는 메타클래스와 다른 점이 있었다.

내가 알고 있기론 메타클래스는 클래스를 정의하는 클래스였는데, 맞는 문장이지만 내가 생각하고 있는 것과는 달랐다.

클래스를 정의하는 클래스가 클래스 변수만 따로 저장한다는 의미가 아니다. (혹은 클래스의 내부 선언들을 보유)

난 메타클래스와 `name mangling` 을 함께 쓰면 될 것이라고 생각했고, 이를 이용해 속성에 대해서만 정의된 부모 클래스를 만들고 이를 메타클래스로 상속받고자 했다. 하지만 둘 다 틀린 생각이었다.

> 아래는 읽은 내용을 내 방식으로 정리한 글이다. 많은 내용이 생략되어 있으니, 원글을 참조바란다.

## 파이썬에서 모든 것은 객체

알고 있듯이 파이썬에서 모든 것은 객체이다. 변수, 함수, 클래스 모든 것이 객체이다.

파이썬에는 `str` , `int` 등의 선언된 객체들이 존재한다. 그 중 `type` 이라는 객체가 있다.

`type` 은 일반적으로 객체가 어떤 타입인지 알 수 있게 해주지만 클래스를 만드는 용도로 사용할 수도 있습니다.

아래와 같이 선언되어 있습니다.

```python
type(name, bases, attrs)
```

- **`name`**: 클래스의 이름
- **`bases`**: 부모 클래스의 튜플 (for inheritance, can be empty)
- **`attrs`**: 속성들의 딕셔너리

이를 이용해 아래와 같이 사용할 수 있습니다.

```python
>>> MyShinyClass = type('OurShinyClass', (), {}) # returns a class object
>>> print(MyShinyClass)
<class '__main__.OurShinyClass'>
>>> print(MyShinyClass()) # create an instance with the class
<__main__.OurShinyClass object at 0x1024f8ac0>
```

속성을 준다면 아래와 같습니다.

```python
>>> Foo = type('Foo', (), {'bar':True})
>>> print(Foo)
<class '__main__.Foo'>
>>> print(Foo.bar)
True
>>> f = Foo()
>>> print(f)
<__main__.Foo object at 0x8a9b84c>
>>> print(f.bar)
True
```

마찬가지로 상속도 가능합니다.

```python
>>> FooChild = type('FooChild', (Foo,), {}) # (Foo)라고 적으면 튜플 아니라 타입이라고 에러납니다.
>>> print(FooChild)
<class '__main__.FooChild'>
>>> print(FooChild.bar) # bar is inherited from Foo
True
>>> hasattr(FooChild, 'bar')
True
```

## 메타클래스란

메타클래스는 클래스를 만드는 '무언가'입니다.

`type` 은 클래스를 만들었습니다. 그러므로 `type` 은 메타클래스입니다.

(`type` 이 소문자인 이유는 `str`, `int` 등의 일관성을 위해서인 것으로 보입니다.)

Python3에서는 아래와 같이 메타클래스를 사용할 수 있습니다.

```python
class Foo(object, metaclass=something):
	...
```

Python2에서는 아래와 같이 메타클래스를 사용합니다.

```python
class Foo(object):
    __metaclass__ = something...
    [...]
```

Python은 `__metaclass__`가 클래스 선언에 있는지 확인합니다. (`metaclass` 가 파라미터에 있는지)

찾았다면, `Foo` 클래스의 객체를 생성하는데 이를 사용합니다.

없다면, `type` 을 사용합니다.

> 이 부분에 오번역이 있을 수 있으나, 중요하므로 원문을 함께 남깁니다.

> Python will look for **metaclass** in the class definition. If it finds it, it will use it to create the object class Foo. If it doesn't, it will use type to create the class.
> Read that several times.

메타클래스를 사용하면 다음과 같은 일이 쉬워집니다.

- 클래스 생성 가로채기
- 클래스 수정하기
- 수정된 클래스 반환하기

## 왜 쓸까요?

파이썬 구루 *Tim Peters*가 이야기했습니다.

> Metaclasses are deeper magic that 99% of users should never worry about it. If you wonder whether you need them, you don't (the people who actually need them to know with certainty that they need them and don't need an explanation about why).

> 메타클래스는 99%의 사용자는 전혀 고려할 필요가 없는 흑마법입니다. 만약 당신이 이게 정말로 필요할지에 대한 의문을 갖는다면, 필요하지 않습니다. (이게 진짜로 필요한 사람은 이게 진짜로 필요하다고 알고 있으면서, 왜 필요한지에 대해 설명할 필요가 없는 사람들입니다.)

실제 필요한 예시는 `Django ORM` 과 같은 경우입니다.

```python
class Person(models.Model):
    name = models.CharField(max_length=30)
    age = models.IntegerField()

person = Person(name='bob', age='35')
print(person.age)
```

위 코드는 `IntegerField` 객체를 반환하지 않습니다. 대신에 `int` 를 반환합니다. 심지어 데이터베이스에서 직접 가져올 수도 있습니다.

## 결론

- [monkey patching](http://en.wikipedia.org/wiki/Monkey_patch)
- class decorators

클래스 변형이 필요한 99%의 순간에 대부분 위 두 가지 방법을 사용하는 편이 훨씬 낫습니다.

98%의 시간에는 클래스 변형이 아예 필요하지 않습니다.
