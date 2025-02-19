---  
layout: post  
title:  "Schema Design"  
categories: [ PostgreSQL ]  
image: assets/img/20241114_01.png  
tags: [featured]  
---  
  
## SQL Schema Designers  
- dbdiagram.io  
- drawsql.app  
- sqldbm.com  
- quickdatabasediagrams.com  
- ondras.zarovi.cz/sql/demo  
  
## How to Build a 'Like' System  
  
이를 구현하기 위한 데이터베이스 테이블 구조는 일반적으로 **다대다(Many-to-Many)** 관계를 표현. 이는 **하나의 사용자**가 여러 게시물에 좋아요를 누를 수 있고, **하나의 게시물**도 여러 사용자로부터 좋아요를 받을 수 있기 때문  
  
   ```sql  
   CREATE TABLE likes (  
       like_id SERIAL PRIMARY KEY,  
       user_id INT REFERENCES users(user_id) ON DELETE CASCADE,  
       post_id INT REFERENCES posts(post_id) ON DELETE CASCADE,  
       liked_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  
       CONSTRAINT unique_user_post UNIQUE (user_id, post_id)  
   );  
   ```  
  
## How to build a 'Mention' System  
  
인스타그램에서 **사진에 태그를 추가하는 기능**과 **캡션에서 사용자 언급(mention)** 기능은 서로 다른 목적과 관계를 가지므로, 데이터베이스 테이블 구조도 각각의 기능에 맞게 설계해야 합니다.   
  
아래는 두 기능을 효과적으로 구현하기 위한 테이블 구조를 설계한 예시입니다.  
  
---  
  
## 1. **사진에 태그를 추가하는 기능**  
  
### 요구사항  
- 사진에 **다수의 사용자**를 태그할 수 있어야 합니다.  
- 특정 사진에 태그된 사용자 목록을 조회할 수 있어야 합니다.  
- 태그는 사용자와 사진 간의 **다대다(Many-to-Many)** 관계를 표현합니다.  
  
### 테이블 구조  
  
1. **`users`**: 사용자 정보 저장.  
2. **`posts`**: 게시물 정보 저장.  
3. **`photo_tags`**: 사용자와 사진 간의 태그 관계를 저장.  
  
#### 테이블 설계  
```sql  
CREATE TABLE users (  
    user_id SERIAL PRIMARY KEY,  
    username VARCHAR(50) NOT NULL UNIQUE,  
    email VARCHAR(100) NOT NULL UNIQUE  
);  
  
CREATE TABLE posts (  
    post_id SERIAL PRIMARY KEY,  
    user_id INT REFERENCES users(user_id),  
    content TEXT,  
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP  
);  
  
CREATE TABLE photo_tags (  
    tag_id SERIAL PRIMARY KEY,  
    post_id INT REFERENCES posts(post_id) ON DELETE CASCADE,  
    user_id INT REFERENCES users(user_id) ON DELETE CASCADE,  
    tag_position JSONB, -- 태그 위치 저장 (x, y 좌표)  
    tagged_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  
    CONSTRAINT unique_photo_user_tag UNIQUE (post_id, user_id)  
);  
```  
  
### 주요 필드 설명  
- **`photo_tags` 테이블**:  
  - `post_id`: 태그된 사진(게시물) ID.  
  - `user_id`: 태그된 사용자 ID.  
  - `tag_position`: 사진에서 태그의 좌표 정보. 예를 들어, `{ "x": 0.5, "y": 0.3 }` 형식으로 저장 가능.  
  - `tagged_at`: 태그가 추가된 시간.  
  - **`unique_photo_user_tag`**:  
    - 특정 사진에서 동일한 사용자를 중복 태그할 수 없도록 유일성 제약을 추가.  
  
---  
  
## 2. **캡션에서 사용자 언급 기능**  
  
### 요구사항  
- 캡션에 작성된 텍스트에서 여러 사용자를 **언급(mention)**할 수 있어야 합니다.  
- 특정 게시물에서 언급된 사용자 목록을 조회할 수 있어야 합니다.  
- 언급 기능은 게시물과 사용자 간의 **다대다(Many-to-Many)** 관계로 구현됩니다.  
  
### 테이블 구조  
  
