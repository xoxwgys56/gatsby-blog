---
layout: post
title: 'Pytest What it is?'
author: [Dan]
tags: ['python', 'pytest', 'unittest']
image: img/python-default.jpeg
date: '2021-05-19'
draft: false
excerpt: pytest에 대해 알아보자.
---

# pytest

> https://docs.pytest.org/en/6.2.x/

`pytest`는 유닛테스트 프레임워크이다.

```python
# content of test_sample.py
def inc(x):
    return x + 1


def test_answer():
    assert inc(3) == 5
```

위의 url에 들어가면 나와있는 예시이다.  
파일명이 `test_sample.py`라는 점을 주목하자.
`pytest`는 정해진 명명법에 따라 작성된 `.py` 스크립트에 대해 테스트를 시도한다.  
또한 테스트 대상이 되는 함수, 클래스 등도 마찬가지로 정해진 이름 규칙이 있다.  
위 코드의 경우 `test_`가 함수 이름 앞에 붙었다.

```bash
$ pytest
=========================== test session starts ============================
platform linux -- Python 3.x.y, pytest-6.x.y, py-1.x.y, pluggy-0.x.y
cachedir: $PYTHON_PREFIX/.pytest_cache
rootdir: $REGENDOC_TMPDIR
collected 1 item

test_sample.py F                                                     [100%]

================================= FAILURES =================================
_______________________________ test_answer ________________________________

    def test_answer():
>       assert inc(3) == 5
E       assert 4 == 5
E        +  where 4 = inc(3)

test_sample.py:6: AssertionError
========================= short test summary info ==========================
FAILED test_sample.py::test_answer - assert 4 == 5
============================ 1 failed in 0.12s =============================
```

테스트 결과는 위와 같이 나온다.  
위 경우는 실패한 경우인데, 만약 성공했다면 `pass`라고만 나오고 어떻게 성공했는지에 대한 결과는 나오지 않는다. (이건 좀 아쉽다. 아마 보여주는 기능이 있을 것 같다)
만약 커스텀 로그가 필요한 경우 `print`함수를 사용하면, 실패한 경우만 출력되니 참고하자.

# naming convention

### file name

- `python3.7.5` 까지
  - `test_<file_name>.py` 앞에 지정해주어야한다.
- `python3.8.5` 에서는

  - `test_<file_name>.py` 도 가능하며, `<file_name>_test.py` 도 가능하다.

- test_example.py
- example_test.py

### class name

`class Test<class_name>` 로 앞에 `Test` 를 붙여주어야 한다.

- class TestExample

### method or function

`def test_<method or function name>()` 로 앞에 `test_` 를 붙여주어야 한다.

- def test_example()

# mutiple test

테스트를 한다면 여러개의 테스트케이스로 테스트하고 싶을 것 이다.
(테스트케이스를 작성하는 방법에 대해 정해진 방법에 대해 찾지 못했다.)  
어떤 테스트를 의도했는지에 따라 다르게 접근할 것이라고 생각되는데 이 글에서는 내가 생각하는 방법 2가지를 제안한다.

### 1. parameter 사용

> https://docs.pytest.org/en/6.2.x/parametrize.html

`pytest`는 파라미터를 제공한다. 아래의 코드를 보자.

```python
# content of test_expectation.py
import pytest


@pytest.mark.parametrize("test_input,expected", [("3+5", 8), ("2+4", 6), ("6*9", 42)])
def test_eval(test_input, expected):
    assert eval(test_input) == expected
```

테스트하는 함수에 `@pytest.mark.parametrize` 노테이션을 사용할 수 있다.  
(이는 앞으로 언급할 `fixture`에도 사용할 수 있는 기능이다.)

코드를 보면 예상할 수 있듯이 3개의 테스트케이스를 실행한다.
실행 과정만 나열한다면 아래와 같을 것 이다.

```python
# 함수 선언부와 노테이션을 생략했다.
...
assert eval("3+5") == 8
...
assert eval("2+4") == 6
...
assert eval("6*9") == 42
```

3번째 `assert`문에서 에러가 발생할 것이고 해당 테스트에 대해서만 에러가 발생할 것 이다.

### 2. 테스트하고 싶은 수만큼 테스트 함수 작성

이 방법은 테스트 대상이 되는 코드를 반복 호출할 경우에는 비효율적일 것 이다.

```python
def test_eval_sum():
    assert eval("3+5") == 8

def test_eval_mult():
    assert eval("6*9") == 42

# 위와 같은 코드로 아래처럼 작성할 수 있을 것이다.

def test_eval():
    assert eval("3+5") == 8
    assert eval("6*9") == 42
```

조금 다른 경우로 볼 수 있지만, 내 경우 스크롤 다운 기능을 위의 기능으로 구현했다.

