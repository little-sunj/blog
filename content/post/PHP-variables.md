---
title: "PHP Variables"
date: 2021-04-13T22:12:21+09:00
categories:
- language
- PHP
tags:
- PHP
- 독학노트
keywords:
- 독학노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# PHP Variables

&nbsp;

- Loosely Typed Language
  - PHP 7부터 변수 자료형 선언 기능이 추가되었다.

&nbsp;

-----

### SCOPE

3가지 종류의 변수 스코프가 존재한다.

- local
  - 함수 안에서 선언된 변수는 함수 내에서만 사용할 수 있다.
  - 다른 함수에서 동일한 이름으로 변수가 선언 될 수 있다.
  ```php
    <?php
    function myTest() {
    $x = 5; // local scope
    echo "<p>Variable x inside function is: $x</p>";
    }
    myTest();

    // using x outside the function will generate an error
    echo "<p>Variable x outside function is: $x</p>";
    ?>
  ```
- global
  - 함수 밖에 선언된 변수는 함수 밖에서만 사용할 수 있다.
  ```php
    <?php
    $x = 5; // global scope

    function myTest() {
    // using x inside this function will generate an error
    echo "<p>Variable x inside function is: $x</p>";
    }
    myTest();

    echo "<p>Variable x outside function is: $x</p>";
    ?>
  ```
  - 함수 내에서 글로벌 변수에 접근하고자 한다면 `global`키워드를 사용한다.
  ```php
    <?php
    $x = 5;
    $y = 10;

    function myTest() {
    global $x, $y;
    $y = $x + $y;
    }

    myTest();
    echo $y; // outputs 15
    ?>
  ```
  - php는 모든 글로벌 변수를 `$GLOBALS[index]`라는 배열에 저장한다. (`index`는 변수명) 함수 내에서 해당 배열에 접근이 가능하다.
  ```php
    <?php
    $x = 5;
    $y = 10;

    function myTest() {
    $GLOBALS['y'] = $GLOBALS['x'] + $GLOBALS['y'];
    }

    myTest();
    echo $y; // outputs 15
    ?>
  ```
- static
  - 보통 함수가 실행완료되면 해당 함수 내 모든 변수는 삭제된다. 만일 해당 변수가 유지되길 바라는 경우 `static`키워드를 사용할 수 있다.
  ```php
    <?php
    function myTest() {
    static $x = 0;
    echo $x;
    $x++;
    }

    myTest();
    myTest();
    myTest();
    ?>
    //매번 함수가 호출 될 때마다 이전 함수에서 저장한 변수 값이 유지된다.
  ```

&nbsp;

-----