---  
layout: post  
title:  "Learning Data Types, Functions, Installation, and Management Tools"  
categories: [ PostgreSQL ]  
image: assets/img/20241112_01.png  
tags: [featured]  
---  
  
## Selecting Distinct Records  
  
`DISTINCT`는 SQL에서 **중복된 값을 제거**하고 **고유한 값만을 반환**하는 데 사용되는 키워드입니다. `SELECT` 구문에서 특정 컬럼이나 전체 행에 대해 중복을 없애고자 할 때 `DISTINCT`를 사용합니다.  
  
### 기본 문법  
```sql  
SELECT DISTINCT 컬럼명1, 컬럼명2, ...  
FROM 테이블명;  
```  
  
- `DISTINCT`는 `SELECT` 바로 뒤에 위치하며, 지정한 컬럼에 대해 **중복된 행은 하나로 간주**하고 고유한 값만 반환합니다.  
- 여러 컬럼을 지정하면, **모든 지정된 컬럼 조합이 중복되지 않는 행만** 반환합니다.  
  
### 예시 테이블  
예를 들어, `employees`라는 테이블이 있다고 가정합니다.  
  
| id  | name     | department |  
|-----|----------|------------|  
| 1   | Alice    | HR         |  
| 2   | Bob      | IT         |  
| 3   | Charlie  | Sales      |  
| 4   | Diana    | IT         |  
| 5   | Edward   | HR         |  
| 6   | Frank    | Sales      |  
  
### 1. 단일 컬럼에 DISTINCT 사용  
`department` 컬럼에 대해 중복을 제거하고 각 부서의 이름을 한 번씩만 가져오려면 다음과 같이 `DISTINCT`를 사용할 수 있습니다.  
  
```sql  
SELECT DISTINCT department  
FROM employees;  
```  
  
#### 결과:  
| department |  
|------------|  
| HR         |  
| IT         |  
| Sales      |  
  
위 쿼리는 `employees` 테이블에서 `department` 값이 고유한 행만 반환합니다.  
  
### 2. 여러 컬럼에 DISTINCT 사용  
여러 컬럼에 대해 중복을 제거하려면, 지정한 컬럼 조합이 모두 일치하는 행을 중복으로 간주하고 하나로 처리합니다. 예를 들어, `department`와 `name` 컬럼 조합에 대해 고유한 행만 반환하려면 다음과 같이 작성할 수 있습니다.  
  
```sql  
SELECT DISTINCT department, name  
FROM employees;  
```  
  
#### 결과:  
| department | name     |  
|------------|----------|  
| HR         | Alice    |  
| IT         | Bob      |  
| Sales      | Charlie  |  
| IT         | Diana    |  
| HR         | Edward   |  
| Sales      | Frank    |  
  
각 `department`, `name` 조합이 고유한 행만 반환됩니다. 모든 열이 서로 다른 값을 가지기 때문에 모든 행이 결과에 포함됩니다.  
  
### 3. COUNT와 함께 DISTINCT 사용  
`DISTINCT`는 `COUNT` 함수와 함께 사용하여 고유한 값의 개수를 세는 데 유용합니다. 예를 들어, 각 부서의 고유한 수를 세려면 다음과 같이 쓸 수 있습니다.  
  
```sql  
SELECT COUNT(DISTINCT department) AS unique_departments  
FROM employees;  
```  
  
#### 결과:  
| unique_departments |  
|--------------------|  
| 3                  |  
  
이 쿼리는 `department`의 고유한 값이 3개임을 반환합니다.  
  
### DISTINCT의 주요 특징과 주의사항  
- `DISTINCT`는 데이터의 **중복을 제거**하여 간결한 결과를 얻는 데 유용합니다.  
- 다만, **여러 컬럼을 지정**할 경우, 지정된 컬럼 조합이 동일한 경우에만 중복을 제거합니다.  
- `DISTINCT`는 **대용량 데이터에서 성능에 영향을 줄 수 있으므로** 필요한 경우에만 사용하는 것이 좋습니다.  
  
### 요약  
- **DISTINCT**는 중복을 제거하여 고유한 값을 반환하는 키워드입니다.  
- 단일 또는 여러 컬럼에 대해 고유한 조합을 가져올 수 있습니다.  
- `COUNT(DISTINCT 컬럼명)`과 같이 **집계 함수와 결합**하여 고유 값의 개수를 구할 수도 있습니다.  
  
`DISTINCT`는 데이터에서 중복을 제거하여 유니크한 결과를 얻고자 할 때 매우 유용한 SQL 기능입니다.  
  
  
## GREATEST, LEAST  
  
`GREATEST`는 SQL에서 **여러 값 중 가장 큰 값을 반환**하는 함수입니다. 주어진 값 중에서 최댓값을 찾고자 할 때 유용하게 사용됩니다. 숫자, 날짜, 문자열 등 비교 가능한 데이터 타입에 사용 가능하며, 여러 컬럼이나 표현식의 최댓값을 구할 수 있습니다.  
  
### 기본 문법  
```sql  
GREATEST(value1, value2, ...);  
```  
  
- `GREATEST` 함수는 여러 개의 값을 인수로 받아, **가장 큰 값**을 반환합니다.  
- 모든 인수가 **동일한 데이터 타입**이거나 비교 가능한 데이터 타입이어야 합니다.  
- 만약 인수 중 하나라도 `NULL`이면 `GREATEST`는 `NULL`을 반환합니다. `NULL`을 무시하려면 `COALESCE` 함수와 함께 사용할 수 있습니다.  
  
### 예시  
예를 들어, `scores`라는 테이블에 학생의 여러 시험 점수가 있다고 가정합니다.  
  
**`scores` 테이블**  
| student_id | test1 | test2 | test3 |  
|------------|-------|-------|-------|  
| 1          | 85    | 90    | 78    |  
| 2          | 88    | 92    | 85    |  
| 3          | 70    | 68    | 75    |  
  
