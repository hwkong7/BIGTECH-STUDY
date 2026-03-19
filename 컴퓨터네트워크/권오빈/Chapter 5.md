# 5장 응용 계층 - HTTP의 기초

## 도메인 네임과 DNS

<aside>

네트워크 상에서 호스트 식별을 위해서는 IP 주소를 사용하지만
IP 주소는 항상 바뀔 수 있기 때문에 도메인 네임을 사용한다.

도메인 네임은 IP 주소와 대응되고 이 IP 주소는 **네임 서버(name server)**에서 관리한다.
도메인 네임을 관리하는 네임 서버는 **DNS 서버**라고 한다.

![image.png](attachment:83f1a40e-e304-4f91-84b6-ce010bd2a8a7:image.png)

도메인 네임은 .을 기준으로 계층 구조이다.
최상단에 **루트 도메인**, 다음에 **최상위 도메인(TLD)**이 있다.
최상위 도메인의 하부 도메인은 **2단계 도메인(세컨드 레벨 도메인)**이다.
`www.example.com.` 처럼 도메인 네임을 모두 포함하는 도메인 네임은 **전체 주소 도메인 네임(FQDN)**이다.

계층적으로 분산되어 있는 도메인 네임에 대한 관리 체계를 **도메인 네임 시스템(DNS)**라고 한다.

</aside>

## DNS 질의 과정

![image.png](attachment:338e86ce-6fdb-46be-aed3-9aa2bcf4cb69:image.png)

<aside>

1. 로컬 네임 서버에게 도메인 네임을 질의한다.
2. 루트 네임 서버에게 질의한다.
3. TLD 네임 서버에게 질의한다.
4. 하위 레벨의 도메인을 관장하는 네임 서버에게 질의한다.

위의 과정으로 질의가 반복되면 부하가 크기 때문에 캐싱을 사용한다.

</aside>

## DNS 레코드 타입

| 레코드 유형 | 설명 |
| --- | --- |
| A | 특정 호스트에 대한 도메인 네임과 IPv4의 관계 |
| AAAA | 특정 호스트에 대한 도메인 네임과 IPv6의 관계 |
| CNAME | 호스트 네임에 대한 별칭 지정 |
| NS | 특정 호스트의 IP 주소를 찾을 수 있는 네임 서버 |
| MX | 해당 도메인과 연동되어 있는 메일 서버 |

## 자원과 URI/URL

<aside>

**자원(Resource)**는 네트워크 상의 메시지를 통해 주고받는 최종 대상이다.
**URI(Uniform Resource Identifier)**는 자원을 식별하기 위한 정보이다.
URI는 **URN(Uniform Resource Name)**과 **URL(Uniform Resource Locator)**로 나뉜다. 

![image.png](attachment:30247593-7fb3-4add-afd2-f8383f167a9b:image.png)

</aside>

## HTTP의 특징

<aside>

HTTP의 목적은 애플리케이션의 다양한 자원을 데이터 형식에 구애받지 않고 네트워크를 통해 송수신하는 것이다.

HTTP는 아래의 4가지 특징을 가진다.

1. 요청과 응답 기반으로 동작한다.
2. 미디어 독립적 프로토콜이다.(미디어 타입에 제한을 두지 않는다.)
3. **상태를 유지하지 않는다.(stateless)**
4. **지속 연결을 지원한다.**(HTTP 1.1 이상에서 지원. 1.0은 지속 X)

![image.png](attachment:e14eebdc-ba37-404e-b9d2-bd3382e800e4:image.png)

### HTTP 1.1

지속 연결 기능을 지원한다.

메시지를 평문으로 주고받는다.

### HTTP 2.0

바이너리 데이터 기반으로 메시지를 주고 받는다.

헤더를 압축하여 송수신해 네트워크 이용 효율을 높일 수 있다.

**서버 푸시(server push).** 클라이언트가 요청하지 않아도 미래에 필요할 것으로 예상되는 자원을 미리 전송한다. 

여러 개의 독립적인 스트림으로 요청-응답 메시지를 병렬적으로 주고 받는 **멀티플렉싱(multiplexing)** 기법을 사용한다.
멀티플렉싱을 통해 **HOL(Head-Of-Line blocking)** 문제를 완화했다.

### HTTP 3.0

UDP 기반으로 동작한다.

UDP 기반으로 구현된 QUIC(Quick UDP Internet Connections) 기반으로 동작한다.

</aside>

## HTTP 메시지 구조

<aside>

start line은 요청 메시지일 경우 요청 라인(request-line)이 되고, 응답 메시지일 경우 상태 라인(status-line)이 된다.

![image.png](attachment:928233b2-0052-4073-92a0-87b8f7e36dd5:image.png)

![image.png](attachment:e3165a2b-fa9c-4ec6-bc3b-2996943cf9bd:image.png)

</aside>

## HTTP 메서드와 상태 코드

<aside>

### HTTP 메서드

| HTTP 메서드 | 설명 |
| --- | --- |
| GET | 자원을 습득하기 위한 메서드 |
| POST | 서버로 하여금 특정 작업을 처리하게끔 하는 메서드 |
| PUT | 자원을 대체하기 위한 메서드 |
| PATCH | 자원에 대한 부분적 수정을 위한 메서드 |
| DELETE | 자원을 삭제하기 위한 메서드 |

### HTTP 상태 코드

| 상태 코드  | 이유 구문  | 설명 |
| --- | --- | --- |
| 200 | OK | 요청이 성공했음 |
| 201 | Created | 요청이 성공했으며, 새로운 자원이 생성되었음 |
| 202 | Accepted | 요청을 잘 받았으나, 아직 요청한 작업을 끝내지 않았음 |
| 204 | No Content | 요청이 성공했지만, 메시지 본문으로 표시할 데이터가 없음 |

| 상태 코드 | 이유 구문 | 설명 |
| --- | --- | --- |
| 301 | Moved Permanetly | 영구적 리다이렉션 - 재요청 메서드가 변경될 수 있음 |
| 308 | Permanent Redirect | 영구적 리다이렉션 - 재요청 메서드가 변경되지 않음 |
| 302 | Found | 일시적 리다이렉션 - 재요청 메서드가 변경될 수 있음 |
| 303 | See Other | 일시적 리다이렉션 - 재요청 메서드가 GET으로 변경됨 |
| 307 | Temporary Redirect | 일시적 리다이렉션 - 재요청 메서드가 변경되지 않음 |
| 304 | Not Modified | 캐시 - 자원이 변경되지 않음 |

| 상태 코드 | 이유 구문 | 설명 |
| --- | --- | --- |
| 400 | Bad Request | 요청 메시지의 내용이나 형식 자체에 문제가 있음 |
| 401 | Unauthorized | 요청한 자원에 대한 유효한 인증이 없음 |
| 403 | Forbidden | 요청이 서버에 의해 거부됨 |
| 404 | Not Found | 요청받은 자원을 찾을 수 없음 |
| 405 | Method Not Allowed | 요청한 메서드를 지원하지 않음 |

| 상태 코드 | 이유 구문 | 설명 |
| --- | --- | --- |
| 500 | Internal Server Error | 요청을 처리할 수 없음 |
| 502 | Bad Gateway | 중간 서버의 통신 오류 |
</aside>
