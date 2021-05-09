---
title: "PHP SuperGlobals"
date: 2021-05-09T22:00:08+09:00
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
# PHP Superglobals

&nbsp;

superglobals :  함수, 클래스, 파일, 스코프와 관계 없이 언제나 접근 가능한 PHP built-in변수.

- `$GLOBALS` : 모든 글로벌 변수를 담는 배열
    ```php
    <?php
    $x = 75;
    $y = 25;
    
    function addition() {
    $GLOBALS['z'] = $GLOBALS['x'] + $GLOBALS['y'];
    }
    
    addition();
    echo $z;
    ?>
    ```
- `$_SERVER` : header, paths, script location정보를 담는 배열
    ```php
    <?php
    echo $_SERVER['PHP_SELF'];
    echo $_SERVER['SERVER_NAME'];
    echo $_SERVER['HTTP_HOST'];
    echo $_SERVER['HTTP_REFERER'];
    echo $_SERVER['HTTP_USER_AGENT'];
    echo $_SERVER['SCRIPT_NAME'];
    ?>
    //https://www.w3schools.com/php/php_superglobals_server.asp
    ```
- `$_REQUEST` : HTML폼 submit이후 데이터를 collect
    ```php
    <form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
        Name: <input type="text" name="fname">
    <input type="submit">
    </form>

    <?php
        if ($_SERVER["REQUEST_METHOD"] == "POST") {
            // collect value of input field
            $name = $_REQUEST['fname'];
            if (empty($name)) {
                echo "Name is empty";
            } else {
                echo $name;
            }
        }
    ?>
    ```
- `$_POST` : HTML폼 submit이후 데이터를 collect (POST방식)
    ```php
    <form method="post" action="<?php echo $_SERVER['PHP_SELF'];?>">
        Name: <input type="text" name="fname">
    <input type="submit">
    </form>

    <?php
        if ($_SERVER["REQUEST_METHOD"] == "POST") {
            // collect value of input field
            $name = $_POST['fname'];
            if (empty($name)) {
                echo "Name is empty";
            } else {
                echo $name;
            }
        }
    ?>
    ```
- `$_GET` : HTML폼 submit이후 데이터를 collect (GET방식). URL로 받는 정보도 가져올 수 있다
    ```php
        <a href="test_get.php?subject=PHP&web=W3schools.com">Test $GET</a>

        <?php
        echo "Study " . $_GET['subject'] . " at " . $_GET['web'];
        ?>
    ```
- `$_FILES`
- `$_ENV`
- `$_COOKIE`
- `$_SESSION`


&nbsp;

-----