1. **`users`**: 사용자 정보 저장 (위에서 동일).  
2. **`posts`**: 게시물 정보 저장 (위에서 동일).  
3. **`mentions`**: 게시물과 사용자 간의 언급 관계 저장.  
  
#### 테이블 설계  
```sql  
CREATE TABLE mentions (  
    mention_id SERIAL PRIMARY KEY,  
    post_id INT REFERENCES posts(post_id) ON DELETE CASCADE,  
    user_id INT REFERENCES users(user_id) ON DELETE CASCADE,  
    mentioned_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  
    CONSTRAINT unique_post_user_mention UNIQUE (post_id, user_id)  
);  
```  
  
### 주요 필드 설명  
- **`mentions` 테이블**:  
  - `post_id`: 언급이 포함된 게시물 ID.  
  - `user_id`: 언급된 사용자 ID.  
  - `mentioned_at`: 언급이 기록된 시간.  
  - **`unique_post_user_mention`**:  
    - 동일한 게시물에서 동일한 사용자가 여러 번 언급되는 것을 방지.  
  
---  
  
## 3. **두 기능의 데이터 흐름**  
  
### 데이터 삽입 예제  
  
#### 사진에 태그 추가  
1. 사용자가 사진에서 다른 사용자를 태그:  
   ```sql  
   INSERT INTO photo_tags (post_id, user_id, tag_position)  
   VALUES (1, 2, '{"x": 0.5, "y": 0.3}');  
   ```  
  
2. 특정 사진에서 태그된 사용자 목록 조회:  
   ```sql  
   SELECT u.username, pt.tag_position  
   FROM photo_tags pt  
   JOIN users u ON pt.user_id = u.user_id  
   WHERE pt.post_id = 1;  
   ```  
  
#### 캡션에서 언급 추가  
1. 게시물 캡션에서 사용자를 언급:  
   ```sql  
   INSERT INTO mentions (post_id, user_id)  
   VALUES (1, 3);  
   ```  
  
2. 특정 게시물에서 언급된 사용자 목록 조회:  
   ```sql  
   SELECT u.username  
   FROM mentions m  
   JOIN users u ON m.user_id = u.user_id  
   WHERE m.post_id = 1;  
   ```  
  
---  
  
## 4. 확장 가능성  
  
### 사진 태그 확장  
1. **알림(Notification)**:  
   태그된 사용자에게 알림을 생성하는 시스템 추가.  
   ```sql  
   INSERT INTO notifications (user_id, type, post_id, created_at)  
   VALUES (2, 'photo_tag', 1, CURRENT_TIMESTAMP);  
   ```  
  
2. **태그 삭제**:  
   태그를 삭제하려면 `photo_tags` 테이블에서 해당 행을 삭제.  
   ```sql  
   DELETE FROM photo_tags WHERE post_id = 1 AND user_id = 2;  
   ```  
  
---  
  
### 캡션 언급 확장  
1. **캡션 분석**:  
   캡션에서 텍스트를 파싱하여 `@username` 형식을 자동으로 `mentions` 테이블에 기록.  
  
   예:  
   ```sql  
   -- 파싱 결과  
   INSERT INTO mentions (post_id, user_id)  
   SELECT 1, user_id FROM users WHERE username = 'alice';  
   ```  
  
2. **언급 삭제**:  
   특정 게시물에서 언급을 삭제하려면:  
   ```sql  
   DELETE FROM mentions WHERE post_id = 1 AND user_id = 3;  
   ```  
  
---  
  
## 5. 최적화 및 고려사항  
1. **인덱스 추가**:  
   - 대규모 데이터에서 `post_id`, `user_id` 검색 속도를 높이기 위해 인덱스를 추가.  
   ```sql  
   CREATE INDEX idx_photo_tags_post_id ON photo_tags(post_id);  
   CREATE INDEX idx_mentions_post_id ON mentions(post_id);  
   ```  
  
2. **제약 조건**:  
   - 태그 및 언급 시 동일한 사용자와 게시물 간 중복을 방지하기 위해 `UNIQUE` 제약 조건 활용.  
  
3. **데이터 삭제 연쇄**:  
   - 게시물 삭제 시 관련 태그와 언급 데이터도 자동으로 삭제되도록 `ON DELETE CASCADE` 설정.  
  
---  
  
## 요약  
  
