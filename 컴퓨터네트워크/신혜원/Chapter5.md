https://rainbow-bream-010.notion.site/5-HTTP-325a3228ceb3814db3fdc672b68ced49?source=copy_link

# 5. 응용 계층 - HTTP의 기초

# DNS와 URI/URL

## 도메인 네임

![image.png](image.png)

### **네임서버**

- 도메인 네임과 그에 대응하는 IP 주소
- 여러개

### DNS 서버

- 도메인 네임을 관리하는 네임 서버

<aside>
❓

**네임서버 여러개, DNS 서버 여러개?**

- 네임서버 = DNS 서버
</aside>

### Resolve /  Resolving (풀이)

- IP 주소를 모르는 상태)
    - 도메인 네임에 대응되는 IP주소를 알아내는 과정

---

## 도메인 계층 구조

![FQDN(Fully Qulified Domain Name)](image%201.png)

FQDN(Fully Qulified Domain Name)

<aside>
❗

### 최상위 도메인

- 마지막 부분X
- 점`.` 으로 표현되는 루트 도메인 ⇒ 도메인 네임의 일부
- 마지막 점을 생략하기 때문에 최상위가 도메인으로 간주됨
</aside>

---

## DNS (Domain Name System)

- **도메인 네임 → IP주소로 바꿔주는 시스템**
- 사람이 이해하기 쉬운 이름을 컴퓨터가 이해할 수 있는 주소로 변환
    
    ```
    google.com → 142.250.206.14
    ```
    
- 공개 DNS 서버 : ISP에서 할당해 주는 로컬 네임 서버 주소가 아닌 서버

### 로컬 네임서버

- 클라이언트와 맞닿아 있는 네임 서버
- 클라이언트가 도메인 네임을 통해 IP주소를 알아내고자 할 때 가장 먼저 찾게 되는 네임 서버

### 공개 DNS 서버

- ISP에서 할당해 주는 로컬 네임 서버 주소가 아닌 서버

### 루트 네임 서버

- 루트 도메인 관장

### TLD 네임 서버

- 최상위 도메인 관장

---

![image.png](image%202.png)

질의 과정이 많을수록 네트워크 내 트래픽이 많아짐.

- 트래픽 :  네트워크를 통해 오가는 데이터의 양

리졸빙에 시간이 높으면 임시저장하고 추후 질의에 활용된다.

⇒ **DNS 캐시**

- 적은 트래픽, 짧은 시간
- 이전에 조회한 결과를 저장

---

# 자원

- 네트워크 상의 메세지를 통해 주고받는 최종대상
- 두 호스트가 송수신 하는 대상

---

# URI / URL

## URI (Uniform Resource Identifier)

- 인터넷 자원을 **식별하기위한 정보**
- 종류:
    - URL (위치)
    - URN (이름)

## URL (Uniform Resource Locator)

- 자원의 **위치 정보**

## URL 구조

```
https://www.example.com:8042/over/there?name=ferret#nose
```

| 구성 | 설명 |
| --- | --- |
| scheme | 프로토콜 (http, https 등) |
| authority | 도메인 + 포트 |
| path | 경로 |
| query | 요청 파라미터 |
| fragment | 문서 내부 위치 |

---

## 주요 포인트

1. `http://` → HTTP (보안 없음)
    
    `https://` → HTTPS (보안)
    
2. 도메인 or IP 주소 포함
3. 자원의 위치 경로 포함
4. query → 서버로 데이터 전달
5. fragment → 페이지 내부 이동

---

# HTTP

## HTTP 역할

- 웹에서 데이터를 주고받기 위한 프로토콜

## 특징

### 1. 요청-응답 기반 프로토콜

### 2. 미디어 독립적 (데이터 종류 상관 없음) 프로토콜

### **3. Stateless (상태 없음) 프로토콜**

- 요청 간 상태 기억 X
- 설계목표
    - 확장성
    - 견고성

### 4. 지속 연결 프로토콜(HTTP 1.1 이상) = 킵 얼라이브

- 연결 방식
    
    ![image.png](image%203.png)
    
    ### 🔸 비지속 연결
    
    - 요청마다 연결 생성/종료
    
    ### 🔸 지속 연결
    
    - 한 번 연결로 여러 요청 처리

---

# HTTP 메시지 구조

![HTTP는 요청 응답 기반의 프로토콜](image%204.png)

HTTP는 요청 응답 기반의 프로토콜

![image.png](image%205.png)

---

## HTTP 요청 라인 / 상태 라인 구성

