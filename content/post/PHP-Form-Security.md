---
title: "PHP Form Security"
date: 2021-05-10T21:11:18+09:00
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
# PHP Form Security

&nbsp;

### 보안문제

```php
<form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">
```

 `$_SERVER["PHP_SELF"]` 변수가 페이지내에서 사용 된 경우 `/`를 입력하고 `XSS (Cross-site scripting)` 커멘드를 execute할 수 있게 된다.

 - `XSS` : 해커들이 클라이언트사이드 스크립트를 웹페이지에 inject해 다른 유저들에게도 보이게 할 수 있게한다. 

- 예시

```php

<form method="post" action="<?php echo $_SERVER["PHP_SELF"];?>"/>

/*정상동작 예시
입력 URL : http://www.example.com/test_form.php 인 경우 
*/ 

<form method="post" action="test_form.php">



/*XSS 예시
입력 URL : http://www.example.com/test_form.php/%22%3E%3Cscript%3Ealert('hacked')%3C/script%3E
*/

<form method="post" action="test_form.php/"><script>alert('hacked')</script>

```

2번째 예시대로라면 페이지 로드시 자바스크립트가 동작할 것이다.
**`<script>`태그에는 모든 자바스크립트 코드가 추가될 수 있다는 점을 명심해야한다.** 해커가 얼마든지 페이지를 이동시키거나, 글로벌 변수를 조작할 malicious code를 가진 파일로 유도하거나, 사용자 정보를 다른 url로 submit되게 할 수도 있다.

&nbsp;

### 해결방안

위 문제는 `htmlspecialchars()`함수를 활용하여 피할 수 있다.   
특수문자를 HTML entitiy로 변경시켜주어 아래처럼 적용되게 해준다.

```php
<form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">


<form method="post" action="test_form.php/&quot;&gt;&lt;script&gt;alert('hacked')&lt;/script&gt;">
```

&nbsp;

-----

### Validation sample

1. 사용자 input데이터에서 불필요한 문자 정리
2. 사용자 input데이터에서 백스래쉬 제거

&nbsp;

```php
<?php
// define variables and set to empty values
$name = $email = $gender = $comment = $website = "";

//check whether the form has been submitted using $_SERVER["REQUEST_METHOD"]
if ($_SERVER["REQUEST_METHOD"] == "POST") {
  $name = test_input($_POST["name"]);
  $email = test_input($_POST["email"]);
  $website = test_input($_POST["website"]);
  $comment = test_input($_POST["comment"]);
  $gender = test_input($_POST["gender"]);
}

function test_input($data) {
  $data = trim($data);
  $data = stripslashes($data);
  $data = htmlspecialchars($data);
  return $data;
}
?>
```

- 필수값 체크
```php
<?php
// define variables and set to empty values
$nameErr = $emailErr = $genderErr = $websiteErr = "";
$name = $email = $gender = $comment = $website = "";

if ($_SERVER["REQUEST_METHOD"] == "POST") {
  if (empty($_POST["name"])) {
    $nameErr = "Name is required";
  } else {
    $name = test_input($_POST["name"]);
  }

  if (empty($_POST["email"])) {
    $emailErr = "Email is required";
  } else {
    $email = test_input($_POST["email"]);
  }

  if (empty($_POST["website"])) {
    $website = "";
  } else {
    $website = test_input($_POST["website"]);
  }

  if (empty($_POST["comment"])) {
    $comment = "";
  } else {
    $comment = test_input($_POST["comment"]);
  }

  if (empty($_POST["gender"])) {
    $genderErr = "Gender is required";
  } else {
    $gender = test_input($_POST["gender"]);
  }
}
?>


<form method="post" action="<?php echo htmlspecialchars($_SERVER["PHP_SELF"]);?>">

Name: <input type="text" name="name">
<span class="error">* <?php echo $nameErr;?></span> //오류메시지를 출력해준다.
<br><br>
E-mail:
<input type="text" name="email">
<span class="error">* <?php echo $emailErr;?></span>
<br><br>
Website:
<input type="text" name="website">
<span class="error"><?php echo $websiteErr;?></span>
<br><br>
Comment: <textarea name="comment" rows="5" cols="40"></textarea>
<br><br>
Gender:
<input type="radio" name="gender" value="female">Female
<input type="radio" name="gender" value="male">Male
<input type="radio" name="gender" value="other">Other
<span class="error">* <?php echo $genderErr;?></span>
<br><br>
<input type="submit" name="submit" value="Submit">

</form>
```

&nbsp;

-----