```python
def test_scroll_down_once():
    # scroll down once

@pytest.mark.parametrize("scroll_times", [1, 2, 3, 4])
def test_scroll_n_times():
    # scroll down n times
```

결국 아래의 방법에서도 파라미터를 썼지만.. 어쨌든 이러한 방식으로 사용했다.

# fixture

> [그린몬님 블로그](https://greenmon.dev/2019/10/15/pytest-fixture.html)

> [pytest official](https://docs.pytest.org/en/6.2.x/fixture.html)

> [번역본 (내 블로그 ㅎ)](https://dankim.gatsbyjs.io/pytest-fixture/)

유닛테스트를 하다보면 반복되는 준비과정이 필요할 수 있다.
내 경우는 `selenium`을 통해 테스트를 했는데, 그 과정에서 `webdriver`를 초기화 해주어야했다.  
그래서 초기화하는 과정을 `fixture`로 만들어 사용했다.

```python
from selenium.webdriver.remote.webdriver import WebDriver
from selenium.webdriver.chrome.options import Options as ChromeOptions
from selenium import webdriver
import pytest

url_list = [
    "https://www.naver.com",
    "https://www.google.com",
]


@pytest.fixture(
    scope="function",
    params=url_list,
)
def initialize_browser(request) -> WebDriver:
    """just initialize browser"""
    chrome_options = ChromeOptions()
    chrome_options.add_argument("--headless")
    chrome_options.add_argument("--disable-gpu")
    chrome_options.add_argument("--no-sandbox")
    chrome_options.add_argument("--disable-dev-shm-usage")
    chrome_options.add_argument("--single-process")
    # add options if you need
    driver = webdriver.Chrome(
        chrome_options=chrome_options,
    )
    driver.get(request.param)
    time.sleep(1)

    yield driver

    driver.quit()
```

코드의 내용을 간단히 설명하자면

### 파라미터

위에서 설명한 것과 같이 `fixuture`에서도 파라미터를 사용할 수 있다.  
대신 문법이 조금 다르다는 것을 볼 수 있는데, 함수의 파라미터인 `request`는 고정된 이름이다. ( 다른 이름 쓰면 안된다)  
파라미터는 위의 코드처럼 `request.param`으로 접근할 수 있는데 만약 파라미터를 2개 이상 제공하고 싶다면 아래의 방법을 제안해본다.

```python
...
scroll_pagination_list = [
    ("https://www.naver.com", "some css selector"),
    ("https://www.google.com", "some xpath"),
]
... # inside of function
    (url, path) = request.param
    # OR
    driver.get(request.param[0])
...
```

### yield return

> https://docs.pytest.org/en/reorganize-docs/yieldfixture.html

선언한 `fixture` 함수는 `yield` 문법으로 웹드라이버를 반환하고 있는데  
이렇게 작성하면 사용할 값은 `yield`를 통해 반환하고, 이 값이 사용된 다음에 수행해야 하는 동작(위 코드의 경우 브라우저 종료)하도록 구현할 수 있다.

## 그럼 이 `fixture`를 어떻게 사용할까?

호출하는 방법은 굉장히 간단하다.

```python
def test_parse_element(initialize_browser):
    webdriver = initialize_browser
    # webdriver.find_element_by_xpath .. blah
```

함수 내에서 위의 코드처럼 호출하면 반환값을 받을 수 있다.  
(이 부분에 대해서 아직 명확하게 알지 못하나 `webdriver`는 반환값을 보유한 것이지 함수 자체를 보유한 것 아님을 인지하자.)

### 반복되는 `fixture` 호출

위에서 선언한 웹드라이버를 초기화하는 코드의 경우  
여러 코드에서 참조할 수 있다. 이 경우 해당 `fixture`를 `conftest.py` 내에 위치시킬 수 있다.
그러면 `pytest`에서 `conftest.py`에 있는 `fixture`를 참조해서 사용한다.

# Skip

> https://stackoverflow.com/questions/38442897/how-do-i-disable-a-test-using-pytest

테스트 코드가 많아질 경우 테스트 시간이 오래 걸릴 수도 있다.  
그렇다고 테스트하지 않는 코드를 주석처리해버리는 것은 _아름답지 못하다._

```python
@pytest.mark.skip(reason="no way of currently testing this")
def test_the_unknown():
    ...

import sys
@pytest.mark.skipif(sys.version_info < (3,3),
                    reason="requires python3.3")
def test_function():
    ...
```

이렇게 선언하면, 테스트 결과에서 `s`라고 표시되며 테스트가 skip된다.  
(노테이션을 여러 줄로 작성할 수 있다. `mark`와 `parametrize`를 함께 사용할 수 있다. 우선순위가 있는진 잘모르겠다.)
