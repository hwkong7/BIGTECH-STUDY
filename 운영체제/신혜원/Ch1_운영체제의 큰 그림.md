# 1. 운영체제의 큰 그림

# 운영체제

- 컴퓨터의 자원을 관리하고 사용자에게 서비스를 제공하는 시스템
- 데스크탑 운영체제

![windows.webp](windows.webp)

![image.png](image.png)

![image.png](image%201.png)

- 스마트폰 운영체제

![image.png](image%202.png)

![image.png](image%203.png)

# 커널(Kernel)

- 운영체제의 핵심 기능을 담당하는 부분

- ≒ 자동차의 엔진

- ≒ 사람의 심장 (핵심부)

# 운영체제의 역할

<aside>
❗

자원 할당 및 관리

</aside>

<aside>
💡

자원 : 프로그램 실행에 마땅히 필요한 요소

- 데이터 (소프트웨어)

- 부품 (하드웨어)
</aside>

## CPU 관리: CPU 스케줄링

- CPU는 한정자원
- 운영체제는 실행 중인 모든 프로그램들이 공정하고 합리적으로 CPU를 할당받도록 CPU의 할당 순서와 사용 시간을 결정한다.

<aside>
<img src="https://www.notion.so/icons/categories_green.svg" alt="https://www.notion.so/icons/categories_green.svg" width="40px" />

- 기본 개념 - 우선순위, 스케줄링 큐, 선점형과 비선점형

- CPU 스케줄링 알고리즘

- 리눅스 CPU 스케줄링

</aside>

## 메모리 관리: 가상 메모리

- 새롭게 실행하는 프로그램을 메모리에 적재
- 종료된 프로그램을 메모리에서 삭제
- ⇒ 가상 메모리 활용

<aside>
💡

가상 메모리 : 더 큰 메모리 이용할 수 있는 기술

</aside>

<aside>
<img src="https://www.notion.so/icons/categories_green.svg" alt="https://www.notion.so/icons/categories_green.svg" width="40px" />

- 물리 주소와 논리 주소

- 메모리 할당

- 페이징과 페이지 교체 알고리즘

</aside>

## 파일/디렉터리 관리: 파일 시스템

- 보조기억장치를 효율적으로 관리하기 위해 파일 시스템을 활용

<aside>
💡

파일 시스템 : 보조기억장치 내의 정보를 파일 및 폴더(디렉터리) 단위로 접근 · 관리할 수 있도록 만드는 운영체제 내부 프로그램

</aside>

<aside>
<img src="https://www.notion.so/icons/categories_green.svg" alt="https://www.notion.so/icons/categories_green.svg" width="40px" />

- 파일과 디렉터리

- 파일 시스템
</aside>

## 프로세스 및 스레드 관리

<aside>
💡

프로세스 : 실행 중인 프로그램

</aside>

<aside>
💡

스레드 : 프로세스를 이루는 실행의 단위

</aside>

![같은 프로그램이라도 여러 번 실행하면 별도의 프로세스가 될 수 있다.](image%204.png)

같은 프로그램이라도 여러 번 실행하면 별도의 프로세스가 될 수 있다.

<aside>
<img src="https://www.notion.so/icons/categories_green.svg" alt="https://www.notion.so/icons/categories_green.svg" width="40px" />

- 프로세스와 스레드

- 동기화와 교착 상태
</aside>

# 시스템 콜과 이중 모드

![커널 영역과 사용자 영역](image%205.png)

커널 영역과 사용자 영역

![응용 프로그램 작동 원리](image%206.png)

응용 프로그램 작동 원리

소프트웨어 인터럽트 : 사용자 프로그램이 OS 서비스를 요청하기 위해 발생시키는 인터럽트

![image.png](image%207.png)

- 사용자 영역을 실행하는 과정에서 시스템 콜이 호출되면 여느 인터럽트와 마찬가지로 CPU는 현재 수행 중인 작업을 백업하고,
- 커널 영역 내의 인터럽트를 처리하기 위한 코드(시스템 콜을 구성하는 코드)를 실행한 뒤,
- 다시 사용자 영역의 코드 실행을 재개

### 이중모드

- 사용자 모드 : 운영체제 서비스를 제공받을 수 없는 실행모드
- 커널 모드 : 운영체제 서비스를 제공받을 수 있는 실행 모드
