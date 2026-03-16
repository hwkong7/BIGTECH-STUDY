# 4. 전송 계층 - TCP와 UDP 
## 전송 계층 (Transport Layer)
- - 전송 계층은 **호스트 내 프로세스 간 논리적 통신(Process-to-Process Communication)**을 담당하는 계층이다.
- 대표적인 프로토콜은 TCP와 UDP이다.
  - TCP : 신뢰성 있는 데이터 전송
  - UDP : 빠른 데이터 전송
 
## TCP와 UDP의 목적과 특징
- 전송 계층은 프로세스 간 데이터 전달을 담당한다.
- IP 주소는 **호스트를 식별**한다.
- 하지만 실제 네트워크 통신의 대상은 **프로세스**이다.
- 따라서 전송 계층에서는 **포트 번호(Port Number)**를 사용하여 프로세스를 식별한다.

## 포트를 통한 프로세스 식별
- 네트워크에서 하나의 호스트는 여러 프로세스를 실행한다.
- 예시) 192.168.0.15 : 8000
  - IP 주소 → 호스트 식별
  - 포트 번호 → 프로세스 식별
- IP 주소 + 포트 번호를 통해 특정 프로세스를 식별한다.

## 포트 번호
- 포트 번호는 16비트로 표현된다.
- 가능한 범위 [0 ~ 65535] 포트는 다음 3가지로 구분된다.

| 종류              | 포트 범위         |
| --------------- | ------------- |
| Well-known Port | 0 ~ 1023      |
| Registered Port | 1024 ~ 49151  |
| Dynamic Port    | 49152 ~ 65535 |

## 잘 알려진 포트 (Well-known Port)

| 포트    | 서비스    |
| ----- | ------ |
| 20,21 | FTP    |
| 22    | SSH    |
| 23    | TELNET |
| 53    | DNS    |
| 67,68 | DHCP   |
| 80    | HTTP   |
| 443   | HTTPS  |

## 등록된 포트 (Registered Port)

| 포트   | 서비스           |
| ---- | ------------- |
| 1194 | OpenVPN       |
| 1433 | MS SQL Server |
| 3306 | MySQL         |
| 6379 | Redis         |
| 8080 | HTTP 대체 포트    |

## 동적 포트 (Dynamic Port)
- 범위 [ 49152 ~ 65535 ]
- **특징**
  - 클라이언트 프로그램이 사용
  - 임시 포트 (Ephemeral Port)

- 웹 브라우저 접속
  - 클라이언트 : 192.168.0.15:50000
  - 서버 : 10.10.10.1:80

## NAT와 NAPT
### NAT (Network Address Translation)
- NAT는 사설 IP를 공인 IP로 변환하는 기술이다.
- 목적
  - 사설 네트워크에서 인터넷 사용 가능
  - 공인 IP 부족 문제 해결
### NAPT
- NAPT는 IP + 포트 기반 주소 변환이다.
- **특징**
  - 하나의 공인 IP 사용
  - 여러 사설 IP 공유 가능
- 예) 192.168.0.5:1025
- 예) 192.168.0.6:1026
- ↓
- 1.2.3.4:6200
- 1.2.3.4:6201

## TCP와 UDP
- TCP와 UDP는 전송 계층의 대표적인 프로토콜이다.

| 특징 | TCP | UDP |
|---|---|---|
| 연결 방식 | 연결형 (Connection-oriented) | 비연결형 (Connectionless) |
| 신뢰성 | 높음 | 낮음 |
| 데이터 순서 보장 | O | X |
| 오류 제어 | O | X |
| 속도 | 상대적으로 느림 | 빠름 |

## UDP 헤더
- UDP 헤더는 구조가 매우 단순하다.
- 구성
  - 송신 포트
  - 수신 포트
  - 길이
  - 체크섬
- 특징
  - 연결 과정 없음
  - 빠른 전송
- 사용 예
  - DNS 
  - 스트리밍
  - 실시간 게임
  - VoIP

## TCP 헤더
- TCP 헤더는 UDP보다 훨씬 복잡하다.
- 주요 필드
  - 송신 포트
  - 수신 포트
  - 순서 번호 (Sequence Number)
  - 확인 응답 번호 (ACK Number)
  - 윈도우
  - 체크섬
  - 플래그

- 플래그 종류
  - SYN
  - ACK
  - FIN
  - RST
  - PSH
  - URG

## TCP 연결 수립 (3-way Handshake)
- TCP 연결은 3단계 과정으로 이루어진다.
### 1단계
- 클라이언트 → 서버 : SYN
- 연결 요청
### 2단계
- 서버 → 클라이언트 : SYN + ACK
- 요청 수락
### 3단계
- 클라이언트 → 서버 : ACK
- 연결 완료

### 정리
- SYN
- SYN + ACK
- ACK

## TCP 오류 제어
- TCP는 재전송을 통해 오류를 제어한다.
- 재전송이 발생하는 경우
  - 1️⃣ 중복 ACK 발생
  - 2️⃣ 타임아웃 발생

## 흐름 제어 (Flow Control)
- 흐름 제어는 **수신 호스트 처리 속도**를 고려하여 전송 속도를 조절하는 기능이다.
- 수신 호스트는 윈도우 크기(Window Size) 를 통해 한 번에 받을 수 있는 데이터 양을 알려준다.

