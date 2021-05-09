---
layout: post
title: 'Gatsby Cloud setup'
author: [Dan]
tags: ['gatsby']
image: img/welcome-to-ghost.jpg
date: '2021-05-09'
draft: false
excerpt: 개츠비 클라우드를 이용해서 블로그를 생성해보자.
---

> [https://www.gatsbyjs.com/products/cloud/](https://www.gatsbyjs.com/products/cloud/)

---

개츠비로 블로그를 새로 만들어서 사용하려고 했는데, 이전에는 안보였던 개츠비 클라우드라는 기능이 보였다.
빌드와 호스팅을 모두 해주는 서비스인데
빌드는 로컬에서 해도 되고, 호스팅도 깃헙을 통해 해도 괜찮다.
(깃헙에서 호스팅을 할 수느 있지만, 내가 정한 이름이 아니라 내 계정명을 도메인으로 사용해야되는게 좀 별로다.)
하지만 1개의 웹사이트 호스팅에 대해서는 무료기에 사용해보기로 했다.
(1개 호스팅, 1개 빌드)
![Build Plan](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/994093a2-a9ef-47df-8707-01d68053cf19/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T022818Z&X-Amz-Expires=86400&X-Amz-Signature=f3a7b4e0ba8ee17e84ce4884b374a4ef0f745316654ad70d62300a04cc7e1816&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
![Hosting Plan](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b6d39020-c6e3-4521-a220-2d58fcac8006/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T145924Z&X-Amz-Expires=86400&X-Amz-Signature=c17404c0ddff1896f868c783b2bdebd632f5b97e18051adfeb47bf224a34d1b1&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

블로그 운영을 위해서 사용할 것이기 때문에 무료 버전으로도 충분하다고 생각했다.
그러면 개츠비 클라우드를 통한 블로그 생성을 시작해보자.

---

## 블로그 생성

![Add a site](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/cb87e603-6cf4-4cb1-941a-ae33a241b08c/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T150132Z&X-Amz-Expires=86400&X-Amz-Signature=76eecfa423463236730009b9704e7417858e5a1edb77efb627d3b202e97eac4a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
난 기존에 개츠비로 만든 블로그가 없었기 때문에, 템플릿에서 시작했다.

click `Start from a Template` ![Select Template](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e48c15ec-6758-4f8b-8911-745e754250d2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T150214Z&X-Amz-Expires=86400&X-Amz-Signature=4cbb964fd1f1b958d2fc4880d94e3a2063d1025fd2c806ff9f7dbe3bcf6212a4&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
여러 스타터 템플릿 중 나는 `Minimal Blog` 를 선택했다.
괜찮은 템플릿인지 아닌지는 잘모르고 깔끔해서 골랐는데, 괜찮다고 생각한다.
아직도 개츠비에 대해 잘모르는게 많은데, 이 블로그는 많은 커스터마이징을 `shadowing` 을 통해 지원한다.
what is [shadowing in gatsby?](https://www.gatsbyjs.com/docs/how-to/plugins-and-themes/shadowing/)
또한 콘텐츠는 `mdx` 를 통해 제작할 수 있도록 지원하고, 그 외에도 기본적인 기능을 여러 제공하는데 자세한 내용은 [템플릿 저장소](https://github.com/LekoArts/gatsby-starter-minimal-blog)에서 확인해보자.
이 다음 과정에서 난 이미 블로그를 생성하면서 깃헙의 권한을 개츠비에게 줬다. 모든 권한을 다 주어야 개츠비가 저장소를 생성할 수 있다.
![Create new site](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8f276df8-9bcf-45c3-8321-ebfd7bc03e48/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T150248Z&X-Amz-Expires=86400&X-Amz-Signature=267ec3ef5ec896de21ee039714b77fcfb1ceeca830ef0ec2c2ad76c0b6cb8a86&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
선택했으면 이제 저장소의 이름을 작성하자. 여기서 작성된 이름으로 도메인이 작성된다.
이 이름은 바꿀 수 없는 것이 아니다. (저장소 이름은 못바꾼다.)
도메인 이름은 추후에 설정에서 바꿀 수 있다.

```plain
https://<project_name>.gatsbyjs.io
```

![Success](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8180f838-c7bf-4c96-82a7-9151490dd4d4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T150312Z&X-Amz-Expires=86400&X-Amz-Signature=bdb084c3144754bfd2f881b7877d6a4e335315e87496ae559955db1da3dc936a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
고생했다. 이제 당신의 블로그의 생성됐다.

---

## 블로그 상세 정보

이제 대시보드에서 생성한 블로그를 확인할 수 있다.
![List on cloud](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/caee9994-b860-4fc0-aa42-2e79449c93b4/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T150334Z&X-Amz-Expires=86400&X-Amz-Signature=c2db4096c67d1774fcc6cca9734b9e5bf0f44fce906e7cae9a92e7f79ee845e9&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

> 난 처음에 블로그의 이름 뒤에 개츠비가 붙는줄 모르고 생성했다가 나중에 이름을 바꿨다..

### 수동 빌드

만든 블로그의 디테일을 확인하기 위해 클릭하여 들어가보자.
![Trigger build](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/95a4affb-349b-45ed-9058-8ade01585a85/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T150407Z&X-Amz-Expires=86400&X-Amz-Signature=a04b25dffb2b128d7d7b2c7678d05c46cc3d08c3eafaf18634f2933910958ffa&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
`Build` , `Site Settings` 가 보인다. 이름만 봐도 뭘 하는지 알 수 있는 이름들이다.
오른쪽의 보라색 `Trigger build` 버튼을 통해 수동으로 빌드할 수 있다.

### 자동 빌드

이제 생성된 github 혹은 gitlab 저장소로 가보자. (아래의 예시는 github)
![webhook](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b11bc981-46b9-4173-b4f1-9ba1716f222e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T150425Z&X-Amz-Expires=86400&X-Amz-Signature=8fa35da8bf19e0a21a5385188d9c4dceb5aa2ac6dcd1e6f98526ea1af3490d50&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
`Settings` → `Webhooks` 가보면 `push` 에 대한 웹훅이 생성된 것을 확인할 수 있을 것이다.
코드를 작성해서 `push` 만 하면 자동으로 빌드가 되게 설정되어 있는 것이다.
(이 부분에 대해서 `main` 브랜치만 업데이트되도록 설정된 것인지 잘모르겠다. `edit` 버튼을 누르고 들어가보면 브랜치 정보가 없다. 그런데 이 저장소는 `main` 브랜치가 하나이기도 하다.)

---

### google analytic id

위의 과정에서 `GOOGLE_ANALYTICS_ID` 를 입력하는 과정을 생략했다.
만약 자신이 google analytics를 블로그에 추가했다면 (기본 설정이 analytics 활성화인지는 잘모르겠다.) ID를 입력하지 않아서 빌드가 실패했을 수도 있다.
위에서 언급된 `Build Settings` 를 눌러 이동해보자.
![env id](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/00d0b597-b80e-4e45-9e54-4277f60ae073/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T150450Z&X-Amz-Expires=86400&X-Amz-Signature=43ce22cf65809118ada7774361ebd3d2d65afc077543867b8bf3b24078186c97&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
이동한 경우 `General` 로 이동해있을 것이다. 아래로 조금만 스크롤 다운하면, `Environment variables` 가 보인다. 여기에 `GOOGLE_ANALYTICS_ID` 가 비어있을 것이다. (이 id를 얻는 방법은 구글에 찾아보면 잘설명한 글이 많으니 참조하길 바란다.) 추가하고 수동으로 빌드하여 빌드가 성공했는지 확인하기 바란다.

### 추가 웹훅

조금 더 스크롤을 내려보면 `Outgoing Notifications` 이 보인다.
![Notification](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/fda25c54-70f4-4ffd-8ef8-d4a97bbdc5c1/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T150517Z&X-Amz-Expires=86400&X-Amz-Signature=50b855b2b480dd2172159eabc01ed1492265d049b0dee6a88aa83f5c274e6ca8&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
아마 이 2개의 웹훅이 표시되어 있을 것이다.
`Add Notification` 을 선택해 추가할 수 있다.
![Webhook list](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/ce48c2e1-65ec-4fa0-a957-21e7b74e194f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210509%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210509T150714Z&X-Amz-Expires=86400&X-Amz-Signature=17b3995bc3a57d182102a441e0bc231dec6d1fa64262685832014232dc4a6ba8&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
나의 경우 `Slack` 을 추가하여, 슬랙 채널에서 빌드,배포 실패 혹은 배포 성공시 알람이 울리도록 설정했다.
