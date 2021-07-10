---
layout: post
title: 'Docker run django'
image: img/docker-default.jpeg
author: [Dan]
date: 2021-07-10
tags: ['docker']
excerpt: 'docker로 django 서버 띄우기'
---

## TLDR

build dockerfile with below code

```dockerfile
ENTRYPOINT [ "python3", "manage.py", "runserver", "0.0.0.0:8000"]
```

run docker with below script

```shell
docker run --rm -t -p 8000:8000 <image-name>:<tag>
```

---

## 시행착오

`django`로 만든 웹 앱을 dockerize하는 과정을 진행했다.  
내가 만든 `dockerfile`은 아래와 같았는데, 실행하면 localhost로 접근이 안됐다.

```dockerfile
FROM python:3.8

ENV SRC_DIR='/workspace'
ENV SECRET_KEY='%$=rdc5mxoz96)zd#kn#e*fza-b%scx2&0fd6=3v5z+ii_zn(0'
EXPOSE 8000

COPY . ${SRC_DIR}
WORKDIR  ${SRC_DIR}
RUN pip3 install -r requirements.txt
WORKDIR ${SRC_DIR}/mailproject
RUN python3 ./manage.py makemigrations
RUN python3 ./manage.py migrate
RUN python3 ./manage.py shell < ./init.py

ENTRYPOINT [ "python3", "manage.py", "runserver"]
```

> django secret은 [Djecrety](https://djecrety.ir/)에서 생성했다.

그러나 실행한 웹앱에 127.0.0.1로 접근이 되지 않았다.

```dockerfile
ENTRYPOINT [ "python3", "manage.py", "runserver", "0.0.0.0:8000"]
```

그리고 docker를 실행할 때 아래와 같이 실행하면 된다.

```shell
docker run --rm -t -p 8000:8000 <image-name>:<tag>
```

실행할 때 `--network`에 `"host"`를 추가해줬는데, 접근이 되지 않아서 제외하고 실행했다. (왜 안되는지에 대해서는 알아보지 않았다.)

---

## References

- [flyingsquirrel님 medium](https://flyingsquirrel.medium.com/django-%EC%BD%94%EB%93%9C%EB%A5%BC-docker-container%EB%A1%9C-%EC%A0%95%EC%83%81%EC%A0%81%EC%9C%BC%EB%A1%9C-%EB%9D%84%EC%9B%A0%EC%A7%80%EB%A7%8C-local%EC%97%90%EC%84%9C-%EC%97%B4%EB%A6%AC%EC%A7%80-%EC%95%8A%EC%9D%84-%EB%95%8C-bff5f3b1f97b)
- [seulcode님 tistory](https://seulcode.tistory.com/231)
- [stackoverflow MrMins 답변](https://stackoverflow.com/questions/46325098/expose-port-8000-using-docker-and-django)
- [stackoverflow docker network](https://stackoverflow.com/questions/24319662/from-inside-of-a-docker-container-how-do-i-connect-to-the-localhost-of-the-mach)
- [dockerfile repo](https://github.com/xoxwgys56/mail-subscription-system)
