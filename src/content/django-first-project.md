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

## Why Django ?

backend framework로 `django`를 선택한 이유는 현재 시점에서 나에게 가장 친숙한 언어가 `python3`이기 때문이다. 회사에서 계속 `python3`을 써왔고 친숙한 언어의 framework를 사용하는 것이 분명한 장점이 존재한다고 생각하기 때문이었다.

이 생각은 꽤나 들어맞는 생각이었다. 어느정도 `python3`를 이용한 unittest(`pythone builtin unittest` or `pytest`)에 대해서 알고 있었기 때문에 `django`의 test를 작성하는 것은 그리 어려운 일은 아니었다.  
하지만 `ORM`과 `routing`(url, path)에 대해 어떻게 동작하는지 잘몰랐기 때문에 알아보면서 프로젝트를 진행하게 되었다. 또한 `RESTful`하게 API를 구현하고 싶었다.

## API

프로젝트에서 만들어야 하는 것은 API server였다. 나는 `RESTful`하게 만들고 싶었고,

---

## References

- [DRF versioning](https://www.django-rest-framework.org/api-guide/versioning/)
