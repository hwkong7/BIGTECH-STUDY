# 서브 쿼리와 조인

#### 서브 쿼리(subquery)

- 다른 SQL문이 포함된 SQL문을 의미

#### 조인(join)

- 2개의 테이블을 하나로 합치는 것

⇒ 둘 다 여러 테이블에 질의하거나 복잡한 요청을 처리할 때 사용

## 여러 테이블에 질의하기

```sql
SELECT 테이블1.필드1, 테이블1.필드2, 테이블2.필드3
	FROM 테이블1, 테이블2
	WHERE 테이블1.필드1 = 테이블2.필드2;
```

- 여러 테이블에서 원하는 데이터를 함께 조회할 때 사용

## 서브 쿼리(subquery)

- MySQL 공식 문서 기준) 다른 SQL문 안에 들어 있는 `SELECT`문
- 소괄호 `()`로 감싸 외부 쿼리와 구분
- `SELECT`, `INSERT`, `UPDATE`, `DELETE`문 안에 포함될 수 있음
  외부 쿼리 | 서브 쿼리
  ```sql
  **SELECT**
  	users.username,
  	(**SELECT** COUNT(*)
  	FORM posts
  	WHERE posts.user_id = users.user_id) AS post_count
  FROM users;
  ```
  ```sql
  **DELETE** FROM posts,
  WHERE user_id = (
  	**SELECT** user_id
  	FROM users
  	WHERE email = 'kim@example.com'
  );
  ```

## 조인(join)

<aside>
💡

여러 테이블을 하나로 합치는 것

</aside>

- 서브 쿼리보다 가독성이 좋고, 여러 테이블 관계를 명확히 표현하기 쉬움

### 조인의 종류

![image.png](attachment:5830224f-8c8d-4a33-bff6-2c925242d5a9:image.png)

### INNER JOIN

<aside>
💡

- 가장 일반적인 조인
- 두 테이블에서 **조인 조건이 일치하는 행만** 조회
</aside>

```sql
SELECT 필드
FROM 테이블1
	INNER JOIN 테이블2 ON 조인 조건;
```

- `INNER` 생략가능

### OUTER JOIN

#### LEFT OUTER JOIN

<aside>
💡

- 왼쪽 테이블의 모든 행 유지
- 오른쪽 테이블에 대응되는 값이 없으면 `NULL`
</aside>

```sql
SELECT 필드
FROM 테이블1
	LEFT OUTER JOIN 테이블2 ON 조인 조건;
```

#### RIGHT OUTER JOIN

<aside>
💡

- 오른쪽 테이블의 모든 행 유지
- 왼쪽 테이블에 대응되는 값이 없으면 `NULL`
</aside>

```sql
SELECT 필드
FROM 테이블1
	RIGHT OUTER JOIN 테이블2 ON 조인 조건;
```

#### FULL OUTER JOIN

<aside>
💡

- 양쪽 테이블의 모든 레코드 조회
- 매칭되지 않는 부분은 `NULL`로 표시
- MySQL을 포함한 많은 RDBMS는 `FULL OUTER JOIN` 문법을 직접 지원하지 않음
- 대신 `LEFT JOIN` + `RIGHT JOIN` 결과를 `UNION`으로 합쳐 구현 가능
- `UNION` : 여러 SQL 실행 결과의 합집합을 구하는 키워드
</aside>

```sql
SELECT 필드
FROM 테이블1
	LEFT JOIN 테이블2 ON 조인 조건
	UNION
SELECT 필드
FROM 테이블1
	RIGHT JOIN 테이블2 ON 조인 조건;
```

<aside>
🗣

**조인과 서브쿼리 차이**

- 조인은 테이블을 연결해서 한 번에 조회
- 서브쿼리는 쿼리 안에 또 다른 쿼리를 넣어 조건이나 결과를 활용
</aside>

# 뷰

<aside>
💡

SELECT문의 결과로 만들어진 가상의 테이블

</aside>

## 뷰 생성

```sql
CREATE VIEW 뷰_이름 AS SELECT문;
```

## 뷰 삭제

```sql
DROP VIEW 뷰_이름;
```

## 뷰 특징

- SQL문을 단순화할 때 사용
- 특정 사용자에게 테이블의 일부 데이터만 보여주고 싶을 때 사용
- 원본 테이블 전체를 노출하지 않고, 필요한 데이터만 보여주는 용도로 유용
- 특정 사용자에게 뷰에 대한 접근 권한만 줄 수도 있음
  => `GRANT` 활용

<aside>
⚠️

## 주의점

