---
layout: post
title: 'Docker 용량 관리'
image: img/docker-default.jpeg
author: [Dan]
date: 2021-09-23
tags: ['docker']
excerpt: '도커가 하드에 공간을 너무 많이 차지한다.'
draft: false
---

## 원인

나는 맥북 2019 air 기본 모델을 사용하고 있다. 하드 용량이 128인데, `OS`, `Application` 등을 설치하다보면 용량이 부족해지는 순간이 자주 온다.

`xcode`를 업데이트하기 위해 용량을 줄이려고 정리를 하고 있었는데, `finder`의 `view option`을 수정해서 `size`를 보면서 용량을 많이 차지하는 곳을 찾다보니 `/Users/<username>/Library/Container/Docker`가 용량을 64 기가바이트를 차지하고 있었다.

### 찾았다

이제 원인을 찾았으니 파일을 지우면 된다고 생각했다. `/Users/<username>/Library/Container/Docker`은 `Directory` 경로이기 때문에 64 기가바이트의 용량을 차지하는 파일을 찾았다.

파일은 아래 경로에 있었다.  
`/Users/<username>/Library/Container/Docker/Data/vms/0/data/Docker.raw`  
뭔가 이상했다. 파일 하나가 용량을 너무 많이 차지하고 있었으니까. 어떻게 해야될지 고민했다.

### 지우면 안될거 같이 생겼는데

내 노트북 용량의 반을 차지하는 녀석이지만, 왠지 그냥 지우면 `Docker`가 동작하지 않을 것 같다는 예감이 들었다. 이 파일이 뭔지 알아봤다.

이미 많은 사람들이 나처럼 이 녀석에 대해 궁금해하고 있었다. (아마 다들 용량이 부족한가보다.)

여러 글들에서 귀결되는 내용은 이랬다.

> `Docker`는 `container`가 실행되기 전에 미리 디스크의 일부 용량을 할당받아서 차지한다. (기본 값이 64 기가바이트 인듯) 이 용량은 사용자가 설정할 수 있는데 더 늘릴 수도 있고 , 더 줄일 수도 있겠다.

역시 지우면 안될거 같더라

## 해결

**Official docker mac dist utilization** : [solution](https://docs.docker.com/desktop/mac/space/)

나는 `Docker desktop`으로 `Docker daemon`을 켜고 끄면서 사용하고 있다. 그래서 공식 문서의 가이드대로 했다.

`Preferences` -> `Resources` -> `Advanced` -> `Disk image size`

### 말꼬리

그런데 공식 문서를 읽다보면 [파일이 너무 큰 경우](https://docs.docker.com/desktop/mac/space/#if-the-file-is-too-big) 아래 방법을 권하고 있다.

- 더 큰 드라이브로 옮긴다.
- 필요없는 컨테이너 혹은 이미지를 지운다.
- 허용 가능한 최대 파일 사이즈를 줄인다.

내 생각엔 위에서 아래로 우선순위가 있어 보인다. 되도록 64 기가바이트를 지켜주는 것이 좋겠다.  
컴퓨터 용량이 128 기가바이트가 아니라면..

---

## References

- [Docker official article](https://docs.docker.com/desktop/mac/space) about disk management
- [`iamjjanga`님 블로그 글](https://iamjjanga.tistory.com/50)
