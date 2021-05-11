---
layout: post
title: '"private" 상속'
image: img/shot-by-cerqueira-hTz9w338qTw-unsplash.jpeg
author: [Dan]
date: 2021-05-11
tags: ['python', 'designPattern']
excerpt: meaning of private in Python
---

> [https://stackoverflow.com/questions/20261517/inheritance-of-private-and-protected-methods-in-python](https://stackoverflow.com/questions/20261517/inheritance-of-private-and-protected-methods-in-python)

# Question

```python
class Parent(object):
    def _protected(self):
        pass

    def __private(self):
        pass

class Child(Parent):
    def foo(self):
        self._protected()   # This works

    def bar(self):
        self.__private()    # This doesn't work, I get a AttributeError:
                            # 'Child' object has no attribute '_Child__private'
```

# Answer

> 아래는 원문을 번역한 내용이다. 번역이 매끄럽지 못한 점 양해 바랍니다.

파이썬에는 `private` 모델이 없다. 고로 C*++, C# or Java*같은 `access modifier` 는 없습니다.

진짜 `protected` 혹은 `private` 모델은 없는 것 입니다.

`__<name>` (이름 뒤에 `__` 붙지 않은 경우)로 명명된 경우는, 상속할 때 이름이 충돌하는 것을 보호하기 위해서 _mangled_ 된 것입니다. 서브클래스는 `__private()` 메소드를 선언하여 가질 수 있고 이는 부모 클래스에서 같은 이름으로 명명된 멤버를 간섭하지 않습니다. (override하지 않습니다. 보이지 않으니까요)

클래스 이름을 `class private` 으로 고려한다고 해도 여전히 외부에서 접근할 수 있습니다. 그러나 충돌 할 가능성은 낮아집니다.

_Mangling_ 은 어떤 이름에 `_` 가 몇개 붙던지 클래스 이름이던지 (어떤 이름이고 존재하든 아니든 상관없이) 선언된 이름에 (변수, 클래스 메서드 등에게) `namespace` 를 부여한다. `Parent` class 내에서, 어떤 `__private` 선언자든 (컴파일 시점에) `_Parent__private` 이라는 이름으로 교체된다. (프로그램이 컴파일된 이후에 사용하는 이름은 `_Parent__private` 인 것이다. 디버거를 통해 확인할 수 있다.) 클래스가 정의된 모든 곳에서 `Child` class 선언 또한 `_Child__private` 으로 대체된다.

아래의 코드는 잘 동작한다:

```python
class Child(Parent):
    def foo(self):
        self._protected()

    def bar(self):
        self._Parent__private()
```

파이썬 구문 분석 문서 내의 [Reserved classes of identifiers](https://docs.python.org/2/reference/lexical_analysis.html#reserved-classes-of-identifiers) 를 보자. (파이썬2 문서)

- 파이썬3.6 문서 [식별자의 예약 영역](https://docs.python.org/ko/3.6/reference/lexical_analysis.html#reserved-classes-of-identifiers)

### Reserved classes of identifiers

`__*`

class-private names. 이 카테고리의 이름들을 클래스 정의 문맥에서 사용하면 뒤섞인 형태로 변형된다. 부모 클래스와 자식 클래스의 《비공개(private)》 어트리뷰트 간의 이름 충돌을 피하기 위함이다. 식별자 (이름) 섹션을 보라.

_번역은 알아보기 힘들다. 아래의 원문을 보자._

Class-private names. Names in this category, when used within the context of a class definition, are re-written to use a `mangled form` to help _avoid_ name clashes between “private” attributes of base and derived classes.

그리고 [documentation on names](https://docs.python.org/2/reference/expressions.html#atom-identifiers) 를 참조하자.

- 파이썬3.9 [식별자](https://docs.python.org/ko/3/reference/expressions.html#atom-identifiers)

**비공개 이름 뒤섞기(private name mangling)**:

클래스 정의에 등장하는 식별자가 두 개나 그 이상의 밑줄로 시작하고, 두 개나 그 이상의 밑줄로 끝나지 않으면, 그 클래스의 비공개 이름(private name) 으로 간주합니다. 비공개 이름은 그들을 위한 코드가 만들어지기 전에 더 긴 형태로 변환됩니다.

이 변환은 그 이름의 앞에 클래스 이름을 삽입하는데, 클래스 이름의 처음에 오는 모든 밑줄을 제거한 후, 하나의 밑줄을 추가합니다. 예를 들어, `Ham` 이라는 이름의 클래스에 식별자 `__spam` 이 등장하면, `_Ham__spam` 으로 변환됩니다.

이 변환은 식별자가 사용되는 문법적인 문맥에 무관합니다. 변환된 이름이 극단적으로 길면(255자보다 길면), 구현이 정의한 잘라내기가 발생할 수 있습니다. 클래스 이름이 밑줄로만 구성되어 있으면, 변환은 일어나지 않습니다.

_공식 번역은 뒤섞기라고 번역되어 있다._

`class-private` 이름은 사용하지 마라. 만약 너의 클래스의 서브클래스가 특정 이름을 쓸 수 없거나 클래스를 망칠 위험이 있어서 _특별하게_ 개발자들에게 말하는 것을 피하고 싶은게 아니라면. (애초에 이런 상황을 만들지 말아야지)

[PEP 8 Python Style Guide](https://www.python.org/dev/peps/pep-0008/#id49) 에서 이 `private name mangling` 에 대해 말하고 있다.

### original

If your class is intended to be subclassed, and you have attributes that you do not want subclasses to use, consider naming them with double leading underscores and no trailing underscores. This invokes Python's name mangling algorithm, where the name of the class is mangled into the attribute name. This helps avoid attribute name collisions should subclasses inadvertently contain attributes with the same name.

- Note 1: Note that only the simple class name is used in the mangled name, so if a subclass chooses both the same class name and attribute name, you can still get name collisions.
- Note 2: Name mangling can make certain uses, such as debugging and **getattr**(), less convenient. However the name mangling algorithm is well documented and easy to perform manually.
- Note 3: Not everyone likes name mangling. Try to balance the need to avoid accidental name clashes with potential use by advanced callers.

### translated

만약 너의 클래스가 서브클래스로 의도한거라면, 그리고 너의 서브클래스가 속성들을 가지지 않길 원한다면, `__<name>` 을 고려해봐라. 이것은 파이썬의 이름 `mangling` 알고리즘을 호출한다. (`_className__name` 을 생성하는 알고리즘이겠지...) 클래스 이름이 속성명 안에 뒤섞는다. (어느 정도 mixed in 된다는 뜻을 내포하는듯)

이것은 속서명 충돌을 피하고 서브 클래스가 부주의로 인해 같은 이름의 속성을 갖는 것을 피할 수 있게 돕는다.

- Note 1 : 간단한 클래스 이름만이 오직 `mangled name` 되어 사용된다는 것을 명심하라. 그러므로 만약 서브클래스가 똑같은 클래스명과 속성명을 선택한다면, 여전히 이름 충돌이 생기는 것이다.
- Note 2 : `Name mangling` 은 특정 기능으로 쓸 수 있다. 가령 디버깅 그리고 `__getattr__()` , 덜 간편한. 그러나 `name mangling` 알고리즘은 문서화가 잘되어 있고 쉽게 동작하도록 할 수 있다. (번역이 매끄럽지 못하다.
- Note 3 : 모두가 `name mangling` 을 좋아하진 않는다. 좋은 콜러에 의해 잠재적 사용에 의한 이름 충돌을 피하도록 시도해보라.
