---
title:  "[프로그래머스 SQL] DATETIME에서 DATE로 형 변환"

categories:
  - SQL
  
last_modified_at: 2021-04-07T18:35:00
---

[![DATETIME에서 DATE로 형 변환](https://user-images.githubusercontent.com/53072057/113819243-0349ae80-97b4-11eb-9100-5a28d0ef75f1.JPG)](https://programmers.co.kr/learn/courses/30/lessons/59414)  
<h2>[ 문제풀이 ]</h2>  
DATE_FORMAT 함수를 사용할 수 있으면 간단하게 해결할 수 있는 문제이다.  

```java
﻿DATE_FORMAT(날짜, 형식) : 날짜를 지정한 형식으로 출력
```

```java
SELECT ANIMAL_ID, NAME, DATE_FORMAT(DATETIME, '%Y-%m-%d') AS '날짜' //%m과 %M, %d와 %D는 다른 형식
FROM ANIMAL_INS
ORDER BY ANIMAL_ID ASC;
```