## 혼잡 제어 (Congestion Control)
- 혼잡 제어는 네트워크 혼잡을 방지하기 위한 기능이다.
- 혼잡이 발생하면
  - 패킷 손실  
  - 전송 속도 저하
- 가 발생한다.

### 혼잡 제어 방식
- 대표적인 혼잡 제어 알고리즘
  - AIMD
  - Slow Start
  - Congestion Avoidance
- **AIMD**
  - Additive Increase
  - Multiplicative Decrease
- **동작 방식**
  - 1️⃣ 혼잡이 없으면 전송량 증가
  - 2️⃣ 혼잡 발생 시 전송량 감소

## RTT (Round Trip Time)
- RTT는 패킷을 전송한 후 응답을 받을 때까지 걸리는 왕복 시간이다.
- 예) ping 결과 
- 예) min / avg / max / mdev
- 여기서 avg가 RTT 평균값이다.

## TCP의 종료 (TCP Connection Termination)
- TCP 연결 종료는 FIN과 ACK 세그먼트를 이용한 4단계 과정으로 이루어진다.
- 이 과정을 Four-way Handshake라고 한다.

### TCP 연결 종료 과정
- 1️⃣ FIN 전송
  - 클라이언트 → 서버 : FIN
  - 연결 종료 요청
- 2️⃣ ACK 응답
  - 서버 → 클라이언트 : ACK
  - FIN 요청을 확인
- 3️⃣ FIN 전송
  - 서버 → 클라이언트 : FIN
  -서버도 연결 종료 요청
- 4️⃣ ACK 응답
  - 클라이언트 → 서버 : ACK
  - 연결 종료 완료

-**정리**
  - FIN
  - ACK
  - FIN
  - ACK

### Active Close vs Passive Close
- TCP 종료 과정에서는 두 가지 역할이 존재한다.

| 구분            | 설명                  |
| ------------- | ------------------- |
| Active Close  | 먼저 연결 종료 요청을 보낸 호스트 |
| Passive Close | 종료 요청을 받은 호스트       |

## TCP의 상태 관리 (TCP State Management)
- TCP는 연결 상태를 유지하며 동작하는 프로토콜이다.
- 이를 Stateful Protocol이라고 한다.
- TCP는 연결 과정에서 여러 상태(State) 를 가진다.
- 예) 
  - CLOSED
  - LISTEN
  - SYN-SENT
  - ESTABLISHED
  - FIN-WAIT
  - TIME-WAIT 

## 연결 수립 전 상태
### CLOSED
- 연결이 없는 상태
### LISTEN
- 서버가 연결 요청을 기다리는 상태
- 서버는 일반적으로 이 상태에서 시작한다.

## 연결 수립 과정 상태
- 3-way handshake 과정에서 다음 상태가 사용된다.

| 상태           | 설명                         |
| ------------ | -------------------------- |
| SYN-SENT     | 클라이언트가 SYN을 보낸 상태          |
| SYN-RECEIVED | 서버가 SYN을 받고 SYN+ACK을 보낸 상태 |
| ESTABLISHED  | 연결이 완료된 상태                 |

## 연결 종료 과정 상태
- 연결 종료 시 다음 상태들이 사용된다.

| 상태         | 설명                        |
| ---------- | ------------------------- |
| FIN-WAIT-1 | FIN을 보내고 ACK를 기다리는 상태     |
| FIN-WAIT-2 | FIN에 대한 ACK를 받은 상태        |
| CLOSE-WAIT | FIN을 받고 응답한 상태            |
| LAST-ACK   | FIN을 보내고 마지막 ACK를 기다리는 상태 |
| TIME-WAIT  | 일정 시간 대기 상태               |

## 주요 종료 상태 설명
### FIN-WAIT-1
- Active Close 호스트가 FIN을 전송한 상태
### FIN-WAIT-2
- FIN에 대한 ACK를 받은 상태
### CLOSE-WAIT
- Passive Close 호스트가
  - FIN 수신
  - ACK 응답
- 후 대기하는 상태
### LAST-ACK
- Passive Close 호스트가 FIN을 전송한 후 ACK를 기다리는 상태
### TIME-WAIT
- Active Close 호스트가 
  - 마지막 ACK 전송 후
  - 일정 시간 대기
- 하는 상태

## TIME-WAIT가 필요한 이유
- 마지막 ACK가 손실될 가능성이 있기 때문이다.
- ACK가 손실되면
- 서버가 FIN을 다시 전송할 수 있기 때문에
- 일정 시간 동안 연결 정보를 유지한다.

## CLOSING 상태
- 드물지만 양쪽이 동시에 FIN을 보낼 경우 발생한다.
- 이 경우
  - FIN → FIN
  - ACK → ACK
- 형태로 연결이 종료된다.

## TCP 상태 흐름 요약
- CLOSED
- ↓
- LISTEN
- ↓
- SYN-SENT / SYN-RECEIVED
- ↓
- ESTABLISHED
- ↓
- FIN-WAIT / CLOSE-WAIT
- ↓
- TIME-WAIT
- ↓
- CLOSED

## 핵심 정리
### TCP 연결 수립
- SYN
- SYN + ACK
- ACK

### TCP 연결 종료
- FIN
- ACK
- FIN
- ACK

### 주요 상태
- CLOSED
- LISTEN
- SYN-SENT
- SYN-RECEIVED
- ESTABLISHED
- FIN-WAIT
- TIME-WAIT