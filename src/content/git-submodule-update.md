---
layout: post
title: 'git update submodule once'
author: [Dan]
tags: ['git']
image: img/welcome-to-ghost.jpg
date: '2021-05-12'
draft: false
excerpt: 한번에 서브모듈을 업데이트하기
---

## pull all submodule

[https://pinedance.github.io/blog/2019/05/28/Git-Submodule](https://pinedance.github.io/blog/2019/05/28/Git-Submodule)

```bash
# in main project root folder
# git local config에 submodule을 인지시킴
# 명령 전후로 'git config --list --local'를 확인해 보자
git submodule init

# clone submodules
git submodule update

# checkout master each sub project ... (*)
# pull all submodule from master branch
git submodule foreach git checkout master
```

> [https://stackoverflow.com/questions/10168449/git-update-submodules-recursively](https://stackoverflow.com/questions/10168449/git-update-submodules-recursively)

```bash
git submodule update --recursive --remote --merge
```
