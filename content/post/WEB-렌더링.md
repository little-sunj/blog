---
title: "WEB 렌더링"
date: 2021-02-10T09:55:03+09:00
categories:
- web
tags:
- web
keywords:
- WEB
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# 렌더링

&nbsp;

### static sites
서버에 이미 배포되어 있는 HTML문서를 가져와서 보여주는 형식. 페이지 내 링크 클릭시 다시 서버에서 해당 page HTML을 받아와서 페이지 전체가 업데이트 되어야한다.

`iframe` 태그를 이용해 부분적인 HTML만 받아와서 업데이트 하기도 하였다.

&nbsp;

-----

### SPA (single page application)
`XMLHttpRequest`가 개발되면서 서버에서 가볍게 json데이터로 받아올 수 있게 된다. (AJAX) 사용자가 한 화면에 머물면서 필요한 데이터를 서버에서 받아와 부분적으로 업데이트한다.

&nbsp;

-----


### CSR (client side rendering)
서버에서 `index.html`파일을 클라이언트에 보내준다. 해당 index파일엔 어플리케이션에서 필요한 자바스크립트의 링크만 담겨있다. 처음 접속시 빈 화면만 보이며, 다시 서버로부터 필요한 자바스크립트를 내려받게 된다. 


```
TTV (time to view) : 사용자가 웹 사이트를 볼 수 있게 되는 시점
TTI (time to interact) : 사용자가 웹 사이트에서 클릭을 하거나 인터렉션이 가능해지는 시점.

웹 사이트의 성능을 분석할 때 중요한 매트릭으로 사용할 수 있다.
```

&nbsp;

**SCR은 TTV와 TTI가 일치한다.**   

`클라이언트 접속` &#10140; `서버에게서 index파일을 내려받는다.` &#10140; `index에 담긴 경로에 있는 로직 자바스크립트 요청` &#10140; `동적으로 HTML 생성하는 웹 어플리케이션 로직 스크립트를 내려받는다.` &#10140; `웹 사이트를 사용할 수 있게 된다.`


아래와 같은 단점이 있다.
- Initial loading이 오래 걸린다.
- Low SEO (search engine optimization) 
    - web crawlers가 html파일을 분석하려해도 초기 `index.html`파일내 내용이 없어서 검색엔진들이 웹페이지 분석에 어려움을 겪는다.

성능을 개선한다면 최종적으로 번들링해서 사용자에게 보내주는 스크립트 파일을 효율적으로 많이 분할해서 첫 번째로 사용자가 보기 위해 필요한 필수 파일만 보내줄 수 있는지를 고민하게 된다.


&nbsp;

-----


### SSR (server side rendering)
웹사이트 접속시 서버에서 필요한 데이터를 모두 가져와서 HTML을 만들고 동적으로 제어 가능한 소스와 함께 클라이언트에 제공한다.

&nbsp;

**SCR은 TTV와 TTI가 일치하지 않는다.**   

`클라이언트 접속` &#10140; `서버로부터 컨텐츠가 포함된 index파일을 내려받는다` &#10140; `사용자가 웹 사이트를 볼 수 있다`  &#10140; `자바스크립트 파일을 서버에 요청한다.`  &#10140; `동적제어를 위한 스크립트를 내려받는다.` &#10140; `웹 사이트 인터렉션이 가능해진다.`

아래와 같은 장점이 있다.
- CSR보다 Initial 로딩속도가 빠르다.
- html문서 내 컨텐츠가 담겨있기 때문에 검색엔진이 웹페이지 분석도 할 수 있게 된다.

아래와 같은 단점이 있다.
- Blinking Issue, None-rich site interactions. 
  - static site에서도 존재하던 깜박임 이슈 (웹페이지 전체 업데이트)
- Server side overhead
  - 서버에 과부하가 걸리기 쉽다. (특히 사용자가 많은 제품일수록)
- Need to wait before interacting
  - 동적으로 제어를 가능하게 해줄 스크립트가 로드 될 때까지 기다려야한다.
  - TTV와 TTI 사이의 공백기간이 꽤 길다

성능을 개선한다면 TTV와 TTI사이 시간 단차를 줄이기 위한 고민을 하게된다.

&nbsp;

-----


### SSG (server side generation)
생성한 웹 어플리케이션을 static site generator를 통해 서버에 미리 배포해둔다. (react나 gatsby를 같이 사용한다거나..) 자바스크립트 파일을 함께 가지고 있을 수 있기 때문에 동적인 요소도 충분히 추가 할 수 있다. (`Next.js`등의 서버사이드 렌더링 라이브러리는 요즘 ssg도 지원하고 있다.) 상황에 따라 CSR과 SSR을 잘 섞어서 유연하게 구성할 수 있다.



&nbsp;

-----

###### reference
[공부자료](https://www.youtube.com/watch?v=iZ9csAfU5Os)




-----