### 1. GREATEST를 사용하여 최댓값 구하기  
각 학생의 최고 점수를 구하고자 할 때 `GREATEST`를 사용할 수 있습니다.  
  
```sql  
SELECT student_id, GREATEST(test1, test2, test3) AS highest_score  
FROM scores;  
```  
  
#### 결과:  
| student_id | highest_score |  
|------------|---------------|  
| 1          | 90            |  
| 2          | 92            |  
| 3          | 75            |  
  
이 쿼리는 `test1`, `test2`, `test3` 중에서 가장 큰 값을 찾아 `highest_score`로 반환합니다.  
  
### 2. NULL 값을 처리하는 예시  
만약 특정 시험 점수가 `NULL`이라면, `GREATEST`는 결과를 `NULL`로 반환합니다. 이를 방지하려면 `COALESCE`를 사용하여 `NULL` 값을 기본값(예: 0)으로 바꿀 수 있습니다.  
  
예를 들어, `test2`가 `NULL`인 학생이 있을 때 다음과 같이 작성할 수 있습니다.  
  
```sql  
SELECT student_id, GREATEST(test1, COALESCE(test2, 0), test3) AS highest_score  
FROM scores;  
```  
  
이 쿼리는 `NULL` 값을 0으로 대체하여, `NULL`이 있는 경우에도 최댓값을 정상적으로 반환합니다.  
  
### 3. 날짜나 문자열에 사용하기  
`GREATEST`는 숫자뿐 아니라 날짜나 문자열에도 사용할 수 있습니다. 예를 들어, 두 날짜 중 더 최근 날짜를 찾고자 할 때도 사용 가능합니다.  
  
```sql  
SELECT GREATEST('2023-01-01', '2022-12-31') AS latest_date;  
```  
  
#### 결과:  
| latest_date |  
|-------------|  
| 2023-01-01  |  
  
문자열에서는 사전 순으로 더 큰 값을 반환합니다.  
  
```sql  
SELECT GREATEST('apple', 'banana', 'cherry') AS greatest_word;  
```  
  
#### 결과:  
| greatest_word |  
|---------------|  
| cherry        |  
  
여기서 `GREATEST`는 알파벳 순으로 가장 나중에 나오는 `cherry`를 반환합니다.  
  
### 요약  
- **`GREATEST`**는 여러 값 중 최댓값을 반환하는 함수입니다.  
- 숫자, 날짜, 문자열을 비교할 수 있으며, 데이터 타입이 일치해야 합니다.  
- **`NULL`이 포함되면 `NULL`을 반환**하므로, `COALESCE`와 함께 사용하여 `NULL`을 다른 값으로 대체할 수 있습니다.  
    
`GREATEST`는 여러 컬럼 중 최댓값을 구할 때 간단하고 유용한 방법을 제공합니다.  
  
✅ LEAST도 마찬가지로 적용하면 됨  
  
 ## CASE  
  
`CASE` 문은 SQL에서 조건에 따라 다른 값을 반환하거나 다른 작업을 수행할 수 있도록 하는 **조건문**입니다. `CASE` 문은 `WHEN`, `THEN`, `ELSE`, `END` 키워드와 함께 사용되며, 조건에 맞는 값을 찾을 때까지 각 조건을 순차적으로 평가합니다. 프로그래밍 언어의 `if-else` 문과 유사하게 작동합니다.  
  
### 기본 문법  
```sql  
SELECT 컬럼명,  
       CASE   
           WHEN 조건1 THEN 결과1  
           WHEN 조건2 THEN 결과2  
           ...  
           ELSE 기본값  
       END AS 새_컬럼명  
FROM 테이블명;  
```  
  
- **WHEN**: 평가할 조건을 지정합니다.  
- **THEN**: 조건이 참일 경우 반환할 값을 지정합니다.  
- **ELSE**: 모든 조건이 거짓일 경우 반환할 기본값을 지정합니다. (선택 사항)  
- **END**: `CASE` 문을 종료합니다.  
- `CASE` 문은 **컬럼에 대한 조건별 분기**를 만들거나 계산된 값을 반환할 때 유용합니다.  
  
### 예제 테이블  
예를 들어, `employees`라는 테이블에 직원의 급여 정보가 있다고 가정합니다.  
  
**`employees` 테이블**  
| id  | name    | department | salary |  
|-----|---------|------------|--------|  
| 1   | Alice   | HR         | 60000  |  
| 2   | Bob     | IT         | 75000  |  
| 3   | Charlie | Sales      | 50000  |  
| 4   | Diana   | IT         | 80000  |  
| 5   | Edward  | Sales      | 55000  |  
  
### 1. 단순 CASE 사용 예시  
각 직원의 급여가 70,000 이상이면 'High', 50,000 이상이면 'Medium', 그렇지 않으면 'Low'로 분류하고 싶다면 `CASE` 문을 다음과 같이 작성할 수 있습니다.  
  
```sql  
SELECT name, salary,  
       CASE  
           WHEN salary >= 70000 THEN 'High'  
           WHEN salary >= 50000 THEN 'Medium'  
           ELSE 'Low'  
       END AS salary_category  
FROM employees;  
```  
  
#### 결과:  
| name    | salary | salary_category |  
|---------|--------|-----------------|  
| Alice   | 60000  | Medium          |  
| Bob     | 75000  | High            |  
| Charlie | 50000  | Medium          |  
| Diana   | 80000  | High            |  
| Edward  | 55000  | Medium          |  
  
여기서:  
- `salary >= 70000` 조건이 참이면 `High`를 반환합니다.  
- `salary >= 50000` 조건이 참이면 `Medium`을 반환합니다.  
- 모든 조건이 거짓인 경우 `Low`를 반환합니다.  
  
### 2. CASE와 ELSE 사용  
`ELSE`는 모든 조건이 거짓일 때 반환할 기본값을 지정합니다. 예를 들어, 특정 부서(`IT`)의 직원에게는 `Tech`, 그렇지 않은 직원에게는 `Non-Tech` 라벨을 붙일 수 있습니다.  
  
