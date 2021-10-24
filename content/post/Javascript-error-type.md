---
title: "Javascript Error Type"
date: 2021-06-29T21:19:57+09:00
categories:
- language
- Javascript
tags:
- Javascript
- 독서노트
keywords:
- 독서노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
#  Javascript Error Type 

&nbsp;



ECMA-262에서는 다음 일곱 가지 에러 타입을 정의했다.

&nbsp;

1. **Error** 
    - 브라우저가 리턴하는 일은 거의없다.
    - 주요 목적은 개발자가 커스텀 에러를 만들 수 있게 하는 것이다. 
2. **EvalError**
    - `Eval()`함수 사용중 예외 발생시.
    - eval 프로퍼티의 값이 직접적인 호출(즉 CallExpression에 포함된 MemberExpression인 Identifier를 명시적인 이름으로 사용한 호출) 외 다른 방법으로 사용되었거나 eval프로퍼티에 다른 무언가를 할당했을 때 발생.
    - eval은 함수 호출로만 사용할 수 있다는 뜻이다.
3. **RangeError**
    - 숫자가 범위를 벗어났을 때 발생.
4. **ReferenceError**
    - 존재하지 않는 변수에 접근하려 할 때 발생.
5. **SyntaxError**
    - 문법 에러.
    - SyntaxError발생시 자바스크립트 코드 실행을 즉시 멈추게 된다.
6. **TypeError**
    - 가장 많이 발생.
    - 예기치 못한 타입의 변수를 사용하거나 존재하지 않는 메서드 접근 시도시 발생.
    - 함수 매개변수 타입을 확인하지 않고 사용할 경우에도 종종 발생.
7. **URIError**
    - `encodeURI()`나 `decodeURI()`에 잘못된 URI를 넘겼을 때만 발생.

모든 에러타입은 Error타입을 상속한다. (결과적으로 에러타입은 모두 같은 프로퍼티를 공유한다.)

&nbsp;

https://tristan91.tistory.com/439

-----