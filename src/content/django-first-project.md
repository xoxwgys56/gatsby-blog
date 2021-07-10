---
layout: post
title: 'Django 첫 프로젝트 회고'
image: img/django-default.jpeg
author: [Dan]
date: 2021-07-10
tags: ['django', 'python']
excerpt: 'retrospect about my first django project'
draft: true
---

> 나는 `django` 전문가가 아니다. 그렇기 때문에 아래의 내용 중 잘못된 내용이 포함되어 있을 수도 있다.  
> 이를 감안하여 읽어주기 바란다.

## Why Django ?

backend framework로 `django`를 선택한 이유는 현재 시점에서 나에게 가장 친숙한 언어가 `python3`이기 때문이다. 회사에서 계속 `python3`을 써왔고 친숙한 언어의 framework를 사용하는 것이 분명한 장점이 존재한다고 생각하기 때문이었다.

이 생각은 꽤나 들어맞는 생각이었다. 어느정도 `python3`를 이용한 unittest(`pythone builtin unittest` or `pytest`)에 대해서 알고 있었기 때문에 `django`의 test를 작성하는 것은 그리 어려운 일은 아니었다.  
하지만 `ORM`과 `routing`(url, path)에 대해 어떻게 동작하는지 잘몰랐기 때문에 알아보면서 프로젝트를 진행하게 되었다. 또한 `RESTful`하게 API를 구현하고 싶었다.

## Initial setup with DRF

프로젝트에서 만들어야 하는 것은 API server였다. 나는 `RESTful`하게 만들고 싶었고, 그로 인해 `Django REST framework`(이하 `DRF`)를 사용하게 되었다. 처음 프로젝트를 만들고 설치한 의존성은 아래와 같았다.

```pipfile
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
django = "*"
djangorestframework = "*"
loguru = "*"

[dev-packages]
black = "*"

[requires]
python_version = "3.8"

[pipenv]
allow_prereleases = true
```

API server를 만들어야하기 때문에 당연히 `RESTful`하게 만들어야지라는 생각을 했다. (`RESTful`이라는 것이 기술이 아니라 디자인이니까) 그래서 `RESTful`하게 만드려면 어떻게해야 되는지 알아보게 됐다.

