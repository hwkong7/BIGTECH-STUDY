# 4장 효율적 쿼리

## 서브 쿼리

<aside>

서브 쿼리는 내부에 다른 SQL문이 포함되어 있는 SQL이다.

```sql
SELECT
	users.username,
	(SELECT COUNT(*)
	FROM posts
	WHERE posts.user_id = users.user_id) AS post_count
FROM users;
```

</aside>

## 조인

<aside>

### INNER JOIN

테이블 A와 B의 레코드 중 조인 조건을 모두 만족하는 레코드를 결과로 반환한다.(교집합)

![image.png](attachment:e4b9dfbc-4fc4-45b9-8c08-8df8f174609e:image.png)

### OUTER JOIN

테이블 A와 B의 레코드 중 기준 테이블의 레코드를 다 가져오고 조인 조건을 만족하지 않는 다른 테이블의 레코드는 NULL로 채운다.

![image.png](attachment:101b967b-9bfc-4acd-849e-364748c97fc5:image.png)

MySQL에는 FULL OUTER JOIN이 존재하지 않기에 OUTER JOIN을 2번 수행한 후 UNION을 통해 결과를 합쳐야 한다.

</aside>

## 뷰

<aside>

뷰는 SELECT의 결과로 만들어진 가상의 테이블이다.

여러 테이블을 조인하거나 복잡한 조건식을 사용한 SQL문을 하나의 뷰로 만들어 두면, 이후 복잡한 SQL문을 반복적으로 작성하는 것보다 효율적이다.

</aside>

## 인덱스

<aside>

인덱스는 검색 속도 향상을 목적으로 만드는 하나 이상의 테이블 필드에 대한 자료구조이다.

기본키(PK)를 **클러스터형 인덱스(clustered index)**라 한다.
기본키 이외의 인덱스를 **세컨더리 인덱스(secondary index)**라고 한다.

InnoDB에서 세컨더리 인덱스를 통해 레코드를 조회할 때는, 세컨더리 인덱스 엔트리에 함께 저장된 기본 키 값을 이용해 클러스터드 인덱스를 다시 조회하여 실제 데이터에 접근한다.

쿼리가 필요로 하는 컬럼이 모두 인덱스에 포함되어 있어, 테이블(실제 데이터 페이지)까지 추가로 접근하지 않고 결과를 얻을 수 있는 인덱스를 **커버링 인덱스**라고 한다.

</aside>
