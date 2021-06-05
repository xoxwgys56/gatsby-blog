---
layout: post
title: 'with catch'
image: 'img/python-default.jpeg'
author: [Dan]
date: 2021-06-05
tags: ['python', 'IO', 'exception']
excerpt: 'about "with" statement'
---

## `with` statement

python에서 `with`문을 사용하는 것은 파일을 읽을 때, 장점이 있다.

```python
with open("./note.txt") as f:
    text = f.read()
    # f.close()
```

`with`문이 종료할 때 별도로 `.close()`를 하지 않아도 종료해주기 때문에 안전하게 파일에 접근할 수 있다.  
그렇다면 내 생각엔 `with`문으로 파일을 읽는 동안 발생하는 에러에 대해서 `with except`문같은 구문이 있지 않을까 생각했다.

---

## 결론

결론부터 말하면, 그러한 문법은 존재하지 않았다.  
아래의 답변은 `with`문을 `try`로 감싸거나, 파일을 열 때 `try`로 감싸고 그 후 에러가 없는 경우 `with`를 쓰는 코드를 보여주고 있다.

1. try-with

```python
from __future__ import with_statement

try:
    with open( "a.txt" ) as f :
        print f.readlines()
except EnvironmentError: # parent of IOError, OSError *and* WindowsError where available
    print 'oops'
```

2. try open-else with

```python
try:
    f = open('foo.txt')
except IOError:
    print('error')
else:
    with f:
        print f.readlines()
```

## 내 결론

난 위의 방법이 아닌 그저 `with`문을 안쓰는 쪽을 선택했다.

---

## Deep dive

그런데 아직도 이상하다. 왜 지원해주지 않는걸까?  
stackoverflow [catching-an-exception-while-using-a-python-with-statement](https://stackoverflow.com/questions/713794/catching-an-exception-while-using-a-python-with-statement)에서 [a_guest](https://stackoverflow.com/users/3767239/a-guest)의 답변에 따르면 `ContextManager`때문이라고 한다.

자세한 내용까지는 다 이해하지 못했지만, 개략적으로 이야기하자면 `ContextManager`의 동작방식때문이라고 한다. PEP 343의 예시에 따르면 아래의 코드와 `with`문이 동일한 형태라고 한다.

```python
import sys

try:
    mgr = ContextManager()
except ValueError as err:
    print('__init__ raised:', err)
else:
    try:
        value = type(mgr).__enter__(mgr)
    except ValueError as err:
        print('__enter__ raised:', err)
    else:
        exit = type(mgr).__exit__
        exc = True
        try:
            try:
                BLOCK
            except TypeError:
                pass
            except:
                exc = False
                try:
                    exit_val = exit(mgr, *sys.exc_info())
                except ValueError as err:
                    print('__exit__ raised:', err)
                else:
                    if not exit_val:
                        raise
        except ValueError as err:
            print('BLOCK raised:', err)
        finally:
            if exc:
                try:
                    exit(mgr, None, None, None)
                except ValueError as err:
                    print('__exit__ raised:', err)
```

코드에서는 아래와 같은 경우에서 `raise`가 발생한다. (코드에서는 print로 표시되어 있다.)

- `ContextManager.__init__`
- `ContextManager.__enter__`
- the body of the `with`
- `ContextManager.__exit__`

`with`-`except`를 쓸 수 없는 이유는 `raise`가 여러 곳에서 발생하기 때문이라고 이해했다.
물론 `except`로 발생하는 모든 예외를 잡을 수 있을 것이다. 하지만 그것은 잘못된 표현법이다.  
왜냐하면 외부에서 봤을 때 `with`문은 한줄이기 때문에 하나의 뎁스?의 코드가 동작하고 반환되는 것 같지만 실제로는 여러 뎁스의 동작이 존재하기 때문에 명시적으로 `with`-`except`문을 사용하게 되면, 사용자는 `with`에서 발생한 에러에 대해 추상적으로 판단하게 될 것이다.

어쨌든 `with`문은 굉장히 추상화되어 있는 구문이다.  
좋은 표현법이라고 하기 어렵겠다.

---

## References

> stackoverflow [catching-an-exception-while-using-a-python-with-statement](https://stackoverflow.com/questions/713794/catching-an-exception-while-using-a-python-with-statement)

> [python 컨텍스트 관리자](https://docs.python.org/ko/3/library/stdtypes.html#context-manager-types)

> [how-should-i-catch-exceptions-raised-by-with-openfilename-in-python](https://stackoverflow.com/questions/32685951/how-should-i-catch-exceptions-raised-by-with-openfilename-in-python)

> [PEP 343](https://www.python.org/dev/peps/pep-0343/)
