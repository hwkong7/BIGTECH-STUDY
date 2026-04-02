## 서브 쿼리

> 내부에 다른 SQL문이 포함되어 있는 SQL문(= 다른 SQL문 안에 있는 SELECT문)
> 
- 하나의 `SELECT`문으로 여러 테이블의 레코드 조회 가능
- 서브쿼리의 유형
    - `SELECT`문 안에 `SELECT`문이 포함된 서브 쿼리
        - ex. 사용자별로 작성한 글의 개수 조회
            
            ```sql
            SELECT
            	users.username,
            	(SELECT COUNT(*)
            	FROM posts
            	WHERE posts.user_id = users.user_id) AS post_count
            FROM users;
            
            // 조인 연산 적용
            SELECT users.username, COUNT(posts.post_id) AS post_count
            FROM users
            	LEFT JOIN posts ON user.user_id = posts.user_id
            	GROUP BY users.username;
            ```
            
    - `DELETE`문 안에 `SELECT`문이 포함된 서브 쿼리
        - ex. email이 kim@example.com인 사용자의 글을 삭제하는 SQL문
            
            ```sql
            DELETE FROM posts
            WHERE user_id = (
            	SELECT user_id
            	FROM users
            	WHERE email = 'kim@example.com'
            );
            ```
            

---

## 조인

> 여러 테이블을 하나로 합치는 것
> 
- 서브 쿼리의 단점: SQL문이 너무 복잡해질 수 있음.
- 종류
    
    ![image.png](attachment:366009bc-738c-4dc4-bd4f-f564d806633b:image.png)
    

![image.png](attachment:e17af84a-424c-4fe5-adee-1a09a811fa54:image.png)

**INNER 조인**

```sql
SELECT 필드
FROM 테이블1
	INNER JOIN 테이블2 ON 조인 조건;
```

- 테이블1과 테이블2의 레코드 중에서 조인 조건을 모두 만족하는 데이터만을 선택해 결합하는, 일종의 교집합 연산

```sql
SELECT customers.name, customers.age, customers.email, orders.id, orders.product_id, orders.quantity, orders.amount
FROM customers
	INNER JOIN orders ON customers.id = orders.customer_id;
```

⇒ 결과

![image.png](attachment:de7abad0-b38c-4fa8-94fe-578e761f4f7e:image.png)

**LEFT OUTER 조인**

```sql
SELECT 필드
FROM 테이블1
	LEFT OUTER JOIN 테이블2 ON 조인 조건;
```

- 테이블1의 모든 레코드를 기준으로 테이블2의 레코드를 합치되, 테이블2에 대응되는 레코드가 없다면 해당 값을 `NULL`로 간주

```sql
SELECT customers.name, orders.id AS order_id, orders.product_id, orders.quantity, orders.amount
FROM customers
	LEFT OUTER JOIN order ON customers.id = orders.customer_id;
```

⇒ 결과

![image.png](attachment:fc0d9b64-09d2-4529-966d-0abee5ee8293:image.png)

**RIGHT OUTER 조인**

```sql
SELECT 필드
FROM 테이블1
	RIGHT OUTER JOIN 테이블2 ON 조인 조건;
```

- 테이블2의 레코드를 모두 선택하고, 이를 기준으로 테이블1를 합치되 대응되는 레코드가 없다면 `NULL`이 되는 조인 방식

```sql
SELECT customers.name, orders.id AS order_id, orders.product_id, orders.quantity, orders.amount
FROM customers
	RIGHT OUTER JOIN orders ON customers.id = orders.customer_id
```

⇒ 결과

![image.png](attachment:63813546-c7af-47c5-bb5d-c044ca3b9b7e:image.png)

**FULL OUTER 조인**

```sql
SELECT 필드
FROM 테이블1
	LEFT JOIN 테이블2 ON 조인 조건
	UNION
SELECT 필드
FROM 테이블1
	RIGHT JOIN 테이블2 ON 조인 조건;
```

- 두 테이블의 모든 레코드를 선택하되, 대응되지 않는 모든 레코드를 NULL로 표기
- `LEFT OUTER` 조인과 `RIGHT OUTER` 조인의 결과를 하나로 합친 것
    - 합칠 때는 `UNION` 키워드 사용

```sql
SELECT customers.name, orders.id AS order_id, orders.product_id, orders.quantity, orders.amount
FROM customers
	LEFT OUTER JOIN orders ON customers.id = orders.customer_id
	UNION
SELECT customers.name, orders.id AS order_idm orders.product_id, orders.quantity, orders.amount
FROM customers
	RIGHT OUTER JOIN orders ON customers.id = orders.customer_id;
```

⇒ 결과

