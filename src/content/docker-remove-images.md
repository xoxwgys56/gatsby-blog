---
layout: post
title: 'Remove docker image'
image: img/docker-default.jpeg
author: [Dan]
date: 2021-06-05
tags: ['docker']
excerpt: 'docker 이미지 지우기'
---

도커로 이미지를 빌드하다보면 용량을 많이 차지하게 된다. 나의 경우 테스트 이미지를 빌드할 때는 빌드할 때마다 별도의 버전을 지정하지 않는데, 그러면 이전에 생성된 해당 버전의 이미지는 `none`이 되버린다.

## Remove `none` image

```shell
docker rmi $(docker images -f "dangling=true" -q)
```

```shell
docker image prune
```

## Remove all images

이미지를 다 지워버리고 싶은 경우 아래의 명령으로 지우면 된다.

```shell
docker rmi $(docker images -a -q)
```

## Remove with image name

```shell
docker rmi $(docker images | grep <imagename>)
```

혹은

```shell
docker rmi $(docker images <completeimagename> -a -q)
```

In Windows PowerShell:

```shell
docker rmi $(docker images --format "{{.Repository}}:{{.Tag}}"|findstr "imagename")
```

---

## References

> 권윤학님 블로그 [[ Docker ] dangling image ( 이름 없는 이미지 / none 이미지 / <none>:<none> 이미지) 제거](https://web-front-end.tistory.com/102)

> 배워서 남주자님 블로그 [Docker container & image 모두 삭제](https://countryxide.tistory.com/86)

> stackoverflow [How to remove docker images based on name?](https://stackoverflow.com/questions/40084044/how-to-remove-docker-images-based-on-name)
