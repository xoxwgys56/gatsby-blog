---
layout: post
title: '`pipenv`는 정말 좋다.'
image: 'img/python-default.jpeg'
author: [Dan]
date: 2021-06-05
tags: ['python', 'package', 'venv']
excerpt: '이거 정말 좋아. 아주 좋아.'
---

## Install

일단 설치부터 하고 시작하자. 어쨌든 당신은 `pipenv`를 쓰게 될 것이다.  
이 글에서는 미안하지만 `mac`기반 컴퓨터에 대해서만 다룰 것 이다. 🙇🏻‍♂️

`brew`가 설치되어 있다는 가정하에 설치한다.

```shell
# 무려 fomulae !
brew install pipenv
```

## Pipenv ?

`pip`를 대신하는 패키지관리자라고 보면 될 것 같다. 좀 더 많은 기능을 제공하는 관리자이다.  
사용법은 `pip`와 매우 흡사하다. 간단한 세팅을 통해 사용법을 익히면 좋을 것 같다.
빈 디렉토리를 생성하고 아래의 커맨드를 따라가보자. (이 빈 디렉토리를 `pipenv-prac`이라고 부르겠다. 같은 이름의 디렉토리를 생성하는 것은 아래의 설명을 따라가기에 더 편할 것 이다.)

```shell
# use installed python version
$ pipenv --python 3.8

# same as source ./venv/bin/activate
$ pipenv shell

# check Pipfile.
$ ls -al
(pipenv-prac) (base) ➜  pipenv-prac l
total 8
drwxr-xr-x  3 user_name  staff    96B Jun  5 16:04 .
drwxr-xr-x  4 user_name  staff   128B Jun  5 16:04 ..
-rw-r--r--  1 user_name  staff   138B Jun  5 16:04 Pipfile

# assign version is optional
$ pipenv install requests
$ pipenv install black --dev --pre
$ pipenv install pytest --dev
```

> `black`은 설치할 때 `--pre`를 붙여주어야 한다. [해당 이슈](https://github.com/microsoft/vscode-python/issues/5171) 그렇지 않으면 설치할 때 오류가 발생한다. 오류가 발생하면 해결하기 꽤나 까다로웠다.  
> 나의 경우 `Pipfile.lock`을 지우고 `pipenv lock`으로 새로 만들었다.
>
> 하나의 문제가 더 있다. 3.7버전의 python을 사용하는 경우 `pipenv install black --pre --dev`도 안될 수 있다. 왜 그러는진... 잘모르겠다.

위의 커맨드를 입력한 이후 `Pipfile`을 확인해보자.

```yml
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
requests = "*"

[dev-packages]
black = "*"
pytest = "*"

[requires]
python_version = "3.8"

[pipenv]
allow_prereleases = true
```

이 파일에서 `pipenv`의 장점을 느낄 수 있을 것이다.  
그렇다. `pipenv`는 명시적으로 python 버전을 표기해준다. 그리고 NodeJS의 `pacakges.json`과 같이 개발버전과 배포버전의 의존성을 다르게 관리할 수 있다.  
나의 경우 `pipenv`를 사용하게 된 계기는 `black`때문이었다.

AWS Lambda를 이용하는데, 의존성을 layer에 올렸을 때 `black`이 함께 포함되는 것이다. 용량면에서 그렇게 큰 차이는 아니겠지만 굳이 같이 올릴 필요가 없다는 생각이 들어서 찾아보니 아주 좋은 도구가 있었다.

> 하지만 결론은 같은 가상환경을 사용하고, 저장되는 위치는 같아서... 구별할 수 없었다.
> 그래서 `Dockerfile`을 만들 때 사용하는 `requirements.txt`를 생성할 때만 유용하게 쓸 것 같다.

## 지원하는 기능

1. requirements.txt 생성

```shell
pipenv lock -r > requirements.txt
```

아마 많은 분들이 `requirements.txt`로 파이썬 버전을 관리하지 않을까.  
`pipenv`는 편리하게 export하도록 돕는다. 만들어진 `requirements.txt`의 내용 안에는 dev package는 제외되어 있다. (이 기능이 참 좋다.)

2. 가상환경 관리

`pipenv`는 가상환경의 이름을 shell이 만들어진 디렉토리(예시의 경우 `pipenv-prac`)의 위치를 기반으로 해시값을 만든다. 만들어진 디렉토리의 이름은 아래와 같다.

```shell
# similar as rm -rf ~/.local/share/virtualenvs/pipenv-prac-4h9ymY65
pipenv --rm
```

## Cons

`pipenv`는 아직 공식 관리자가 아니다. 그렇기 때문에 사용하면서 오류가 발생할 수 있다는 점은 인지해야 한다. (사실 에러는 다 발생한다.)

---

## References

> [가장 보통의 파이썬 개발 환경 (Pyenv + Pipenv + Black + Flit)](https://jonnung.dev/python/2019/11/23/ordinary-python-development-environment/)

> Official page [Pipenv: Python Dev Workflow for Humans](https://pipenv.pypa.io/en/latest/)

> Official [repositary](https://github.com/pypa/pipenv) on github

> [Pipenv & Virtual Environments](https://docs.python-guide.org/dev/virtualenvs/)

> [How to fix locking failed in pipenv?](https://stackoverflow.com/questions/64124931/how-to-fix-locking-failed-in-pipenv)
>
> > [GProst](https://stackoverflow.com/users/6250385/gprost) said "Try to remove `Pipefile.lock` before installing a package" totally work properly

### Related Issues

> [Generate requirements.txt from Pipfile.lock](https://github.com/pypa/pipenv/issues/3493)

> [Allow creating requirements.txt files without dev-packages](https://github.com/pypa/pipenv/issues/942)

> [Advanced Usage of Pipenv](https://github.com/pypa/pipenv/blob/master/docs/advanced.rst)
>
> > You can check manage `pipenv` install source url. I think most of cases does not needed.
