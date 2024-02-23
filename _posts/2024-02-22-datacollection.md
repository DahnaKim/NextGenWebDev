---
layout: post
title:  "Handling Python-based MySQL from Data Collection to Storage"
categories: [ MySQL ]
image: assets/img/20240222_3.png
tags: [featured]
---
  
# Handling Python-based MySQL from Data Collection to Storage  
  
### 파이썬 크롤링  
![1]({{ site.baseurl }}/assets/img/20240222_1.png)  
![2]({{ site.baseurl }}/assets/img/20240222_2.png)  
  
- 데이터 전처리(Preprocessing raw data)   
데이터 분석, 머신러닝 모델링, 데이터베이스 관리 등 다양한 분야에서 필수적인 단계로 간주됨  
replace(), strip() 같은 메소드를 사용하여 데이터를 포맷팅하거나 불필요한 공백, 문자를 제거하고,
결측치를 처리하거나 데이터 타입을 변환하는 등의 작업을 포함할 수 있다.  

<br>

**Schema 구성**  
workbench를 통해, schema 구성  일회성 schema는 파이썬으로 자동으로 하기보다는 workbench를 활용하는 것이 일반적임  
  
~~~  
DROP DATABASE IF EXISTS ecommerce;  
CREATE DATABASE ecommerce;  
USE ecommerce;   
CREATE TABLE teddyproducts (  
    ID INT UNSIGNED NOT NULL AUTO_INCREMENT,  
    TITLE VARCHAR(200) NOT NULL,  
    CATEGORY VARCHAR(20) NOT NULL,  
    PRIMARY KEY(ID)  
);  
~~~

<br>

**크롤링 + mysql저장**  
![3]({{ site.baseurl }}/assets/img/20240222_3.png)  
![4]({{ site.baseurl }}/assets/img/20240222_4.png)  

  
**mysql 데이터 읽기**  
  
~~~  
# 1. 라이브러리 가져오기
import pymysql

# 2. 접속하기
db = pymysql.connect(
    host='localhost', 
    port=3306, 
    user='root', 
    passwd='1234567', 
    db='ecommerce', 
    charset='utf8')

# 3. 커서 가져오기
cursor = db.cursor()
SQL = "SELECT * FROM teddyproducts WHERE CATEGORY = '행거도어';"
cursor.execute(SQL)
rows = cursor.fetchall()
for row in rows:
    print (row[1])

# 7. DB 연결 닫기
db.close()
~~~
  
※ `row[1]` : 튜플 형태의 데이터를 타이틀만 읽을 수 있게 지정  

<br>

[참고자료] mysql_with_python_practice.ipynb

  
