---
layout: post
title: 'Python nested class'
author: [Dan]
tags: ['python', 'dataStructure']
image: img/python-default.jpeg
date: '2021-05-10'
draft: false
excerpt: 파이썬 내부 클래스에 대해
---

클래스에는 다양한 데이터 멤버와 함수가 포함되어 있으며 이러한 멤버에 액세스 할 수있는 개체를 만들 수 있습니다.

객체 지향 프로그래밍 언어 인 Python에는 이러한 여러 클래스의 객체가 많이 있습니다. 파이썬에는 클래스의 인스턴스가 생성 될 때마다 호출되는`__init__`라는 중요한 생성자가 있고, 클래스의 현재 인스턴스를 참조하는`self` 키워드도 있습니다.

중첩 클래스 (내부 클래스라고도 함)는 다른 클래스 내에 정의됩니다. 모든 객체 지향 프로그래밍 언어에서 매우 일반적으로 사용되며 많은 이점을 가질 수 있습니다. 실행 시간을 개선하지는 않지만 관련 클래스를 함께 그룹화하여 프로그램 가독성 및 유지 관리를 지원하고 중첩 된 클래스를 외부 세계로부터 숨길 수 있습니다.

다음 코드는 중첩 된 클래스의 매우 간단한 구조를 보여줍니다.

```
class Dept:
    def __init__(self, dname):
        self.dname = dname
    class Prof:
        def __init__(self,pname):
            self.pname = pname

math = Dept("Mathematics")
mathprof = Dept.Prof("Mark")

print(math.dname)
print(mathprof.pname)

```

출력:

```
Mathematics
Mark

```

내부 클래스에 직접 액세스 할 수는 없습니다. `outer.inner` 형식을 사용하여 객체를 만듭니다.

외부 클래스의 중첩 클래스에 액세스 할 수 있지만 그 반대의 경우에는 액세스 할 수 없습니다. 외부 클래스의 중첩 클래스에 액세스하려면`outer.inner` 형식 또는`self` 키워드를 사용할 수 있습니다.

아래 코드에서 위의 클래스를 약간 변경하고 상위 클래스를 사용하여 중첩 된 클래스의 함수에 액세스합니다.

```
class Dept:
    def __init__(self, dname):
        self.dname = dname
        self.inner = self.Prof()
    def outer_disp(self):
        self.inner.inner_disp(self.dname)
    class Prof:
        def inner_disp(self,details):
            print(details, "From Inner Class")

math = Dept("Mathematics")

math.outer_disp()

```

출력:

```
Mathematics From Inner Class

```

Python에 여러 중첩 클래스를 가질 수도 있습니다. 이러한 상황을 하나 이상의 내부 클래스가있는 다중 중첩 클래스라고합니다.

중첩 된 클래스 내에 중첩 된 클래스가있는 경우도있을 수 있으며이를 다중 레벨 중첩 클래스라고합니다.

다음 코드는 다단계 중첩 클래스의 매우 간단한 예를 보여줍니다.

```
class Dept:
    def __init__(self, dname):
        self.dname = dname
    class Prof:
        def __init__(self,pname):
            self.pname = pname
        class Country:
            def __init__(self,cname):
                self.cname = cname

math = Dept("Mathematics")
mathprof = Dept.Prof("Mark")
country = Dept.Prof.Country("UK")

print(math.dname,mathprof.pname,country.cname)

```

출력:

```
Mathematics Mark UK
```
