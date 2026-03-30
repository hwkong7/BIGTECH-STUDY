## 데이터베이스 객체

> 데이터베이스에서 정의될 수 있는 대상을 통칭하는 용어
> 
- 테이블, 인덱스, 뷰 등

---

## SQL 명령

- 데이터 정의 언어(DDL)
- 데이터 조작 언어(DML)
- 데이터 제어 언어(DCL)
- 트랜잭션 제어 언어(TCL)

---

## 데이터 정의 언어(DDL)

- CREATE
    - 데이터베이스 혹은 데이터베이스 객체 생성
    - 데이터베이스, 테이블, 뷰, 인덱스, 그 외 사용자까지 데이터베이스에서 관리될 수 있는 다양한 대상을 정의
    
    ![image.png](attachment:785749e3-f16a-46c3-ba0c-6c10f71e0249:image.png)
    
    - `필드_타입`의 우측 혹은 `CREATE TABLE`문 하단에 아래의 키워드를 명시함으로써 특정 필드가 지켜야 할 제약 조건 명시 가능
        
        <aside>
        
        ![image.png](attachment:5875ebc3-c053-4d53-a058-f32367447229:image.png)
        
        ![image.png](attachment:69c5173b-bebe-4564-bffd-4c0c866fb152:image.png)
        
        ![image.png](attachment:bfcac7f7-e876-4df0-be3b-10d72d159914:image.png)
        
        </aside>
        
        - 고유 키(unique key): `UNIQUE` 제약 조건이 명시된 필드
            - 기본 키와 달리 테이블 내에 여러 개 존재할 수 있고, `NULL` 값을 가질 수도 있음.
- ALTER
    - 데이터베이스 객체 갱신
    - 테이블에 새로운 필드를 추가하거나 기존의 필드를 수정/삭제 가능
    - 제약 조건을 새롭게 추가하거나 수정/삭제 가능
    - 테이블뿐만 아니라 뷰, 인덱스 등에도 적용 가능
- DROP
    - 데이터베이스 객체 삭제
    - 테이블뿐만 아니라 뷰, 인덱스 등에도 적용 가능
    
    ![image.png](attachment:03400ee6-db91-456c-b708-f86f9ecc1836:image.png)
    
- TRUNCATE
    - 테이블 구조를 유지한 채 모든 레코드 삭제
    
    ![image.png](attachment:4d42aac6-31ac-4dde-bd84-c222ded8ae53:image.png)
    

---

## 데이터 조작 언어(DML)

- SELECT
    - 테이블의 레코드 조회
    - 테이블 내 레코드를 다양하게 정렬하거나 필터링하여 조회하는 것도 가능
    - SELECT문의 문법 순서(작성 순서) ≠ 실제 실행 순서
        - `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY` → `LIMIT`
    - 패턴 검색
        - 문자열 데이터에서 특정 패턴을 찾는 기능
        - `LIKE` 연산자와 와이드카드 문자인 `%`, `_`를 사용
            - `%`: 0개 이상의 임의의 문자와 일치
            - `_`: 정확히 1개인 임의의 문자와 일치
    - 연산/집계 함수
        - 조회된 레코드에 대한 특정 연산을 수행하거나 집계하는 함수
            
            ![image.png](attachment:d223b9d0-aae6-4b20-adb7-a14ae6d4b7e4:image.png)
            
    - GROUP BY
        - 특정 필드를 기준으로 레코드를 그룹화하고자 할 때 사용
    - HAVING
        - `GROUP BY`절로 그룹화된 결과에 조건을 적용하기 위해 사용
        - `WHERE`절: ‘그룹화되기 전 개별 레코드’에 대한 조건식
        - `HAVING`절: ‘그룹화된 레코드’에 대한 조건식
    - ORDER BY
        - 특정 필드를 기준으로 데이터를 정렬하는 데 사용
    - LIMIT
        - 조회활 레코드 수를 제한하기 위해 사용
    
    ![image.png](attachment:ee9f293a-07e5-427a-b8c4-f501ea3d88cc:image.png)
    
- INSERT
    - 테이블에 레코드 삽입
    - 삽입할 값이 지정되지 않은 필드의 경우, 기본값이 있다면 기본값으로 채워지고 기본값이 없다면 `NULL`로 채워짐.
    
    ![image.png](attachment:3560f6d3-4969-46e2-af61-dde502d7d213:image.png)
    
- UPDATE
    - 테이블의 레코드 수정
    - `WHERE` 조건식: 특정 조건에 부합하는 레코드만 선별하기 위한 일종의 필터
    - `SET`에 명시되는 `=`: 대입 연산자
    - `WHERE`절에 명시되는 `=`: 비교 연산자
    
    ![image.png](attachment:ca877110-b043-4893-ab4b-87d3bebd511b:image.png)
    
- DELETE
    - 테이블의 레코드 삭제
    
    ![image.png](attachment:049c99f8-a58c-4a22-ac53-000e1eb18f79:image.png)
    

---

## 트랜잭션 제어 언어(TCL)

> 트랜잭션을 제어하는 데 사용되는 SQL 명령어
> 
- COMMIT
    - 트랜잭션이 성공적으로 완료되어 트랜잭션에서 수행된 모든 변경 사항을 데이터베이스에 영구적으로 반영
    - DDL문은 자동으로 커밋됨.
    - `START TRANSACTION` 이나  `BEGIN` 은 자동 커밋이 꺼진 상태로 실행되므로 해당 명령어 직후에 명시된 작업들은 `COMMIT`이나 `ROLLBACK`을 만나기 전까지는 커밋되지 않음.
- ROLLBACK
    - 트랜잭션에서 수행된 변경 사항을 취소하고, 데이터베이스를 트랜잭션 시작 이전의 상태로 되돌림.
- SAVEPOINT
    - `ROLLBACK`으로 되돌아갈 시점을 지정하는 기능 수행
- 여러 작업을 포함하는 트랜잭션 → `START TRANSACTION` / `BEGIN`

---

## 데이터 제어 언어(DCL)

- RDBMS에서도 접속 가능한 사용자 계정을 생성하거나 삭제 가능
- 사용자마다 사용 가능한 SQL 명령을 제한하는 등의 권한 관리 가능
- GRANT
    - 사용자에게 권한 부여
- REVOKE
    - 사용자로부터 권한 회수
