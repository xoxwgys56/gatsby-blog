---
layout: post
title: '[번역] Pytest fixture'
author: [Dan]
tags: ['python', 'pytest', 'unittest']
image: img/python-default.jpeg
date: '2021-05-19'
draft: false
excerpt: fixture
---

> https://docs.pytest.org/en/6.2.x/fixture.html

> 번역이 매끄럽지 못한 부분이 있을 수 있다. 양해바란다.

# pytest fixtures: explicit, modular, scalable

[Software test fixtures](https://en.wikipedia.org/wiki/Test_fixture#Software)는 테스트 함수를 초기화합니다. 그들은 고정된 baseline을 제공하고, 그로 인해 신뢰되고 일관성있으며, 반복적인 테스트의 결과물을 얻을 수 있게 합니다. 초기화는 services, state 혹은 다른 환경 작업을 할 수 있습니다. 이것들은 arguments를 통해 테스트 함수에 의해 로 접근됩니다. 각각의 `fixture` 는 테스트 함수에 의해 이용되는데 이는 테스트 함수 정의의 전형적인 매개변수(앞으로 `fixture` 라고 부르겠습니다.)입니다.

pytest `fixture` 는 xUnit 스타일의 setup 및 teardown 함수를 극적인 향상을 제공합니다:

- `fixture` 명시적인 이름과 각 테스트 함수, 모듈, 클래스 혹은 전체 프로젝트에 의한 (선택적인) 활성화를 갖고 있습니다.
- `fixture` 는 모듈식의 구현이 되어 있습니다. 이는 각 `fixture` 이름이 `fixture function` 을 호출하고 이것은 다른 `fixture` 를 스스로 이용한다는 것을 의합니다.
- `fixture` 는 간단한 유닛에서 복잡한 함수적 테스트의 스케일까지 관리하며, parameterize `fixture` 와 설정에 의한 테스트 그리고 컴포넌트 옵션, 혹은 함수, 클래스, 모듈 혹은 전체 테스트 세션 스코프를 넘어선`fixture` 재사용을 허용합니다.
- teardown 로직은 쉽고, 안전하게 관리되며, `fixture` 가 얼마나 많이 사용되든 상관없고, 직접 혹은 마이크로단위 관리 조심스레 에러를 관리하지 않아도 정리하는 순서가 추가됩니다.

# What fixtures are

픽스쳐에 대해 들어가기 전에, 먼저 테스트가 무엇인지 봅시다.

간단한 용어로 보면, 테스트는 특정 활동에 대한 결과로 보입니다. 그리고 그 결과는 당신이 예상하는 것과 일치해야죠. 행동이라는 것은 실험적으로 측정하는게 아닙니다. 그래서 테스트를 작성하는 것이 도전적으로 힘든 것이죠.

"행동"은 어떤 시스템의 특정 상황과 자극에 대한 *동작의 응답(acts in response)*입니다. 그러나 정확히 **어떤(how)** 혹은 **왜(why)** 어떤 것이 완료되었는지는 **무엇이(what)** 끝났는지에 비해 중요한 것은 아닙니다.

당신은 테스트를 4단계로 쪼개서 생각할 수 있습니다.

1. Arrange
2. Act
3. Assert
4. Cleanup

> 이 다음 내용은 아직 번역하지 못했다..
