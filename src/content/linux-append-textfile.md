---
layout: post
title: 'Linux append text files'
author: [Dan]
tags: ['linux']
image: img/default-space-unsplash.jpeg
date: '2021-05-05'
draft: false
excerpt: linux bash에서 텍스트 파일을 합치는 명령어
---

아래의 코드를 만들기 위해서는 2개의 기능이 필요하다.

1. 문자 이어 붙이기
   1. [https://stackoverflow.com/questions/4969641/how-to-append-one-file-to-another-in-linux-from-the-shell](https://stackoverflow.com/questions/4969641/how-to-append-one-file-to-another-in-linux-from-the-shell)
2. 여러개의 파일 선택하기
   1. use wildcard
   2. [https://unix.stackexchange.com/questions/122605/how-do-i-copy-multiple-files-by-wildcard](https://unix.stackexchange.com/questions/122605/how-do-i-copy-multiple-files-by-wildcard)

```bash
for file in file*;
do cat "$file" >>  "./result.txt";
done

# chmod +x ./<file_name>.sh
```

하지만 더 쉬운 방법이 있다.

```bash
ls [파일명패턴] | xargs cat > [결과파일명]
```

쉘 명령어로 이거 써도 되더라

전체 파일 cat으로 보내는걸 파일에 저장하는거래

물어보고 찾은거라

혹시 간단한 방법이 있는줄 알았지😋

written by keychron