[RESTful API in Django](https://berkbach.com/restful-api-in-django-16fc3fb1a238)을 읽으면서, `djangorestframework`에 대해 알게되었고 이를 설치하면서 초기 세팅을 마무리했다. ([REST API at a glance](https://berkbach.com/%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C-%EC%82%B4%ED%8E%B4%EB%B3%B4%EB%8A%94-rest-api-79422dfc0a7d)도 `REST`에 대해 잘정리되어 있으니 읽어보기 바란다.)

나는 기존에 `django` offical tutorial만 보고 `django`에 대해 익혔는데, 내가 느끼기에 부족한 부분이 많았다. 예를 들면 아래와 같은 내용이다.

- custom user implementation
- api versioning
- how to handling secret key
- testing

물론 기초적인 내용에 포함되지 않는 내용이지만, 예시 프로젝트가 몇개 있었더라면 이해하기 더 편했을 것 같다. (내가 못찾은걸까) 어쨌든 나는 프로젝트를 진행하기 위해 하나 하나 알아가면서 찾아보게 되었다.  
초기에 `DRF`와 함께 시작한 것은 장점도 있고, 단점도 있었다.

### Pros with DRF

[django-rest-framework](https://www.django-rest-framework.org/)에서 제공하는 [tutorial](https://www.django-rest-framework.org/tutorial/quickstart/)이 있다. 이를 통해 `django`에 대한 새로운 관점을 갖게 되었다. 그리고 `django`도 debugging 모드로 실행하면, url이 잘못되었는지 응답이 처리된 결과를 보기 좋은 dashboard에서 보여주는데 `DRF`도 같은 기능을 제공해주어 편리했다.  
또한 url routing은 설명이 헷갈리긴 했지만 (가능한 여러 경우의 수를 보여줘서 무엇을 선택해서 해야하는지 고민하게 되었다. 기초적인 내용부터 접근해야 하는데, 난 심화된 혹은 사람들이 잘 안쓰는 기능을 써보기 위해 욕심을 냈다. 이것으로 인해 많은 시간을 소요하게 되었다.) 결과적으로 url 구성하는데, 도움이 되었다.

### Cons with DRF

내 입장에서 가장 단점은 검색할 때, 늘 `drf`라는 키워드를 포함해서 검색하기 시작했다는 것이다. `DRF`도 `django`의 확장 라이브러리인데, `django`보다 `DRF`관점에서 보려고 했던 것이 오히려 독으로 작용했다고 생각한다.  
그리고 내가 만든 프로젝트의 API는 그리 많지 않다. 복잡하지도 않다. `DRF`가 없어도 해결할 수 있었을 것 이다. (그래도 있는게 좋은거 같긴한데..)  
그 외에 단점은 없던 것 같다.

---

## Model

난 먼저 model을 만들기 시작했다. 내가 만드는 프로젝트는 메일 서버였다. 사용자가 특정 테마를 구독하면, 해당 사용자들에게 메일을 보내는 서버였다. 처음에는 "굉장히 단순한 기능을 하네", "만들기 쉽겠네"라고 생각했는데 마냥 그렇진 않았다.

### User

구독하는 사용자를 만들 때, 고민이 생겼다.

1. 사용자가 구독 정보를 갖고 있게 할까?
   - 그렇다면 하나의 컬럼에 여러 개의 정보를 갖고 있어야하나?
   - 이것은 [Many to Many](https://docs.djangoproject.com/en/3.2/topics/db/examples/many_to_many/) 관계인가?
2. 구독 정보를 다른 table이 갖고 있게 할까?
   - table을 테마마다 만들어야 하나?
   - 하나의 table에서 `User`와 `Theme`의 관계를 갖고 있어야 하나?

난 방법1에 대해서는 잘못된 방법이라고 생각했다. 내가 알기론 RDB에서는 하나의 컬럼에 하나의 정보를 담고 있으니까.  
그래서 나는 방법 2를 적용하기로 한다.

> 하나의 컬럼에 여러 개의 정보를 저장할 수 있는 방법이 없는 것은 아니다. [이런](https://stackoverflow.com/questions/24241953/sql-query-of-multiple-values-in-one-cell) 방법도 있고, `|`로 구분해 저장하는 방법도 있다. 그 외에 위에서 언급한 `Many to Many`도 방법이 될 수 있는 것 같지만 `Many to Many`는 사용해보지 않아서 설명하지 않도록 하겠다.

방법 2를 사용하면서 나는 다시 의문이 생겼다. `RDB`는 관계형 DB인데, 관계를 나타내는 table도 충분히 존재할 수 있겠다는 생각이 들었다. (~~아마 학부생 때 배웠는데, 까먹은거겠지.~~) 그래서 이에 대해 알아보게 되었다.

#### junction table

처음에 `junction table`이 아니라 `bridge table`이라는 이름으로 이해하고 있었다. 그래도 단어의 의미를 보면 둘 `junction`이 더 어울리긴 하다. [SQL 연결테이블(junction table)과 데이터 패턴](https://m.blog.naver.com/50after/220947766913)가 많은 도움이 됐다.

---

## References

- REST

  - [RESTful API in Django](https://berkbach.com/restful-api-in-django-16fc3fb1a238)
  - [REST API at a glance](https://berkbach.com/%EA%B0%84%EB%8B%A8%ED%95%98%EA%B2%8C-%EC%82%B4%ED%8E%B4%EB%B3%B4%EB%8A%94-rest-api-79422dfc0a7d)
  - [RESTful API 설계 가이드](https://sanghaklee.tistory.com/57)

- Django

  - [MDN tutorial](https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Introduction)

- [DRF versioning](https://www.django-rest-framework.org/api-guide/versioning/)

- Model
  - [SQL 연결테이블(junction table)과 데이터 패턴](https://m.blog.naver.com/50after/220947766913)