| 구분 | 필드 이름 | 설명 |
| --- | --- | --- |
| 요청 라인 | 메서드 (method) | 클라이언트가 서버의 자원에 대해 수행할 작업의 종류를 나타냅니다. |
| 요청 라인 | 요청 대상 (request-target) | 요청을 보낼 서버의 자원을 명시합니다. 일반적으로 URL의 path가 포함됩니다. (예1) [http://example.com/hello?q=network](http://example.com/hello?q=network) → `/hello?q=network` (예2) [http://example.com/](http://example.com/) → `/` |
| 요청 라인 | HTTP 버전 (HTTP-version) | 사용된 HTTP 버전입니다. `HTTP/버전` 형식으로 표시됩니다. (예: HTTP/1.1) |
| 상태 라인 | HTTP 버전 (HTTP-version) | 사용된 HTTP 버전입니다. `HTTP/버전` 형식으로 표시됩니다. |
| 상태 라인 | 상태 코드 (status code) | 요청에 대한 결과를 나타내는 3자리 정수입니다. |
| 상태 라인 | 이유 구문 (reason phrase) | 상태 코드에 대한 문자열 형태의 설명입니다. |
- 상태 라인 예시
    
    ```
    HTTP/1.1 200 OK
    ```
    

---

# HTTP 메서드

| 메서드 | 설명 |
| --- | --- |
| GET | 데이터 조회 |
| HEAD | 헤더만 조회 |
| POST | 데이터 생성 |
| PUT | 전체 수정 |
| PATCH | 일부 수정 |
| DELETE | 삭제 |

# HTTP 상태 코드

| 범위 | 의미 |
| --- | --- |
| 100번대 | 정보 |
| 200번대 | 성공 |
| 300번대 | 리다이렉션 |
| 400번대 | 클라이언트 오류 |
| 500번대 | 서버 오류 |

---

# HTTP 헤더

## 요청 헤더

- Host
- User-Agent
- Referer

## 응답 헤더

- Server
- Allow
- Location

## 공통 헤더

- Date
- Content-Length
- Content-Type,
Content-Language,
Content-Encoding
- Connection

---

# 🔥 면접에서 자주 꼬리 질문 나오는 포인트

## 1️⃣ DNS 조회 과정 (⭐ 거의 필수)

### 핵심 흐름

1. 브라우저 → **로컬 DNS (Recursive Resolver)** 요청
2. 없으면 → **루트 서버**
3. → **TLD 서버 (.com, .kr)**
4. → **권한 있는 네임서버 (Authoritative)**
5. IP 받아서 클라이언트에게 전달

👉 한 줄 정리

> "DNS는 계층적으로 루트 → TLD → Authoritative 순으로 질의하여 IP를 얻습니다."
> 

---

## 2️⃣ Recursive vs Authoritative DNS (⭐ 자주 나옴)

### Recursive DNS  [리커시브]

- 대신 찾아주는 역할
- 캐시 있음
- 예: ISP DNS, Google DNS (8.8.8.8)

### Authoritative DNS [어쏘러테이티브]

- **진짜 정답 가지고 있음**
- 도메인의 실제 IP 보유

👉 한 줄

> "Recursive는 대신 찾아주고, Authoritative는 정답을 가진 서버입니다."
> 

---

## 3️⃣ DNS 캐시 + TTL (⭐ 중요)

### TTL (Time To Live)

- 캐시 유지 시간
- 시간이 지나면 다시 질의

👉 면접 포인트

> "TTL이 길면 빠르지만 최신성이 떨어질 수 있습니다."
> 

---

## 4️⃣ URL vs URI (꼬리 질문 대비)

👉 Q. URL은 URI인가?

- **URL ⊂ URI (포함 관계)**

👉 한 줄

> "모든 URL은 URI지만, 모든 URI가 URL은 아닙니다."
> 

---

## 5️⃣ HTTP Stateless 보완 (⭐ 중요)

지금 Stateless 잘 적었는데

👉 **꼬리 질문 대비 필요**

### Q. Stateless인데 로그인은 어떻게 유지?

- 쿠키 / 세션 / 토큰 사용

👉 한 줄

> "HTTP는 Stateless지만, 쿠키와 세션으로 상태를 유지합니다."
> 

---

## 6️⃣ HTTP vs HTTPS (조금 더 깊게)

### HTTPS 핵심

- TLS 기반 암호화
- 인증서 사용

👉 면접 한 줄

> "HTTPS는 TLS를 통해 데이터를 암호화하고 서버를 인증합니다."
> 

---

## 7️⃣ HTTP 메서드 멱등성 (⭐ 단골)

### 멱등성 (Idempotent)

- 여러 번 요청해도 결과 동일

| 메서드 | 멱등성 |
| --- | --- |
| GET | O |
| PUT | O |
| DELETE | O |
| POST | X |

👉 한 줄

> "GET, PUT, DELETE는 멱등하지만 POST는 아닙니다."
> 

---

## 8️⃣ 상태 코드 대표 예시 (면접에서 많이 물음)

### 꼭 외우면 좋은 것

- 200 OK
- 201 Created
- 301 / 302 Redirect
- 400 Bad Request
- 401 Unauthorized
- 403 Forbidden
- 404 Not Found
- 500 Internal Server Error

---

## 9️⃣ HTTP 헤더 하나만 더 추가하면 좋음

👉 거의 무조건 나오는 것

- **Authorization**
- **Cookie / Set-Cookie**

👉 한 줄

> "Authorization 헤더로 인증 정보를 전달합니다."
> 

---

## 🔟 추가하면 “오?” 하는 포인트

### CORS (⭐ 요즘 중요)

- 다른 도메인 요청 제한 정책

👉 한 줄

> "브라우저는 보안을 위해 다른 출처 요청을 제한하며, 이를 CORS라고 합니다."
>