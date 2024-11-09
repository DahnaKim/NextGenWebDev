---  
layout: post  
title:  "SQL Statements"  
categories: [ PostgreSQL ]  
image: assets/img/20241105_03.png  
tags: [featured]  
---  
    
# SQL Statements  
  
### Challenges of Postgres  
- 데이터베이스에서 정보를 검색하기 위한 효율적인 쿼리를 작성하는 방법  
- 데이터베이스의 스키마나 구조를 어떻게 디자인할 것인가  
- 고급 기능들을 언제 사용할지 이해하기  
- 프로덕션 환경에서 데이터베이스 관리하기   

  
  
### Database Design Process  
- 저장하는 것이 무엇인가?  
- 저장하는 것은 어떤 속성을 가졌는가?  
- 데이터 타입은 무엇인가?  
  
### Calculated Columns  
![1]({{ site.baseurl }}/assets/img/20241105_01.png)  
  
### String Operators and Functions   
- `||` : 문자열 연결 연산자. 'Hello' || ' World'는 'Hello World'  
- `LENGTH()` : 문자열의 길이를 반환. 예를 들어, LENGTH('Hello')는 5를 반환  
- `CONCAT()` : 여러 개의 문자열을 연결 CONCAT('Hello', ' ', 'World')는 'Hello World'를 반환  
- `UPPER()` : 문자열 모두 대문자로 변환  
- `LOWER()` : 문자열 모두 소문자로 변환  
- `WHERE` : 특정 조건을 만족하는 데이터만 선택할 때 사용  
  
```  
  
SELECT name, area FROM cities WHERE area > 4000;  
  
```  
  
### 쿼리 실행 순서  
![2]({{ site.baseurl }}/assets/img/20241105_02.png)  
  
### Comparison Math Operators  
- `=` : 두 값이 같은지 비교합니다. 예를 들어, age = 30은 age가 30인 경우에만 참(TRUE)입니다.  
- `<=` : 왼쪽 값이 오른쪽 값보다 작거나 같은지 비교합니다. 예를 들어, age <= 30은 age가 30 이하일 때 참입니다.  
- `>` : 왼쪽 값이 오른쪽 값보다 큰지 비교합니다. 예를 들어, age > 30은 age가 30보다 클 때 참입니다.  
- `<` : 왼쪽 값이 오른쪽 값보다 작은지 비교합니다. 예를 들어, age < 30은 age가 30보다 작을 때 참입니다.  
- `<>` : 두 값이 다른지 비교합니다. 예를 들어, age <> 30은 age가 30이 아닐 때 참입니다. (SQL 표준에서 지원되는 != 와 같은 기능입니다.)  
- `!=` : 두 값이 다른지 비교합니다. 예를 들어, age != 30은 age가 30이 아닐 때 참입니다. (<>와 같은 기능입니다.)  
- `>=` : 왼쪽 값이 오른쪽 값보다 크거나 같은지 비교합니다. 예를 들어, age >= 30은 age가 30 이상일 때 참입니다.  
- `BETWEEN` : 지정한 두 값 사이에 있는지 확인합니다. 예를 들어, age BETWEEN 20 AND 30은 age가 20 이상 30 이하일 때 참입니다.  
`IN` : 특정 값들이 리스트 내에 있는지 확인합니다. 예를 들어, age IN (25, 30, 35)는 age가 25, 30 또는 35일 때 참입니다.  
`NOT IN  : 특정 값들이 리스트 내에 없는지 확인합니다. 예를 들어, age NOT IN (25, 30, 35)는 age가 25, 30, 35가 아닐 때 참입니다.  
  
### Inserting Data Into a Table  
```  
INSERT INTO cities(name, country, population, area)  
VALUES('Tokyo', 'Japan', 38505000, 8223);  
```  
  
###  Updating Rows  
`UPDATE cities SET population = 39505000 WHERE name = 'Tokyo'`;  
  
### Deleting Rows  
`DELETE FROM cities WHERE name = 'Tokyo'`;  
  
  
  
  
  