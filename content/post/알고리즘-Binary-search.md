---
title: "알고리즘 Binary Search"
date: 2020-09-27T20:39:54+09:00
categories:
- algorithm
- 이론
tags:
- 알고리즘
- 이진탐색
- 독서노트
keywords:
- 독서노트
#thumbnailImage: //example.com/image.jpg
---

<!--more-->
# Binary Search  

&nbsp;


###  이진탐색 (Binary Search)

입력값으로는 정렬된 원소리스트를 받고, 이진 탐색 알고리즘에서 원하는 원소를 찾을시 해당 원소의 위치를 반환하고, 아니면 null값을 반환할 경우.   

가능한 가장 적은 횟수의 추측으로 이 숫자를 알아내고자 한다.   

&nbsp;

ex> 1~99사이에서 57을 찾을 시

- 단순탐색(simple search) : 순서대로 탐색   
`1` &#10140; `2` &#10140; `3` &#10140; ... &#10140; `57`   
57번 진행



- 이진탐색 : 매번 남은 숫자 중 가운데 숫자를 선택하고, 그보다 큰 숫자 혹은 작은 숫자들을 한꺼번에 없앨 수 있다.   
`X` > `50`   
&#10140; `X` < `75`   
&#10140; `X` < `63`   
&#10140; `X` = `57`   
4번 진행

&nbsp;

-----

&nbsp;

1. n개의 원소를 가진 리스트에서 이진 탐색을 사용하면 최대 log<sub>2</sub>(n)번 만에 답을 찾을 수 있다.
2. 리스트의 원소들이 정렬되어 있어야만 사용할 수 있다.

&nbsp;

-----

```python

def binary_search(list, item) :
    #탐색을 하면서 현재 배열 중 어느 부분을 탐색해야 하는지를 기억해 놓아야한다.
    #처음에는 배열 전체를 탐색해야 한다.
    low = 0;
    high = len(list) -1

    while low <=high :
        #가장 가운데 있는 원소 확인
        mid = (low + high) / 2 #파이썬 3.x 이상에선 //2를 사용
        guess = list[mid]

        if guess == item :
            return mid

        #추측한 값이 너무 크면 high값을 아래와 같이 변경
        if guess < item :
            high = mid - 1

        #추측한 값이 너무 작으면 low값을 아래와 같이 변경
        if guess < item :
            low = mid + 1
    return None #아이템이 리스트에 없는 경우

```

- 실행시간   
이진 탐색 사용시 로그시간(logarithmic time)으로 실행된다. O(log n)   
단순탐색시 선형시간(linear time) - O(n)   

&nbsp;

-----


&nbsp;

###### reference
[도서 : Hello Coding 그림으로 개념을 이해하는 알고리즘](https://book.naver.com/bookdb/book_detail.nhn?bid=11823284)


-----