| 기능          | 테이블         | 주요 컬럼                     | 설명                                           |  
|---------------|----------------|-------------------------------|-----------------------------------------------|  
| **사진 태그** | `photo_tags`   | `post_id`, `user_id`, `tag_position` | 사용자와 사진 간 태그 정보 저장               |  
| **캡션 언급** | `mentions`     | `post_id`, `user_id`          | 게시물 캡션에서 언급된 사용자 정보 저장        |  
  
위 테이블 구조는 **사진 태그**와 **캡션 언급**을 효율적으로 관리하면서, 확장성과 성능 최적화를 고려하여 설계되었습니다.  
  
  
## How to Build a 'Hashtag' System  
  
인스타그램의 **해시태그**는 게시물에 추가할 수 있는 텍스트 기반의 태그로, 특정 주제를 중심으로 게시물을 검색하거나 그룹화하는 데 사용됩니다. 데이터베이스에서 해시태그를 구현하려면 해시태그와 게시물 간의 **다대다(Many-to-Many)** 관계를 관리하는 테이블 구조를 설계해야 합니다.  
  
---  
  
### 1. 요구사항  
1. **해시태그 데이터 관리**:  
   - 해시태그는 고유해야 하며, 중복된 해시태그를 허용하지 않아야 합니다.  
2. **게시물-해시태그 관계**:  
   - 하나의 게시물에 여러 해시태그를 추가할 수 있습니다.  
   - 하나의 해시태그는 여러 게시물에서 사용될 수 있습니다.  
3. **해시태그 검색**:  
   - 특정 해시태그를 검색하면 해당 해시태그가 달린 모든 게시물을 빠르게 조회할 수 있어야 합니다.  
4. **해시태그 사용 빈도 추적** (선택 사항):  
   - 각 해시태그가 얼마나 자주 사용되는지 통계를 관리할 수 있어야 합니다.  
  
---  
  
### 2. 테이블 구조 설계  
  
#### (1) `hashtags` 테이블  
- 해시태그 정보를 저장하는 테이블입니다.  
- 해시태그는 고유해야 하므로, 중복 삽입을 방지하기 위해 `UNIQUE` 제약 조건을 설정합니다.  
  
```sql  
CREATE TABLE hashtags (  
    hashtag_id SERIAL PRIMARY KEY,  
    name VARCHAR(255) NOT NULL UNIQUE,  
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP  
);  
```  
  
- **주요 컬럼**:  
  - `hashtag_id`: 해시태그의 고유 ID.  
  - `name`: 해시태그 텍스트 (예: `#travel`).  
  - `created_at`: 해시태그가 처음 생성된 시간.  
  
---  
  
#### (2) `post_hashtags` 테이블  
- 게시물과 해시태그 간의 관계를 관리하는 **교차 테이블**입니다.  
- `post_id`와 `hashtag_id`의 조합은 고유해야 하며, 이를 위해 `UNIQUE` 제약 조건을 설정합니다.  
  
```sql  
CREATE TABLE post_hashtags (  
    post_hashtag_id SERIAL PRIMARY KEY,  
    post_id INT REFERENCES posts(post_id) ON DELETE CASCADE,  
    hashtag_id INT REFERENCES hashtags(hashtag_id) ON DELETE CASCADE,  
    tagged_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,  
    CONSTRAINT unique_post_hashtag UNIQUE (post_id, hashtag_id)  
);  
```  
  
- **주요 컬럼**:  
  - `post_hashtag_id`: 관계의 고유 ID.  
  - `post_id`: 게시물의 ID (`posts` 테이블과 외래 키 관계).  
  - `hashtag_id`: 해시태그의 ID (`hashtags` 테이블과 외래 키 관계).  
  - `tagged_at`: 해시태그가 게시물에 추가된 시간.  
  - **`unique_post_hashtag`**:  
    - 동일 게시물에서 동일 해시태그가 중복으로 추가되지 않도록 보장.  
  
---  
  
### 3. 데이터 흐름 예시  
  
#### 데이터 삽입  
  
1. **새로운 해시태그 추가**  
   ```sql  
   INSERT INTO hashtags (name) VALUES ('travel'), ('food'), ('photography');  
   ```  
  