```sql  
SELECT name, department,  
       CASE  
           WHEN department = 'IT' THEN 'Tech'  
           ELSE 'Non-Tech'  
       END AS department_category  
FROM employees;  
```  
  
#### 결과:  
| name    | department | department_category |  
|---------|------------|---------------------|  
| Alice   | HR         | Non-Tech           |  
| Bob     | IT         | Tech               |  
| Charlie | Sales      | Non-Tech           |  
| Diana   | IT         | Tech               |  
| Edward  | Sales      | Non-Tech           |  
  
여기서:  
- `department = 'IT'` 조건이 참이면 `Tech`를 반환하고, 그렇지 않으면 `Non-Tech`를 반환합니다.  
  
  
## Local PostgreSQL Installation  
  
[Postgres.app 웹페이지](https://postgresapp.com)  
  
**Postgres.app**과 **PostgreSQL** 차이점  
  
| 항목           | PostgreSQL                                          | Postgres.app                                   |  
|----------------|-----------------------------------------------------|------------------------------------------------|  
| 역할           | 데이터베이스 엔진(RDBMS)                             | macOS용 PostgreSQL 설치 및 관리 앱             |  
| 설치 플랫폼    | Linux, Windows, macOS                               | macOS 전용                                     |  
| 설치 과정      | 패키지 매니저, 설치 파일 사용                        | 앱 다운로드 및 실행으로 간단히 설치           |  
| 관리 방식      | 터미널, SQL 클라이언트 도구 사용                    | GUI 기반으로 간단하게 데이터베이스 관리       |  
| 포함된 도구    | 데이터베이스 서버                                  | PostgreSQL 서버 + 여러 확장과 도구 포함       |  
  
### 요약  
- **PostgreSQL**은 데이터베이스 시스템 자체로, 다양한 플랫폼에서 사용할 수 있습니다.  
- **Postgres.app**은 **macOS에서 PostgreSQL을 쉽게 설치하고 실행할 수 있도록** 하는 애플리케이션입니다.  
    
Postgres.app은 macOS 환경에서 PostgreSQL을 간단히 시작하려는 사용자에게 특히 유용하며, PostgreSQL은 모든 플랫폼에서 사용 가능한 본격적인 RDBMS입니다.  
  
  
## Postgres.app의 경로를 PATH 환경변수에 추가하기  
  
### 1. PATH 파일 직접 편집하기  
터미널에서 `~/.zshrc` 파일을 직접 편집하여 PostgreSQL의 경로를 추가하는 방법입니다.  
  
```bash  
nano ~/.zshrc  
```  
  
위 명령어를 실행하면, 편집기 창이 열립니다. 파일의 마지막 줄에 아래 내용을 추가하세요.  
  
```bash  
export PATH="/Applications/Postgres.app/Contents/Versions/latest/bin:$PATH"  
```  
  
### 2. 설정 파일 적용하기  
파일을 저장하고 나서, 터미널에 다음 명령어를 입력하여 변경 사항을 적용하세요.  
  
```bash  
source ~/.zshrc  
```  
  
### 3. 설정 확인하기  
이제 `psql --version` 명령어로 PostgreSQL이 정상적으로 인식되는지 확인해 보세요.  
  
```bash  
psql --version  
```  
  
### 4. 터미널 재시작  
만약 여전히 `psql` 명령어가 인식되지 않는다면, 터미널을 완전히 닫고 다시 열어서 적용이 되었는지 확인합니다.  
  
## pgAdmin  
**pgAdmin**은 PostgreSQL 데이터베이스를 관리하고 상호작용할 수 있도록 돕는 **그래픽 사용자 인터페이스(GUI) 툴**입니다. PostgreSQL에 기본적으로 포함된 터미널 기반의 `psql`과는 달리, **직관적인 인터페이스**를 제공하여 데이터베이스 작업을 더 쉽게 할 수 있도록 도와줍니다.   
  
### 주요 기능  
pgAdmin은 데이터베이스 관리자(DBA), 개발자, 데이터 분석가들이 데이터베이스를 더 쉽게 관리하고 사용할 수 있도록 여러 기능을 제공합니다.  
  
### pgAdmin 설치 및 사용법  
1. **설치**:  
   - [pgAdmin 공식 사이트](https://www.pgadmin.org/)에서 운영 체제에 맞는 설치 파일을 다운로드하여 설치할 수 있습니다.  
   - 설치가 완료되면 pgAdmin을 실행할 수 있습니다.  
  
2. **PostgreSQL 서버 연결**:  
   - pgAdmin을 실행하면, PostgreSQL 서버와 연결을 설정해야 합니다.  
   - PostgreSQL이 설치된 서버의 IP 주소, 포트 번호(기본값 5432), 데이터베이스 이름, 사용자 이름 및 비밀번호를 입력하여 연결합니다.  
  
3. **데이터베이스 작업 시작**:  
   - 연결 후 pgAdmin의 왼쪽 패널에서 PostgreSQL 서버와 데이터베이스 목록을 탐색할 수 있습니다.  
   - 테이블을 클릭하여 데이터를 조회하거나, SQL 쿼리 편집기를 열어 SQL 작업을 수행할 수 있습니다.  
  
### pgAdmin의 장점  
- **직관적인 GUI**: 그래픽 기반으로 데이터를 쉽게 탐색하고 관리할 수 있어, PostgreSQL에 익숙하지 않은 사용자도 빠르게 익힐 수 있습니다.  
- **강력한 도구 통합**: 데이터베이스 작업뿐 아니라 모니터링, 분석, 백업, 복원 등 다양한 관리 기능이 통합되어 있어 데이터베이스 관리에 필요한 거의 모든 기능을 제공합니다.  
- **멀티 플랫폼 지원**: Windows, macOS, Linux 등 여러 운영 체제에서 사용할 수 있습니다.  
- **협업에 용이**: 여러 개의 PostgreSQL 서버를 연결하고 관리할 수 있어, 대규모 데이터베이스 환경에서도 효율적으로 사용할 수 있습니다.  
  
### 요약  
pgAdmin은 PostgreSQL 데이터베이스의 다양한 작업을 시각적이고 직관적인 방식으로 수행할 수 있는 GUI 툴입니다. 데이터베이스 관리자나 개발자에게 유용하며, SQL 쿼리 실행, 데이터 관리, 모니터링, 백업, 사용자 권한 관리 등 **PostgreSQL 관리에 필요한 기능을 광범위하게 지원**합니다.  
  
- 서버 생성시 Username은 사용자 이름으로  
  
## PostgreSQL Complex Datatypes  
  
PostgreSQL은 다양한 데이터 타입을 지원하여, 데이터베이스에서 효율적으로 데이터를 저장하고 관리할 수 있도록 합니다. 각 데이터 타입의 주요 카테고리와 특성에 대해 설명하겠습니다.  
  
### 1. Numbers (숫자)  
PostgreSQL은 **정수형**과 **소수형** 등 여러 숫자 타입을 제공합니다.  
  
- **INTEGER / INT**: 4바이트 정수 타입으로, 약 -21억에서 +21억까지 저장할 수 있습니다.  
- **SMALLINT**: 2바이트 정수 타입으로, -32,768에서 +32,767까지 저장할 수 있습니다.  
- **BIGINT**: 8바이트 정수 타입으로, 매우 큰 정수 값을 저장할 수 있습니다.  
- **SERIAL / BIGSERIAL**: 자동 증가 정수 타입으로, 주로 Primary Key에 사용됩니다.  
- **DECIMAL / NUMERIC**: 고정 소수점 숫자 타입으로, 정확한 소수점 연산이 필요할 때 사용합니다.  
- **REAL**: 4바이트 실수 타입, 부동 소수점 표현을 사용하며, 부동 소수점 정밀도가 높지 않은 실수 데이터를 저장할 때 사용합니다.  
- **DOUBLE PRECISION**: 8바이트 실수 타입으로, 정밀도가 높은 부동 소수점 값을 저장할 수 있습니다.  
  
### 2. Currency (통화)  
- **MONEY**: 금액을 표현하는 타입으로, 통화 형식을 자동으로 적용합니다.  
- 예를 들어, `$123.45`처럼 통화 기호와 소수점을 포함한 형태로 금액을 저장하고 표시할 수 있습니다.  
  
### 3. Binary (이진 데이터)  
PostgreSQL에서 이진 데이터는 파일이나 이미지, 음악 등 **크고 복잡한 바이너리 데이터를 저장**할 때 사용됩니다.  
  
- **BYTEA**: 바이너리 데이터를 저장하기 위한 타입으로, `BLOB`과 유사합니다. 파일, 이미지, 음악 파일 등을 저장할 수 있습니다.  
  
### 4. Date/Time (날짜와 시간)  
PostgreSQL은 다양한 **날짜와 시간 데이터**를 저장할 수 있도록 여러 타입을 지원합니다.  
  
- **DATE**: 날짜를 저장하는 타입으로, 연도-월-일 형식으로 저장됩니다.  
- **TIME [WITHOUT TIME ZONE]**: 시간을 저장하는 타입으로, 시:분:초 형식으로 저장됩니다.  
- **TIME WITH TIME ZONE**: 시간을 저장하는 타입으로, 시간대 정보를 포함합니다.  
- **TIMESTAMP [WITHOUT TIME ZONE]**: 날짜와 시간을 모두 저장하며, 시간대 정보는 포함하지 않습니다.  
- **TIMESTAMP WITH TIME ZONE**: 날짜, 시간과 시간대 정보를 함께 저장합니다.  
- **INTERVAL**: 두 날짜 또는 시간의 차이를 저장하며, 기간을 나타낼 수 있습니다. 예를 들어 `2 days`, `3 hours` 등의 값을 저장할 수 있습니다.  
  
### 5. Character (문자형)  
PostgreSQL은 다양한 문자열 타입을 제공하여 **문자 데이터를 효율적으로 저장**할 수 있습니다.  
  
- **CHAR(N)**: 고정 길이 문자열 타입으로, 길이가 부족할 경우 공백으로 채워집니다.  
- **VARCHAR(N)**: 가변 길이 문자열 타입으로, 최대 길이를 지정할 수 있습니다.  
- **TEXT**: 문자열의 길이에 제한이 없는 타입으로, 매우 긴 텍스트 데이터를 저장할 수 있습니다.  
  
### 6. JSON (제이슨)  
PostgreSQL은 JSON 데이터를 저장하고 조작할 수 있는 **두 가지 JSON 타입**을 제공합니다.  
  
- **JSON**: JSON 형식의 텍스트 데이터를 저장하며, 데이터가 유효한 JSON 형식인지 검증합니다.  
- **JSONB**: JSON 데이터를 바이너리 형태로 저장하여, 효율적인 검색과 인덱싱이 가능합니다. JSON 데이터가 많이 포함된 큰 데이터를 처리할 때 적합합니다.  
  
### 7. Geometric (기하학적 데이터)  
기하학적 데이터를 저장하기 위한 **도형 타입**을 제공하여, 위치나 형태를 저장할 수 있습니다.  
  
- **POINT**: `(x, y)` 형태의 점을 저장합니다.  
- **LINE**: 무한 직선을 저장합니다.  
- **LSEG**: 선분을 저장합니다.  
- **BOX**: 직사각형을 저장합니다.  
- **PATH**: 여러 점으로 구성된 경로를 저장합니다.  
- **POLYGON**: 다각형을 저장합니다.  
- **CIRCLE**: 원을 저장합니다.  
  
### 8. Range (범위)  
PostgreSQL은 **범위 데이터를 저장할 수 있는 특수한 데이터 타입**을 지원합니다. 범위 타입은 특정 범위에 포함된 데이터를 저장할 수 있습니다.  
  
- **INT4RANGE**: `INTEGER` 범위를 저장합니다.  
- **INT8RANGE**: `BIGINT` 범위를 저장합니다.  
- **NUMRANGE**: `NUMERIC` 범위를 저장합니다.  
- **TSRANGE**: `TIMESTAMP WITHOUT TIME ZONE` 범위를 저장합니다.  
- **TSTZRANGE**: `TIMESTAMP WITH TIME ZONE` 범위를 저장합니다.  
- **DATERANGE**: `DATE` 범위를 저장합니다.  
  
### 9. Arrays (배열)  
PostgreSQL에서는 배열 타입을 지원하여 **하나의 컬럼에 여러 값을 저장**할 수 있습니다.  
  
- 예를 들어, `INTEGER[]`, `TEXT[]`, `VARCHAR[]`와 같은 배열을 정의하여 여러 값의 리스트를 저장할 수 있습니다.  
- 배열의 각 요소에 접근하고 조작할 수 있는 다양한 함수와 연산자도 제공합니다.  
  
### 10. Boolean (논리형)  
PostgreSQL의 **논리 데이터 타입**으로, **참(True) 또는 거짓(False)**을 저장합니다.  
  
- **BOOLEAN**: `TRUE`, `FALSE`, `NULL` 값을 가질 수 있습니다.  
- 논리 조건이나 상태를 저장할 때 자주 사용됩니다.  
  
### 11. XML  
PostgreSQL은 **XML 데이터**를 저장하고 처리할 수 있는 데이터 타입도 지원합니다.  
  
- **XML**: XML 형식의 데이터를 저장합니다.  
- XML 데이터의 구조적 무결성을 검증하며, 쿼리 내에서 XML 함수를 통해 데이터 조회와 조작이 가능합니다.  
  
### 12. UUID (Universally Unique Identifier)  
PostgreSQL은 **UUID 데이터 타입**을 지원하여, 고유한 식별자를 생성하고 저장할 수 있습니다.  
  
- **UUID**: 전역 고유 식별자로, 일반적으로 128비트 고유 식별자를 사용합니다. 식별자 간 충돌을 방지하고자 할 때 유용합니다.  
- UUID는 주로 분산 시스템에서 객체를 고유하게 식별할 때 사용됩니다.  
  
### 요약  
- **Numbers, Currency**: 숫자 및 통화 데이터를 저장합니다.  
- **Binary**: 바이너리 데이터를 저장하며, 주로 파일 및 이미지 데이터에 사용됩니다.  
- **Date/Time**: 날짜 및 시간 데이터를 저장합니다.  
- **Character**: 문자 및 텍스트 데이터를 저장합니다.  
- **JSON**: JSON 데이터를 저장하며, JSONB는 효율적인 검색을 제공합니다.  
- **Geometric**: 기하학적 데이터 타입으로, 점, 선, 다각형 등을 저장합니다.  
- **Range**: 특정 범위의 데이터(날짜, 숫자 등)를 저장합니다.  
- **Arrays**: 하나의 컬럼에 여러 값을 배열 형태로 저장합니다.  
- **Boolean**: 논리형 데이터로 참, 거짓 값을 저장합니다.  
- **XML**: XML 데이터를 저장합니다.  
- **UUID**: 고유한 식별자 데이터를 저장합니다.  
  
이 다양한 데이터 타입을 통해 PostgreSQL은 구조화된 데이터, 반정형 데이터, 고유한 데이터 식별자, 시공간 데이터 등 복잡하고 다양한 데이터를 효율적으로 처리할 수 있습니다.  
  
## Numeric Types Fast Rules  
id : SERIAL  
Need to store a number without a decimal : INTEGER  
with a decimal and needs to be very accurate : NUMERIC  
with a decimal and the decimal doesn't make a big diference : DOUBLE PRECISION  
  
## 타입 캐스팅 연산자 `::`  
`SELECT (2.0 :: INTEGER)`는 PostgreSQL에서 **데이터 타입 캐스팅(type casting)**을 사용하여 **`2.0`이라는 실수(float) 값을 정수(integer) 값으로 변환**하는 것을 의미합니다. 여기서 `::`는 **타입 캐스팅 연산자**로, 왼쪽의 값을 오른쪽에 지정된 데이터 타입으로 변환하는 역할을 합니다.  
  
### 동작 방식  
- **`2.0 :: INTEGER`**: `2.0`은 실수(float)로, 정수(integer) 타입으로 변환될 때 소수점 이하 부분이 **잘려서** `2`로 변환됩니다.   
- 결과적으로 이 쿼리는 **정수 2를 반환**합니다.  
  
### 추가 설명: 타입 캐스팅 방식  
- PostgreSQL에서는 `::` 외에도 `CAST` 함수를 사용하여 타입 캐스팅을 할 수 있습니다.   
- 위와 동일한 쿼리는 `CAST`를 사용해 다음과 같이 작성할 수 있습니다:  
  
  ```sql  
  SELECT CAST(2.0 AS INTEGER);  
  ```  
  
## Boolean Types  
`TRUE` : true, yes, on, 1, t, y  
`FALSE` : false, no, off, 0, f, n  
`NULL` : null  
  
  
## INTERVAL  
  
`INTERVAL`은 PostgreSQL에서 **날짜 및 시간 간격**(기간)을 저장하거나 계산할 때 사용하는 데이터 타입입니다. 특정 날짜와 시간 간의 차이를 저장하거나, 날짜와 시간을 더하거나 빼는 연산에 활용됩니다. `INTERVAL`은 **"n년, n개월, n일, n시간, n분, n초"** 등 다양한 단위로 기간을 표현할 수 있습니다.  
  
---  
  
### 기본 문법  
```sql  
INTERVAL '표현식'  
```  
  
- `표현식`은 **연도, 월, 일, 시간, 분, 초** 등으로 구성된 기간을 나타냅니다.  
- 예를 들어, `INTERVAL '1 year 6 months'`는 **1년 6개월**의 기간을 나타냅니다.  
  
---  
  
### INTERVAL 사용 예제  
  
#### 1. INTERVAL 선언  
INTERVAL 값을 직접 선언하여 사용할 수 있습니다.  
  
```sql  
SELECT INTERVAL '2 years 3 months 10 days';  
```  
  
#### 결과:  
| ?column?              |  
|-----------------------|  
| 2 years 3 mons 10 days |  
  
---  
  
#### 2. 날짜 연산에 INTERVAL 사용  
`INTERVAL`을 사용하여 날짜와 시간을 더하거나 뺄 수 있습니다.  
  
- 날짜에 기간을 더하기:  
```sql  
SELECT CURRENT_DATE + INTERVAL '1 year 2 months';  
```  
  
#### 결과 (예시):  
| ?column?   |  
|------------|  
| 2025-01-12 |  
  
- 날짜에서 기간을 빼기:  
```sql  
SELECT CURRENT_DATE - INTERVAL '30 days';  
```  
  
#### 결과 (예시):  
| ?column?   |  
|------------|  
| 2023-11-12 |  
  
---  
  
#### 3. INTERVAL과 TIMESTAMP 사용  
`INTERVAL`은 **TIMESTAMP**와 함께 사용되어 날짜와 시간 간의 차이를 계산하거나 특정 기간 후의 시간을 계산할 수 있습니다.  
  
```sql  
SELECT TIMESTAMP '2024-01-01 10:00:00' + INTERVAL '3 hours 30 minutes';  
```  
  
#### 결과:  
| ?column?             |  
|----------------------|  
| 2024-01-01 13:30:00  |  
  
---  
  
### INTERVAL 단위  
`INTERVAL`은 다양한 시간 단위를 지원합니다. 각 단위를 조합하여 복합적인 기간을 나타낼 수도 있습니다.  
  
#### 지원 단위  
- `YEAR`: 연도 (예: `1 year`)  
- `MONTH`: 월 (예: `3 months`)  
- `DAY`: 일 (예: `7 days`)  
- `HOUR`: 시간 (예: `5 hours`)  
- `MINUTE`: 분 (예: `15 minutes`)  
- `SECOND`: 초 (예: `45 seconds`)  
- `MILLISECOND` 또는 `MICROSECOND`: 밀리초 및 마이크로초  
  
#### 복합 단위  
다양한 단위를 조합하여 표현할 수 있습니다.  
  
```sql  
SELECT INTERVAL '1 year 2 months 10 days 3 hours 15 minutes 20 seconds';  
```  
  
---  
  
### INTERVAL 관련 함수  
  
1. **AGE()**  
   - 두 날짜 사이의 차이를 INTERVAL로 반환합니다.  
   ```sql  
   SELECT AGE(TIMESTAMP '2024-01-01', TIMESTAMP '2022-05-01');  
   ```  
  
   #### 결과:  
   | age                |  
   |--------------------|  
   | 1 year 8 mons      |  
  
2. **JUSTIFY_DAYS()**  
   - INTERVAL 값을 정리하여 불합리한 날짜 간격을 조정합니다.  
   ```sql  
   SELECT JUSTIFY_DAYS(INTERVAL '40 days');  
   ```  
  
   #### 결과:  
   | justify_days       |  
   |--------------------|  
   | 1 mon 10 days      |  
  
3. **EXTRACT()**  
   - INTERVAL 값에서 특정 단위를 추출합니다.  
   ```sql  
   SELECT EXTRACT(YEAR FROM INTERVAL '2 years 6 months');  
   ```  
  
   #### 결과:  
   | date_part |  
   |-----------|  
   | 2         |  
  
---  
  
### 사용 사례  
1. **청구 기간 계산**  
   - `INTERVAL`을 사용하여 다음 청구일 계산:  
   ```sql  
   SELECT CURRENT_DATE + INTERVAL '1 month' AS next_billing_date;  
   ```  
  
2. **구독 만료일 확인**  
   - 특정 날짜로부터 1년 후 구독 만료일 계산:  
   ```sql  
   SELECT TIMESTAMP '2024-01-01' + INTERVAL '1 year' AS expiration_date;  
   ```  
  
3. **시간차 계산**  
   - 두 날짜 사이의 차이를 계산:  
   ```sql  
   SELECT AGE(TIMESTAMP '2024-01-01', TIMESTAMP '2023-01-01');  
   ```  
  
---  
  
### 요약  
- `INTERVAL`은 PostgreSQL에서 기간(시간 간격)을 나타내는 데이터 타입입니다.  
- 날짜/시간 계산에 유용하며, 다양한 단위를 지원합니다.  
- TIMESTAMP, CURRENT_DATE 등과 함께 사용하여 날짜/시간 연산을 수행할 수 있습니다.  
- 관련 함수(`AGE`, `JUSTIFY_DAYS`, `EXTRACT`)를 활용하면 더 복잡한 연산도 가능합니다.  
  
  
## Database - Side Validation and Constraints  
- 유효성 검사  
  
- 테이블 확인하려면. : Schemas > public > Tables 우클릭 > View/Edit Data  
- pgAdmin에서 테이블 row 데이터 내용 변경 후 자체 인터페이스에서 저장 가능함(Save Data Changes 아이콘)  
  
PostgreSQL의 **Multi-Column Uniqueness**는 여러 컬럼 조합에 대해 **유일성 제약 조건(Unique Constraint)**을 적용하는 기능입니다. 이를 통해 특정 컬럼의 개별 값이 아닌, 여러 컬럼의 **조합된 값이 고유**하도록 보장할 수 있습니다.  
  
  
## UNIQUE  
  
### 1. Multi-Column Uniqueness란?  
- 하나의 컬럼만 유일성을 보장하는 것이 아니라, **두 개 이상의 컬럼 값의 조합이 유일**해야 함을 보장하는 제약 조건입니다.  
- 예를 들어, `first_name`과 `last_name` 컬럼이 있는 테이블에서, 동일한 `first_name`과 `last_name` 조합이 중복되지 않도록 할 수 있습니다.  
  
---  
  
### 2. 사용 목적  
1. **복합 키의 유일성 보장**:  
   - 여러 컬럼의 조합이 중복되지 않도록 데이터의 무결성을 유지합니다.  
2. **중복 데이터 방지**:  
   - 특정 조건에서 중복된 데이터를 입력하지 못하도록 제한합니다.  
3. **유니크 제약 조건을 확장**:  
   - 개별 컬럼 수준이 아니라, 컬럼 간 조합에 대해 유일성을 확인합니다.  
  
---  
  
### 3. 문법  
다음과 같은 방식으로 테이블 생성 시 Multi-Column Unique Constraint를 정의할 수 있습니다.  
  
#### 예제 1: 테이블 생성 시 유니크 제약 추가  
```sql  
CREATE TABLE users (  
    first_name VARCHAR(50),  
    last_name VARCHAR(50),  
    email VARCHAR(100),  
    CONSTRAINT unique_name_email UNIQUE (first_name, last_name)  
);  
```  
  
- 위 쿼리는 `first_name`과 `last_name`의 조합이 유일해야 함을 보장합니다.  
- 즉, 동일한 이름과 성 조합을 가진 데이터는 테이블에 추가할 수 없습니다.  
  
#### 예제 2: ALTER TABLE로 기존 테이블에 추가  
이미 생성된 테이블에 Multi-Column Unique Constraint를 추가하려면 `ALTER TABLE`을 사용할 수 있습니다.  
  
```sql  
ALTER TABLE users  
ADD CONSTRAINT unique_name_email UNIQUE (first_name, last_name);  
```  
  
---  
  
### 4. 데이터 삽입 예제  
#### 데이터 삽입 성공:  
```sql  
INSERT INTO users (first_name, last_name, email) VALUES ('Alice', 'Smith', 'alice@example.com');  
INSERT INTO users (first_name, last_name, email) VALUES ('Bob', 'Smith', 'bob@example.com');  
```  
  
위 삽입은 성공합니다. 왜냐하면 `first_name`과 `last_name`의 조합이 서로 다르기 때문입니다.  
  
#### 데이터 삽입 실패:  
```sql  
INSERT INTO users (first_name, last_name, email) VALUES ('Alice', 'Smith', 'anotheralice@example.com');  
```  
  
위 삽입은 실패합니다. 왜냐하면 이미 `('Alice', 'Smith')` 조합이 존재하기 때문입니다. PostgreSQL은 중복된 값에 대해 오류를 반환합니다.  
  
---  
  
### 5. Multi-Column Uniqueness와 NULL  
PostgreSQL에서는 **NULL 값은 서로 다르다고 간주**합니다. 따라서, 유니크 제약 조건이 적용된 컬럼 조합 중 하나에 NULL 값이 있다면, 다른 행에서도 동일한 NULL 값이 허용됩니다.  
  
#### 예시:  
```sql  
CREATE TABLE example (  
    col1 INT,  
    col2 INT,  
    CONSTRAINT unique_col1_col2 UNIQUE (col1, col2)  
);  
  
INSERT INTO example (col1, col2) VALUES (1, NULL); -- 성공  
INSERT INTO example (col1, col2) VALUES (1, NULL); -- 성공 (NULL은 다르다고 간주)  
INSERT INTO example (col1, col2) VALUES (1, 2);    -- 성공  
INSERT INTO example (col1, col2) VALUES (1, 2);    -- 실패 (1, 2는 중복)  
```  
  
---  
  
### 6. Multi-Column Unique Index  
Multi-Column Unique Constraint는 내부적으로 **Unique Index**를 생성합니다. 이를 통해 빠르게 중복을 확인할 수 있습니다. 직접 Unique Index를 생성할 수도 있습니다.  
  
#### 예시:  
```sql  
CREATE UNIQUE INDEX unique_index_name  
ON users (first_name, last_name);  
```  
  
---  
  
### 7. Multi-Column Uniqueness의 활용 사례  
1. **복합 키**:  
   - 예: 사용자 `user_id`와 `email`이 조합으로 고유해야 할 때.  
  
2. **다중 외래 키의 유일성 보장**:  
   - 예: 학생과 수업 관계를 저장하는 테이블에서 `student_id`와 `course_id`의 조합이 중복되지 않아야 할 때.  
  
3. **지역 기반 데이터**:  
   - 예: `city`와 `zip_code` 조합이 중복되지 않도록 보장.  
  
---  
  
### 요약  
- PostgreSQL에서 Multi-Column Uniqueness는 **여러 컬럼의 조합이 유일하도록 보장**합니다.  
- `CONSTRAINT` 또는 `UNIQUE INDEX`를 사용해 설정할 수 있습니다.  
- NULL 값은 서로 다르다고 간주되며, 중복 처리가 허용됩니다.  
- 복합 키, 데이터 무결성 유지, 중복 방지와 같은 다양한 시나리오에서 유용하게 사용됩니다.  
  
  
PostgreSQL에서 **`CHECK` 제약 조건**은 테이블에 저장되는 데이터가 특정 **조건을 만족하는지 검사**하기 위해 사용됩니다. 특정 컬럼의 값이 미리 정의된 규칙을 따르도록 강제하여, **데이터 무결성을 유지**하는 데 중요한 역할을 합니다.  
  
---  
  
## CHECK  
  
### 1. CHECK 제약 조건의 역할  
- **데이터 유효성 검사**: 데이터를 삽입하거나 수정할 때, 지정된 조건을 만족하지 않으면 작업이 실패합니다.  
- **무결성 보장**: 테이블에 잘못된 데이터가 저장되지 않도록 방지합니다.  
- **커스텀 규칙 적용**: 특정 컬럼 값이 범위 안에 있어야 하거나 특정 조건을 만족해야 하는 경우 사용됩니다.  
  
---  
  
### 2. 기본 문법  
`CHECK`는 테이블 생성 시 또는 기존 테이블 수정 시 정의할 수 있습니다.  
  
#### 테이블 생성 시 CHECK 추가  
```sql  
CREATE TABLE table_name (  
    column_name data_type,  
    CONSTRAINT constraint_name CHECK (조건식)  
);  
```  
  
#### ALTER TABLE로 CHECK 추가  
```sql  
ALTER TABLE table_name  
ADD CONSTRAINT constraint_name CHECK (조건식);  
```  
  
---  
  
### 3. CHECK 조건식  
- `CHECK`의 조건식은 **Boolean(참/거짓)**으로 평가됩니다.  
- 조건식이 `TRUE`인 경우에만 데이터가 허용됩니다.  
- 조건식이 `FALSE`이거나 `NULL`인 경우 데이터가 거부됩니다.  
  
---  
  
### 4. 예제  
  
#### (1) 단순 조건  
컬럼 값이 양수여야 한다고 지정:  
```sql  
CREATE TABLE products (  
    product_id SERIAL PRIMARY KEY,  
    price NUMERIC,  
    CONSTRAINT positive_price CHECK (price > 0)  
);  
```  
  
- `price`가 0보다 커야만 데이터가 삽입 또는 수정됩니다.  
- 유효한 데이터:  
  ```sql  
  INSERT INTO products (price) VALUES (100);  
  ```  
- 유효하지 않은 데이터:  
  ```sql  
  INSERT INTO products (price) VALUES (-50);  
  ```  
  오류: **`ERROR: new row for relation "products" violates check constraint "positive_price"`**  
  
---  
  
#### (2) 여러 조건  
특정 컬럼 값이 허용된 범위 안에 있어야 할 경우:  
```sql  
CREATE TABLE employees (  
    employee_id SERIAL PRIMARY KEY,  
    age INT,  
    CONSTRAINT valid_age CHECK (age >= 18 AND age <= 65)  
);  
```  
  
- `age` 값은 **18 이상 65 이하**여야 합니다.  
- 유효한 데이터:  
  ```sql  
  INSERT INTO employees (age) VALUES (25);  
  ```  
- 유효하지 않은 데이터:  
  ```sql  
  INSERT INTO employees (age) VALUES (70);  
  ```  
  오류: **`ERROR: new row for relation "employees" violates check constraint "valid_age"`**  
  
---  
  
#### (3) 특정 값 제한  
컬럼 값이 특정 값 집합 안에 있어야 할 경우:  
```sql  
CREATE TABLE orders (  
    order_id SERIAL PRIMARY KEY,  
    status VARCHAR(20),  
    CONSTRAINT valid_status CHECK (status IN ('pending', 'completed', 'cancelled'))  
);  
```  
  
- `status`는 반드시 `'pending'`, `'completed'`, `'cancelled'` 중 하나여야 합니다.  
- 유효한 데이터:  
  ```sql  
  INSERT INTO orders (status) VALUES ('pending');  
  ```  
- 유효하지 않은 데이터:  
  ```sql  
  INSERT INTO orders (status) VALUES ('in_progress');  
  ```  
  오류: **`ERROR: new row for relation "orders" violates check constraint "valid_status"`**  
  
---  
  
#### (4) 복합 조건  
컬럼 간의 관계를 검사할 수도 있습니다:  
```sql  
CREATE TABLE accounts (  
    account_id SERIAL PRIMARY KEY,  
    balance NUMERIC,  
    minimum_balance NUMERIC,  
    CONSTRAINT valid_balance CHECK (balance >= minimum_balance)  
);  
```  
  
- `balance`는 항상 `minimum_balance` 이상이어야 합니다.  
- 유효한 데이터:  
  ```sql  
  INSERT INTO accounts (balance, minimum_balance) VALUES (500, 300);  
  ```  
- 유효하지 않은 데이터:  
  ```sql  
  INSERT INTO accounts (balance, minimum_balance) VALUES (200, 300);  
  ```  
  오류: **`ERROR: new row for relation "accounts" violates check constraint "valid_balance"`**  
  
---  
  
### 5. CHECK 제약 조건 삭제  
`ALTER TABLE` 명령어를 사용하여 CHECK 제약 조건을 삭제할 수 있습니다.  
  
```sql  
ALTER TABLE table_name  
DROP CONSTRAINT constraint_name;  
```  
  
예:  
```sql  
ALTER TABLE employees  
DROP CONSTRAINT valid_age;  
```  
  
---  
  
### 6. CHECK와 NULL  
- `CHECK` 조건식에서 **NULL 값은 조건식을 평가할 수 없으므로** 기본적으로 조건을 만족하지 못하는 것으로 간주됩니다.  
- 예를 들어:  
  ```sql  
  CREATE TABLE example (  
      value INT,  
      CONSTRAINT non_negative CHECK (value >= 0)  
  );  
  ```  
  여기서 `value`에 `NULL`을 삽입하면 오류 없이 허용됩니다. **왜냐하면 `NULL` 값은 조건을 위반하지 않는 것으로 간주되기 때문**입니다.  
  
---  
  
### 7. CHECK의 사용 제한  
1. **CHECK는 데이터베이스에 강제적인 규칙을 추가하지만, 반드시 필요한 경우에만 사용해야 합니다.**  
   - 너무 많은 CHECK를 추가하면 데이터 삽입/갱신 작업에서 성능 문제가 발생할 수 있습니다.  
2. 데이터 무결성 요구 사항이 애플리케이션 논리로 충분히 처리될 수 있다면 CHECK 대신 애플리케이션에서 처리하는 것도 하나의 방법입니다.  
  
---  
  
### 요약  
- **`CHECK` 제약 조건**은 테이블에 저장되는 데이터가 특정 조건을 만족하는지 확인하여 데이터 무결성을 보장합니다.  
- 테이블 생성 시나 이후 수정 시 추가할 수 있으며, 조건식이 참일 경우에만 데이터 삽입/수정이 허용됩니다.  
- 컬럼 값의 범위, 특정 값 제한, 복합 조건 등 다양한 조건을 정의할 수 있습니다.  
- 잘못된 데이터가 저장되지 않도록 방지할 수 있어 데이터베이스 설계에서 중요한 역할을 합니다.  