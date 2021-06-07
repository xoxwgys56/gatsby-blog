---
layout: post
title: 'AWS Lambda logging 잘남기기'
author: [Dan]
tags: ['aws', 'lambda', 'logging']
image: img/aws-default.jpg
date: '2021-06-05'
draft: true
excerpt: '어떻게하면 AWS Lambda 로그를 잘남길 수 있을까?'
---

## cloud watch

AWS Lambda는 실행되면 CloudWatch에 로그를 남긴다. 여러개의 Lambda가 실행된 경우, 로그를 찾기 힘들어지는데 이 경우 전체 로그에서 검색이 필요해진다.

Lambda 이름 혹은 지정된 로그 그룹에 들어가면 생성된 로그를 확인할 수 있는데, 여기에서 로그 스트림 이름은 실행된 Lambda와 별도의 이름을 가진다. (완전히 )

---

## Reference

> Medium [AWS 람다 로그 잘 남기고 추적하기](https://asleea88.medium.com/aws-%EB%9E%8C%EB%8B%A4-%EB%A1%9C%EA%B7%B8-%EC%9E%98-%EB%82%A8%EA%B8%B0%EA%B3%A0-%EC%B6%94%EC%A0%81%ED%95%98%EA%B8%B0-aws-lambda-logging-f097dddbbc52)

> [awslogs](https://github.com/jorgebastida/awslogs)
