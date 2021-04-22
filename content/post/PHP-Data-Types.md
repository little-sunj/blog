---
title: "PHP Data Types"
date: 2021-04-22T20:07:51+09:00
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
# PHP Data Types

&nbsp;

PHP는 아래 자료구조를 지원한다.

- String
  - single quotes와 double quotes 모두 사용 가능
  - ` $x = "Hello World!"; ` ` $y = 'Hello World!'; `
- Integer
  - ` $x = 5985; `
- Float (floating point numbers - also called double)
  - ` $x = 10.365; `
- Boolean
  - ` $x = true; `, ` $y = false; `
- Array
  - ` $cars = array("Volvo","BMW","Toyota"); `
- Object
  ```php
    <?php
    class Car {
        public $color;
        public $model;
        public funtion __construct($color, $model) {
            $this -> color = $color;
            $this -> model = $model;
        }
        public function message() {
            return "My Car is a " . $this -> color . " " . $this->model . "!";
        }
    }

    $myCar = new Car("black", "Tesla");
    echo $myCar -> mesasge();
    echo "<br>";
    $myCar = new Car("red", "Toyota");
    echo $myCar -> mesage();

    >

    //output
    //My car is a black Volvo!
    //My car is a red Toyota!
  ```
- NULL
  - 변수가 초기화 없이 선언되면 자동으로 NULL값을 할당한다.
  - ` $x = null; `
- Resource
  - 특정 리소스 타입은 실제 데이터 타입은 아니다.
  - ex. database call


&nbsp;

-----

### PHP Strings

[PHP String function 레퍼런스](https://www.w3schools.com/php/php_ref_string.asp)

&nbsp;

- String length : ` strlen() `
    ```php
    <?php
    echo strlen("Hello world!"); // outputs 12
    ?>
    ```
- count number of words in a string : ` str_word_count() `
   ```php
    <?php
    echo str_word_count("Hello world!"); // outputs 2
    ?>
    ```
- reverse a string : ` strrev() `
    ```php
    <?php
    echo strrev("Hello world!"); // outputs !dlrow olleH
    ?>
    ```
- Search For a Text Within a String : ` strpos() `
    ```php
    <?php
    echo strpos("Hello world!", "world"); // outputs 6
    ?>
    ```
- Replace Text Within a String : ` str_replace() `
    ```php
    <?php
    echo str_replace("world", "Dolly", "Hello world!"); // outputs Hello Dolly!
    ?>
    ```

&nbsp;

-----

### PHP Numbers

- **Integers**
    - PHP_INT_MAX : 가장 큰 int 상수
    - PHP_INT_MIN : 가장 작은 int 상수
    - PHP_INT_SIZE : integer의 byte크기
    - integer여부를 체크하는 함수
        - `is_int()`
        - `is_integer()` - alias of is_int()
        - `is_long()` - alias of is_int()

- **Float**
    - PHP_FLOAT_MAX : 가장 큰 float 상수
    - PHP_FLOAT_MIN : 가장 작은 float 상수
    - -PHP_FLOAT_MAX : 가장 작은 float 상수 (음수)
    - PHP_FLOAT_DIG : 값 손실 없이 float로 반올림한 뒤 되돌릴 수 있는 소수 자릿수
    - PHP_FLOAT_EPSILON : 표현 가능한 가장 작은 양수 x, (x + 1.0 != 1.0)
    - float여부를 체크하는 함수
        - `is_float()`
        - `is_double()` - alias of is_float()

- **Infinity**
    - PHP_FLOAT_MAX 보다 큰 numeric값은 infinite으로 간주된다.
    - infinite여부를 체크하는 함수
        - `is_finite()`
        - `is_infinite()`

- **NaN**
    - Not a Number
    - NaN여부를 체크하는 함수
        - `is_nan()`

- **Numerical Strings**
    - 변수가 numeric값인지 체크하는 함수
        - `is_numeric()`
        - numeric값이거나 numeric형 string값인 경우에도 `true`를 반환하고, 그 외의 경우 `false`를 반환한다.
        - From PHP 7.0: The is_numeric() function will return FALSE for numeric strings in hexadecimal form (e.g. 0xf4c3b00c), as they are no longer considered as numeric strings.

- **Casting**
    - `(int)` , `(integer)` . `intval()` 를 사용해 string이나 float값을 Integer로 캐스팅할 수 있다.
    ```php
    <?php
    // Cast float to int
    $x = 23465.768;
    $int_cast = (int)$x;
    echo $int_cast;

    echo "<br>";

    // Cast string to int
    $x = "23465.768";
    $int_cast = (int)$x;
    echo $int_cast;
    ?>
    ```


&nbsp;

-----