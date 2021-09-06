---
title: "[쿼리 최적화] 보다 빠른 쿼리를 위한 체크 리스트"
categories: DB
comments: true
---

# 쿼리 최적화
 방대한 양의 데이터가 존재할 경우 DB에 있는 데이터를 효과적인 쿼리를 통해 최적화 하여 성능을 향상시켜야 한다.


## 임의의 데이터셋

```sql
# movie (약 100개의 레코드)
 -- id(int): primary key
 -- title(varchar): 영화 제목

# rating (약 10,000,000개의 레코드)
 -- id(int): primary key
 -- movie_id(int): 별점의 대상이 되는 movie id
 -- value(int): 별점의 값

# genre (약 100,000개의 레코드)
 -- id(int): primary key
 -- movie_id(int): 별점의 대상이 되는 movie id
 -- genre(varchar): 장르명
```

## Check List✅

### 1. SELECT 시에는 꼭 필요한 컬럼만 불러오기
 - 많은 필드 값을 불러올수록 DB는 더 많은 로드를 부담
 - COLUMN 중 불필요한 값은 제외, 필요한 열만 불러오기

```sql
 -- Inefficient
  SELECT * FROM movie;

 -- Improved
  SELECT id FROM movie;
```

### 2. 조건 부여 시, 가급적이면 기존 DB값에 별도의 연산을 걸지 않는 것이 좋음
 - Inefficient 
   cell값을 탐색 -> 수식(연산) -> 조건충족여부 판단

 - Improved
   r.value가 가지고 있는 index를 그대로 활용할 수 있기 때문에 모든 필드 값을 탐색할 필요가 없어 Inefficient 쿼리 대비 짧은 Running Time을 가짐

```sql
 -- Inefficient
  SELECT m.id, ANY_VALUE(m.title) title, COUNT(r.id) r_count 
  FROM movie m 
  INNER JOIN rating r 
  ON m.id = r.movie_id 
  WHERE FLOOR(r.value/2) = 2 
  GROUP BY m.id;

 -- Improved
  SELECT m.id, ANY_VALUE(m.title) title, COUNT(r.id) r_count 
  FROM movie m 
  INNER JOIN rating r 
  ON m.id = r.movie_id 
  WHERE r.value BETWEEN 4 AND 5 
  GROUP BY m.id;
```

### 3. LIKE사용 시 와일드카드 문자열(%)을 String 앞부분에는 배치하지 않는 것이 좋음
 - 다양한 장르 중에서 Comedy와 Romantic Comedy를 추출하고 싶은 경우, LIKE "%Comedy"보다는, 다른 형태의 조건절을 사용하는 것이 효과적

```sql
 -- Inefficient
  SELECT g.value genre, COUNT(r.movie_id) r_cnt 
  FROM rating r 
  INNER JOIN genre g 
  ON r.movie_id = g.movie_id 
  WHERE g.value LIKE "%Comedy"  
  GROUP BY g.value;

 -- Improved(1): value IN (...)
  SELECT g.value genre, COUNT(r.movie_id) r_cnt 
  FROM rating r 
  INNER JOIN genre g 
  ON r.movie_id = g.movie_id 
  WHERE g.value IN ("Romantic Comedy", "Comedy") 
  GROUP BY g.value;

 -- Improved(2): value = "..."
  SELECT g.value genre, COUNT(r.movie_id) r_cnt 
  FROM rating r 
  INNER JOIN genre g 
  ON r.movie_id = g.movie_id 
  WHERE g.value = "Romantic Comedy" OR g.value = "Comedy"
  GROUP BY g.value;

 -- Improved(3): value LIKE "...%"
 -- 모든 문자열을 탐색할 필요가 없어, 가장 좋은 성능을 내었습니다
  SELECT g.value genre, COUNT(r.movie_id) r_cnt 
  FROM rating r 
  INNER JOIN genre g 
  ON r.movie_id = g.movie_id 
  WHERE g.value LIKE "Romantic%" OR g.value LIKE "Comed%"
  GROUP BY g.value;
```

### 4. SELECT DISTINCT, UNION DISTINCT와 같이 중복 값을 제거하는 연산은 최대한 사용X
 - 중복 값을 제거하는 연산은 많은 시간이 걸림 불가피하게 사용해야 하는 상황이라면 DISTINCT연산을 대체하거나, 연산의 대상이 되는 테이블의 크기를 최소화 하는 방법을 찾아야함
 - 대표적인 대체 방법: `EXISTS` 활용