![image.png](attachment:219c40f1-52cf-48a5-8081-a798f1f776f7:image.png)

---

## 뷰

> SELECT문의 결과로 만들어진 가상의 테이블
> 
- `SELECT`문의 결과를 뷰로 생성한 뒤, 해당 뷰에 다양한 SQL문 실행 가능
- 여러 테이블을 조인하거나 복잡한 조건식을 사용한 SQL문을 하나의 뷰로 만들어 두면, 이후 복잡한 SQL문을 반복적으로 작성하는 대신에 보다 단순하게 동일한 결과를 얻을 수 있음.
- 뷰**를 사용하는 이유**
    - 테이블에 대한 SQL문을 단순화하고 재사용성을 높이기 위해
    - ex. 특정 사용자에게 테이블의 특정 데이터만을 보여주고자 할 때
        - 테이블 상의 노출 가능한 데이터만을 포함하는 뷰를 만들어 특정 사용자에게 해당 뷰에 대한 접근 권한을 부여하면 됨.
- **뷰 사용 시 유의할 점**
    - VIEW에 대한 조회(`SELECT`)에는 제한이 없지만, 삽입(`INSERT`), 수정(`UPDATE`), 삭제(`DELETE`) 등이 불가능할 수 있음.
        - Why? → 삽입/수정/삭제 연산이 제약 조건을 어기기 쉽기 때문

---

## 인덱스

> 검색 속도 향상을 목적으로 만드는 하나 이상의 테이블 필드에 대한 자료구조
> 
- RDBMS의 성능을 향상시키는 가장 대중적인 방법
    - 특정 필드에 대한 인덱스 생성 → 인덱스를 기준으로 원하는 레코드에 더 빠르게 접근 가능
- 인덱스가 없다면?
    - 최악의 경우 원하는 레코드를 조회하기 위해 모든 레코드를 찾아봐야 함.
- 한 테이블에 2개의 인덱스 존재 가능
    - 일반적으로는 테이블당 3개 이하의 인덱스 권고
    - 중복되는 데이터가 많은 필드에 대해 인덱스를생성하는 것은 인덱스의 효능을 떨어뜨림.
- 인덱스는 테이블(필드)에 종속된 개념이기 때문에 테이블이 삭제되면 테이블과 관련된 인덱스는 모두 삭제됨.
- 예시
    - ‘학번’필드에 인덱스를 생성 → 데이터베이스 내부에 ‘학번’필드를 기준으로 정렬된 인덱스 자료구조가 생성됨.
    
    ![image.png](attachment:1ac44262-3b9a-4629-a3a9-ea031cbae1a7:image.png)
    

**인덱스로 사용되는 자료구조**

- 해시 테이블
- B 트리
    - 각 노드에 키 == 인덱스 값
        - 인덱스 값을 탐색하면 실제 데이터(레코드)가 저장된 위치를 알 수 있음.
    - B트리의 특성상 다량의 노드에 대한 빠른 검색이 가능하기 때문에 인덱스를 바탕으로 레코드가 저장된 위치를 빠르게 알 수 있는 것
    

**인덱스 종류**

- 클러스터형 인덱스
    - 테이블당 하나씩 만들 수 있는 인덱스
    - 기본 키로 지정된 필드 → 기본적으로 클러스터형 인덱스로 간주됨.
    - 기본 키로 지정된 필드가 없는 경우 → `NOT NULL` 제약 조건과 `UNIQUE` 제약 조건이 있는 필드 즉, `NULL`값이 될 수 없는 고유한 값을 갖는 필드를 클러스터형 인덱스로 간주
- 세컨더리 인덱스(논클러스터형 인덱스)
    - 테이블당 여러 개가 존재할 수 있는 인덱스
    - 클러스터형 인덱스를 활용한 검색보다 일반적으로 느림.

**모든 필드에 대해 인덱스를 생성하지 않는 이유**

- 인덱스도 여러 데이터를 포함하는 자료구조이므로 인덱스가 차지하는 공간이나 생성 시간이 점점 커질 수 있음.
- 조회(`SELECT`) 성능은 향상시킬 수 있지만, 그 외의 작업(`INSERT`, `UPDATE`, `DELETE`)에 대해서는 오히려 성능을 떨어뜨리는 원인이 되기도함.
    - 새로운 데이터를 삽입하거나 기존 데이터를 수정/삭제할 때 인덱스에 대한 작업도 동시에 이루어져야 함 → 인덱스를 유지하고 갱신하는 추가적인 자원과 연산 필요

**인덱스를 만드는 것이 유용한 경우**

- 데이터가 충분히 많은 테이블
- 조회가 빈번히 이루어지는 테이블
