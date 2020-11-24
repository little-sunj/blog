---
title: "WEB DOM동작원리"
date: 2020-11-10T20:21:43+09:00
categories:
- WEB
tags:
- web
keywords:
- WEB
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# DOM 동작원리와 이해

&nbsp;


### 브라우저 구조

![external-content duckduckgo](https://user-images.githubusercontent.com/28701069/98668387-2aca5580-2393-11eb-9d0c-6c91a46401f4.png)


&nbsp;

1. **사용자 인터페이스**
	- 사용자 컨트롤 영역
2. **브라우저 엔진**
	- 사용자 인터페이스와 렌더링 엔진 사이 동작 제어
3. **렌더링 엔진**
	- 요청한 URI를 브라우저에서 받아 콘텐츠를 표시해준다. HTML을 요청하면 HTML과 CSS를 파싱해 창에 보여준다
4. **통신**
	- HTTP요청과 같은 네트워크 처리를 담당하는 부분. 독립 인터페이스라 각 플랫폼 하부에서 (OS)실행된다
5. **자바스크립트 해석기**
	- 자바스크립트를 해석/실행하는 부분
6. **UI백엔드**
	- 렌더링 엔진이 브라우저에 페이지를 render할때 render tree로 구성된 노드를 가로지르며 그려주는 역할을 한다.
7. **자료저장소**
	- 자료를 저장하는 계층이다. 쿠키 등의 자료가 들어가있으며, PC 하드디스크에 저장된다. HTML5에서는 더 쉽게 사용할 수 있도록 **웹 데이터베이스**를 지원한다.

&nbsp;

-----

&nbsp;

## 브라우저 렌더링 방법 

1. HTML을 파싱해 DOM객체를 생성
2. CSS를 파싱해 스타일규칙 생성
3. 두가지를 합쳐 웹 브라우저에 보여져야할 요소를 표현한 'webkit : render tree/gecko : frame tree' 생성(구축) (webkit에선 attatchment, gecko에선 shape building이라 표현한다.)
4. render tree/frame tree를 기준으로 레이아웃 배치(이를 webkit엔진에선 layout, gecko엔진에선 reflow라고 표현한다). 노드들을 정확한 위치에 표시
5. 배치 완료시 UI 백엔드에서 노드를 가로지르며 paint

- Webkit은 HTML과 stylesheet를 처음부터 분리하여 작업한다.
- Gecko는 HTML을 최초로 호출하고 콘텐츠싱크(Contents Sync)과정을 두어 stylesheet를 분리해서 작업한다. 

&nbsp;

-----

DOM을 반복적으로 직접 조작하면 그 만큼 브라우저가 렌더링을 자주하게 되고, PC자원을 많이 소모하게 된다. (무조건 다 렌더링 다시하는게 아니고 DOM을 제어하는 메서드에 따라 다르지만 어느정도 합쳐서 배치로 작업된다 (batched DOM update))

렌더링 엔진은 좀 더 좋은 사용자 경험을 주기위해 가능한 빠르게 내용을 표시한다. (HTML파싱을 통해 DOM트리 구축이 끝날때까지 기다리지 않는다.) 3번의 DOM트리 구축이 계속 갱신되면서 2~5작업이 진행된다.
즉, 통신 레이어에서 네트워크 작업을 계속 진행하면서 아직 못받은 내용을 기다림과 동시에 받은 데이터 일부를 화면에 출력하게 된다.

&nbsp;


- reference    
https://jeong-pro.tistory.com/210
https://vallista.kr/2019/05/06/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95/
https://beomy.github.io/tech/browser/browser-rendering/


-----