```sql
 -- Inefficient
  SELECT DISTINCT m.id, title 
  FROM movie m  
  INNER JOIN genre g 
  ON m.id = g.movie_id;

 -- Improved
  SELECT m.id, title 
  FROM movie m  
  WHERE EXISTS (SELECT 'X' FROM rating r WHERE m.id = r.movie_id);
```


### 5. 같은 내용의 조건이라면, GROUP BY 연산시에는 HAVING 보다는 WHERE절이 좋음
 - 쿼리 실행 순서에서, WHERE 절이 HAVING절보다 먼저 실행됨, 따라서 WHERE절로 미리 데이터 크기를 작게 만들면 GROUP BY에서 다뤄야 하는 크기가 작아지기 때문에 보다 효율적인 연산가능

```sql
 -- Inefficient
  SELECT m.id, COUNT(r.id) AS rating_cnt, AVG(r.value) AS avg_rating 
  FROM movie m  
  INNER JOIN rating r 
  ON m.id = r.movie_id 
  GROUP BY id 
  HAVING m.id > 1000;

 -- Improved
  SELECT m.id, COUNT(r.id) AS rating_cnt, AVG(r.value) AS avg_rating 
  FROM movie m  
  INNER JOIN rating r 
  ON m.id = r.movie_id 
  WHERE m.id > 1000
  GROUP BY id ;
```

### 6. 3개 이상의 테이블을 INNER JOIN할 때는, 크기가 가장 큰 테이블을 FROM절에 배치하고, INNER JOIN절에는 남은 테이블을 작은 순서대로 배치하는 것이 좋음
 - 항상 통용되지는 않음
 - 간단한 INNER JOIN의 경우는 대부분 Query Planner에서 가장 효과적인 순서를 탐색해 INNER JOIN의 순서를 바꿔서 실행 시간에는 차이가 없음
 - But, 테이블의 개수가 늘어난다면, 탐색해야 할 INNER JOIN 순서의 경우의 수가 늘어나고, Planning 비용의 증가로 이어짐 
   -> 최적의 순서보다는 차선의 순서로 실행하더라도 Planning 비용을 줄이는 것이 더 효과적
 - INNER JOIN 최적화 여부가 연산량에 미치는 영향은 상당히 크다.

```sql
 -- Query (A)
  SELECT m.title, r.value rating, g.value genre 
  FROM rating r 
  INNER JOIN genre g 
  ON g.movie_id = r.movie_id  
  INNER JOIN movie m 
  ON m.id = r.movie_id;

 -- Query (B)
  SELECT m.title, r.value rating, g.value genre 
  FROM rating r 
  INNER JOIN movie m
  ON r.movie_id = m.id 
  INNER JOIN genre g 
  ON r.movie_id = g.movie_id;
```

### 7. 자주 사용하는 데이터의 형식에 대해서는 미리 전처리된 테이블을 따로 보관/관리하는 것도 좋음
 - RDBMS의 원칙에 어긋나고, DB의 실시간성을 반영하지 못할 가능성이 높아 대부분 분석계에서 사용
 - 사용자에 의해 발생한 Log 데이터 중에서 필요한 Event, 핵심 서비스 지표를 주기적으로 계산해서 따로 적재(모아)해두는것


## 정리
 - 이 외에도 `ORDER BY는 연산 중간에 사용하지 않기`, `LIMIT 활용하기`등의 습관에 가까운 것등이 있습니다.

## 느낀점
 실제 업무에서 QUERY문을 직접 작성하고 있는데 참고하고 있는 이미 제작된 query들에는 이미 사용하고 있는 쿼리 최적화 방법도 있었다는게 신기했다!
 그냥 얼레벌레 코드 작성하는줄 알았더니 스니 생각보다 똑똑해~~😍  
 몰랐던 건 생각나면 언젠간 적용해보는걸로~~  
 특히 1번은 회사 들어오고 초반부터 굳이 *로 다 조회하지 않고 컬럼을 하나하나 쓰라고 하는 이유가 있나 했는데 최적화 때문이라니!! 똑똑하구만~.~!


참고자료 : [WATACHA 팀 블로그: 쿼리 최적화: 빠른 쿼리를 위한 7가지 체크리스트](https://medium.com/watcha/%EC%BF%BC%EB%A6%AC-%EC%B5%9C%EC%A0%81%ED%99%94-%EC%B2%AB%EA%B1%B8%EC%9D%8C-%EB%B3%B4%EB%8B%A4-%EB%B9%A0%EB%A5%B8-%EC%BF%BC%EB%A6%AC%EB%A5%BC-%EC%9C%84%ED%95%9C-7%EA%B0%80%EC%A7%80-%EC%B2%B4%ED%81%AC-%EB%A6%AC%EC%8A%A4%ED%8A%B8-bafec9d2c073)