- `SELECT`는 보통 가능
- 하지만 `INSERT`, `UPDATE`, `DELETE`는 불가능할 수도 있음
- 특히 여러 테이블을 조합한 뷰는 삽입/수정/삭제 시 제약조건을 어기기 쉬움

=> 그래서 뷰는 **조회 목적**으로 많이 사용됨

</aside>

---

# 인덱스

<aside>
💡

- RDBMS 성능 향상 방법 중 가장 대표적
- 레코드 수가 많고 조회가 자주 일어나는 환경에서 많이 사용
- 검색 속도 향상을 위해 테이블의 하나 이상의 필드에 만드는 자료구조
</aside>

![image.png](attachment:ad75b541-7d68-4c04-b1ba-83a476a4a9c9:image.png)

## 장점

- 조회 성능 향상
- 인덱스가 없으면 최악의 경우 모든 레코드를 전부 확인해야 함
  => Full Scan 발생 가능

## 단점

- 인덱스를 너무 많이 만들면 오히려 성능 저하
- 저장 공간 추가 필요
- 생성 시간도 필요
- `INSERT`, `UPDATE`, `DELETE` 시 인덱스도 함께 갱신해야 함
  => 쓰기 성능 저하 가능

## 특징

- 테이블(필드)에 종속된 개념
- 테이블이 삭제되면 관련 인덱스도 함께 삭제됨
- 한 테이블에는 여러 인덱스를 만들 수 있음

![인덱스를 테이블의 형태로 표현
실제는 B 트리(혹은 B 트리의 변형) 형태를 띈다.](attachment:6af244fd-947d-46f9-ad5a-38ce52cbc695:image.png)

인덱스를 테이블의 형태로 표현
실제는 B 트리(혹은 B 트리의 변형) 형태를 띈다.

## 인덱스 생성 / 조회 / 삭제

```sql
CREATE INDEX 인덱스_이름ON 테이블_이름 (필드);

SHOW INDEXFROM 테이블_이름;

DROP INDEX 인덱스_이름FROM 테이블_이름;
```

### 클러스터형 인덱스(clustered index)

<aside>
💡

테이블당 하나만 만들 수 있는 인덱스

</aside>

- 기본키(`PRIMARY KEY`)는 기본적으로 클러스터형 인덱스로 간주
- 기본키가 없으면 `NOT NULL` + `UNIQUE` 조건을 가진 필드가 클러스터형 인덱스로 간주될 수 있음

### 세컨더리 인덱스(secondary index) = 논클러스터형 인덱스(non-clustered index)

<aside>
💡

클러스터형 인덱스가 아닌 인덱스

</aside>

- 테이블당 여러 개 생성 가능
- 일반적으로 클러스터형 인덱스를 활용한 검색보다 느림

```sql
-- '테이블_이름'의 '필드'에 세컨더리 인덱스인 '인덱스_이름'을 생성
CREATE INDEX 인덱스_이름 ON 테이블_이름 (필드);

-- 인덱스 조회
SHOW INDEX FROM 테이블_이름;

-- 인덱스 삭제
DROP INDEX 인덱스_이름 FROM 테이블_이름;
```

## 인덱스로 사용되는 자료구조

### B 트리(혹은 B+트리와 같은 B 트리의 변형)

- 인덱스는 실제로 보통 B 트리 계열 구조로 구현됨
- 정렬된 상태를 유지하며 탐색, 삽입, 삭제를 효율적으로 처리

![제품 테이블](attachment:8bbea209-0427-4f14-8811-1680afab69a0:image.png)

제품 테이블

![B 트리](attachment:2da2da2d-6481-49f5-bf31-311afcefce03:image.png)

B 트리

## 단점

- 저장 공간이 추가로 필요
- 인덱스 생성 시간이 듦
- 조회 성능은 좋아질 수 있음
- 하지만 `INSERT`, `UPDATE`, `DELETE` 성능은 오히려 떨어질 수 있음
  => 인덱스 유지/갱신 작업이 추가되기 때문

## 어떻게 활용?

- 데이터가 충분히 많은 테이블에 사용
- 조회가 자주 일어나는 필드에 사용
- 특히 `WHERE`, `ORDER BY`, `JOIN`에서 자주 쓰이는 필드가 적합
- 보통 테이블당 3개 이하 권고
- 중복값이 너무 많은 필드는 인덱스 효율이 떨어짐
  => 어차피 많은 레코드를 다시 확인해야 하기 때문

<aside>
🗣

## 뷰 vs 인덱스

- 뷰
  - `SELECT` 결과로 만든 가상의 테이블
  - 보안, 조회 단순화 목적
- 인덱스 - 검색 속도 향상을 위한 자료구조 - 성능 최적화 목적
</aside>