2. **게시물과 해시태그 연결**  
   게시물 ID가 1인 게시물에 `#travel`과 `#food` 해시태그를 추가하려면:  
   ```sql  
   INSERT INTO post_hashtags (post_id, hashtag_id)  
   SELECT 1, hashtag_id FROM hashtags WHERE name IN ('travel', 'food');  
   ```  
  
---  
  
#### 데이터 조회  
  
1. **특정 게시물에 달린 해시태그 조회**  
   ```sql  
   SELECT h.name  
   FROM post_hashtags ph  
   JOIN hashtags h ON ph.hashtag_id = h.hashtag_id  
   WHERE ph.post_id = 1;  
   ```  
  
2. **특정 해시태그가 사용된 게시물 조회**  
   `#travel` 해시태그가 사용된 모든 게시물을 조회:  
   ```sql  
   SELECT p.post_id, p.content  
   FROM post_hashtags ph  
   JOIN posts p ON ph.post_id = p.post_id  
   JOIN hashtags h ON ph.hashtag_id = h.hashtag_id  
   WHERE h.name = 'travel';  
   ```  
  
3. **해시태그 사용 빈도 조회**  
   가장 많이 사용된 해시태그를 찾으려면:  
   ```sql  
   SELECT h.name, COUNT(*) AS usage_count  
   FROM post_hashtags ph  
   JOIN hashtags h ON ph.hashtag_id = h.hashtag_id  
   GROUP BY h.name  
   ORDER BY usage_count DESC;  
   ```  
  
---  
  
### 4. 고려사항 및 확장  
  
#### (1) 성능 최적화  
- **인덱스 추가**:  
  - `hashtags(name)`와 `post_hashtags(post_id, hashtag_id)`에 인덱스를 추가하면 해시태그 검색과 연결 작업이 더 빨라집니다.  
  ```sql  
  CREATE INDEX idx_hashtag_name ON hashtags(name);  
  CREATE INDEX idx_post_hashtag ON post_hashtags(post_id, hashtag_id);  
  ```  
  
#### (2) 해시태그 정규화  
- 해시태그는 대소문자를 구분하지 않고 저장하거나 검색해야 하는 경우가 많습니다.  
- 예: `#Travel`과 `#travel`은 동일하게 간주.  
- 이를 위해 `LOWER()` 함수로 입력된 해시태그를 소문자로 변환하여 저장:  
  ```sql  
  INSERT INTO hashtags (name)  
  VALUES (LOWER('Travel'));  
  ```  
  
#### (3) 태그 삭제 처리  
- 게시물이 삭제될 경우 관련된 태그도 함께 삭제되도록 `ON DELETE CASCADE`를 설정합니다.  
- 해시태그 자체는 삭제되지 않으므로, 사용되지 않는 해시태그를 정리하는 별도의 작업이 필요할 수 있습니다.  
  
---  
  
### 5. 최종 테이블 구조 요약  
  
#### `hashtags` 테이블  
| 컬럼 이름     | 데이터 타입   | 설명                      |  
|---------------|---------------|---------------------------|  
| `hashtag_id`  | SERIAL        | 해시태그 고유 ID          |  
| `name`        | VARCHAR(255)  | 해시태그 텍스트           |  
| `created_at`  | TIMESTAMP     | 해시태그 생성 시간         |  
  
#### `post_hashtags` 테이블  
| 컬럼 이름         | 데이터 타입   | 설명                                   |  
|-------------------|---------------|----------------------------------------|  
| `post_hashtag_id` | SERIAL        | 관계의 고유 ID                         |  
| `post_id`         | INT           | 게시물 ID (`posts`와 외래 키 관계)     |  
| `hashtag_id`      | INT           | 해시태그 ID (`hashtags`와 외래 키 관계)|  
| `tagged_at`       | TIMESTAMP     | 태그가 게시물에 추가된 시간            |  
  
---  
  
### 요약  
- **해시태그 저장**: `hashtags` 테이블에서 해시태그 텍스트를 고유하게 저장.  
- **게시물-해시태그 관계**: `post_hashtags` 테이블에서 게시물과 해시태그 간 다대다 관계를 관리.  
- **확장 가능성**: 해시태그 사용 빈도, 정규화, 인덱스 최적화 등을 통해 성능 개선 가능.  
  
이 구조는 대규모의 해시태그와 게시물을 효과적으로 관리하면서 빠른 검색과 확장을 지원합니다.  


