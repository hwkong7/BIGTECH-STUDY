# 3장 SQL

## 데이터 정의 언어(DDL)

<aside>

| 종류 | 설명 |
| --- | --- |
| CREATE | 데이터베이스 혹은 데이터베이스 객체 생성 |
| ALTER | 데이터베이스 객체 갱신 |
| DROP | 데이터베이스 객체 삭제 |
| TRUNCATE | 테이블 구조를 유지한 채 모든 레코드 삭제 |

### CREATE

데이터베이스, 테이블, 뷰, 인덱스, 그 외 사용자까지 데이터베이스에서 관리될 수 있는 대상을 정의한다.

```sql
CREATE DATABASE 데이터베이스_이름
```

하단의 키워드를 명시하여 특정 필드가 지켜야 할 제약 조건을 명시할 수 있다.

| 키워드 | 제약 조건 |
| --- | --- |
| PRIMARY EKY | 특정 필드를 기본 키로 지정 |
| UNIQUE | 특정 필드가 고유한 값을 갖도록 설정 |
| FOREIGN KEY | 특정 필드를 외래 키로 지정 |
| DEFAULT 기본값 | 기본값 지정 |
| NULL/NOT NULL | 특정 필드에 NULL 값을 허용/허용하지 않음 |

### ALTER

이미 생성된 테이블에 새로운 필드를 추가하거나 기존의 필드를 수정/삭제할 수 있고, 제약 조건 또한 새롭게 추가하거나 수정/삭제할 수 있다.

```sql
ALTER TABLE 테이블 이름 ADD COLUMN 필드_이름 필드_타입 [제약 조건]
```

### DROP

테이블이나 데이터베이스를 삭제할 수 있다.

```sql
DROP DATABASE 데이터베이스_이름
```

### TRUNCATE

테이블의 구조를 유지한 채로 테이블의 모든 레코드를 삭제한다.

```sql
TRUNCATE TABLE 테이블_이름
```

</aside>

## 데이터 조작 언어(DML)

<aside>

| 종류 | 설명 |
| --- | --- |
| SELECT | 테이블의 레코드 조회 |
| INSERT | 테이블에 레코드 삽입 |
| UPDATE | 테이블의 레코드 수정 |
| DELETE | 테이블의 레코드 삭제 |

### INSERT

테이블에 새로운 레코드를 삽입할 수 있다.

```java
INSERT INTO 테이블(필드1, 필드2) VALUES(값1, 값2)
```

### UPDATE와 DELETE

```sql
UPDATE 테이블_이름
	SET 필드1 = 값1
	WHERE 조건식
```

```sql
DELETE FROM 테이블 이름
	WHERE 조건식
```

**외래키 제약 조건**

한 테이블이 다른 테이블을 외래키로 참조하는 상황에서 참조되는 레코드가 수정되거나 삭제될 경우, 참조하는 레코드는 아래와 같이 동작할 수 있다.

| 제약 조건 | 설명 |
| --- | --- |
| CASCADE | 참조하는 데이터도 함께 수정/삭제함 |
| SET NULL | 참조하는 데이터를 NULL로 변경함 |
| SET DEFAULT | 참조하는 데이터를 기본값(default)으로 변경함 |
| RESTRICT | 수정/삭제를 허용하지 않음 |
| NO ACTION | MySQL의 경우 RESTRICT와 동일하게 동작 |

### SELECT

삽입된 레코드를 조회할 수 있다.

```sql
SELECT 필드1, 필드2
	FROM 테이블_이름
	WHERE 조건식
	GROUP BY 그룹화할 필드
	HAVING 조건식
	ORDER BY 정렬할 필드
	LIMIT 뽑아올 레코드 수
```

SELECT문의 실행 순서는 **FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT** 이다.

</aside>

## 트랜잭션 제어 언어(TCL)

<aside>

트랜잭션을 제어하는 데 사용되는 SQL 명령들이다.

| 종류 | 설명 |
| --- | --- |
| COMMIT | 데이터베이스에 작업 반영 |
| ROLLBACK | 작업 이전의 상태로 되돌림 |
| SAVEPOINT  | 롤백의 기준점 설정 |

트랜잭션의 결과는 **변경된 내용을 적용한 뒤 종료하는 COMMIT과 변경된 내용을 적용하지 않고 종료하는 ROLLBACK**이다.

</aside>
