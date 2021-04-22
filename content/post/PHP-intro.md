---
title: "PHP Intro"
date: 2021-04-04T21:03:11+09:00
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
# PHP intro

&nbsp;

- 서버에서 실행된다.
- 파일 내 text, HTML, CSS, Javascript와 PHP코드를 포함한다
- 확장자명 `.php`를 사용한다. 
- 동적 페이지 컨텐츠를 생성한다.
- 서버에서 파일을 create, open, read, write, delete, close 할 수 있다.
- 쿠키를 보내고 받을 수 있다.
- DB의 데이터를 crud할 수 있다.
- 데이터 암호화가 가능하다
- HTML뿐 아니라 이미지, PDF, Flash영상, XML이나 XHTML까지 ouput가능하다.
-  `<?php` 로 시작하고 `?>`로 끝난다.

```php

//PHP 키워드는 case-sensitive하지 않다
<?php
//PHP code
echo "My first PHP script!";
ECHO "My first PHP script!";
EcHo "My first PHP script!";
?>

//그러나 변수명들은 case-sensitive하다.
<?php
$color = "red";
// $color와 $COLOR는 같지 않음
?>

#주석은 //와 #로 single-line comment 작성이 가능하다.

/*
multi-line comment는 이렇게 작성된다.
*/
<?php
//코드 사이에도 주석이 아래처럼 추가될 수 있다.
$x = 5 /* + 15 */ + 5;
echo $x;
?>


```

&nbsp;

-----


### echo and print

output을 내는 방법으론 `echo`와 `print` 두 가지가 있다.

- 차이점
  - `echo` 
    - return value가 없다
    - 매개변수를 여러개 받을 수 있다. (흔하게 사용되지는 않음)
    - 속도가 약간 더 빠르다
    - 괄호를 사용해도 되고, 사용하지 않아도 된다. ex.`echo`, `echo()`
    ```php
    <?php
    echo "<h2>PHP is Fun!</h2>";
    echo "Hello world!<br>";
    echo "I'm about to learn PHP!<br>";
    echo "This ", "string ", "was ", "made ", "with multiple parameters.";
    ?> 
    ```
  - `print` 
    - 1을 리턴하기 대문에 expression에 사용될 수 있다.
    - 1개의 매개변수만 받을 수 있다
    ```php
    <?php
    print "<h2>PHP is Fun!</h2>";
    print "Hello world!<br>";
    print "I'm about to learn PHP!";
    ?>
    ```

&nbsp;

-----


### PHP 7

- 기존 구 버전 (주로 PHP 5.6)에 비해 훨씬 빠르다
- error handling적인 면에서 발전되었다.
- 좀더 strict한 type declaration을 지원한다.
- 신규 operation을 지원한다. ex.`<=>`