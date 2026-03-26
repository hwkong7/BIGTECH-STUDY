# CPU 스케줄링(CPU scheduling)

<aside>
💡

CPU 배분 방법

</aside>

## CPU 스케줄링 알고리즘

<aside>
💡

CPU 스케줄링의 절차

</aside>

## CPU 스케줄러

<aside>
💡

CPU 스케줄링 알고리즘을 결정하고 수행하는 운영체제의 일부분

</aside>

## 우선순위(priority)

- 모든 프로세스는 CPU의 자원을 필요로 하기 때문에 운영체제는 공정하고 합리적인 방법으로 CPU의 자원을 프로세스에 할당해야한다. ⇒ 공정하게 배분하는 방법은 프로세스별 우선순위를 판단하여 PCB에 명시하고 수선순위가 높은 프로세스에는 CPU 의 자원을 더 빨리, 더 많이 할당합니다.

## CPU 할당 기준

### CPU 활용률(CPU utilization)

<aside>
💡

전체 CPU의 가동 시간 중 작업을 처리하는 시간의 비율

기본적으로 입출력 작업이 많은 프로세스의 우선순위를 높게 유지

</aside>

## CPU 버스트(CPU burst)

<aside>
💡

CPU를 이용하는 작업

</aside>

## 입출력 버스트(I/O burst)

<aside>
💡

입출력장치를 기다리는 작업

</aside>

## 입출력 집중 프로세스

## CPU 집중 프로세스

<aside>
💡

비디오 재생이나 디스크 백업 작업을 담당하는 프로세스처럼 입출력 작업이 많은 프로세스

</aside>

<aside>
💡

복잡한 수학 연산이나 그래픽 처리 작업을 담당하는 프로세스

</aside>

입출력 집중 프로세스를 가능한 빨리 실행시켜 끊임없이 입출력장치를 작동시킨 다음, CPU 집중 프로세스에 집중적으로 CPU를 할당하는 것이 더 합리적

## 스케줄링 큐

<aside>
💡

CPU를 이용하고 싶은 프로세스의 PCB와 메모리로 적재되고 싶은 프로세스의 PCB, 특정 입출력장치를 이용하고 싶은 프로세스의 PCB를 큐에 삽입하여 줄 세우는 것

</aside>

![image.png](attachment:6672882d-3c4b-4b4e-a177-6ae6314e037b:image.png)

<aside>
💡

**준비 큐** : CPU를 이용하고 싶은 프로세스의 PCB가 서는 줄

</aside>

<aside>
💡

**대기 큐** : 대기 상태에 접어든 프로세스의 PCB가 서는 줄

</aside>

![image.png](attachment:32b6f597-3071-41d2-96b1-555b930b8181:image.png)

![image.png](attachment:94d579b0-d9ef-4d01-90cc-ef23a0561fac:image.png)

## 선점형 스케줄링과 비선점형 스케줄링

스케줄링은 프로세스의 실행이 끝나면 이루어진다.

하지만 종료되지 않았음에도 실행 도중 스케줄링이 실행되는 대표적인 두 시점

## 선점형 스케줄링

<aside>
💡

운영체제가 프로세스로부터 CPU 자원을 강제로 빼앗아 다른 프로세스에 할당할 수 있는 스케줄링

</aside>

![image.png](attachment:531a9156-c92b-4300-a807-b573171e39a4:image.png)

## 비선점형 스케줄링

<aside>
💡

어떤 프로세스 CPU를 사용하고 있을 때 그 프로세스가 종료되거나 스스로 대기 상태에 접어들기 전까지는 다른 프로세스가 끼어들 수 없는 스케줄링 방식

⇒ 다른 프로세스들은 그 프로세스의 CPU 사용이 모두 끝날 때까지 기다려야한다.

</aside>

![image.png](attachment:378c9cef-9960-471f-b449-45603319c500:image.png)

<aside>
❗

### 강제성의 차이

| **구분**      | **선점형 (Preemptive)**               | **비선점형 (Non-preemptive)**                    |
| ------------- | ------------------------------------- | ------------------------------------------------ |
| **핵심**      | OS가 CPU를 **강제로 빼앗을 수 있음**  | 프로세스가 **스스로 반납할 때까지** 기다림       |
| **장점**      | 응답성이 좋고 자원 독점 방지          | 문맥 교환(Context Switch) 오버헤드가 적음        |
| **단점**      | 잦은 문맥 교환으로 인한 오버헤드 발생 | 긴 작업이 CPU를 잡으면 뒤의 프로세스가 무한 대기 |
| **상태 변화** | 실행 → 준비 (타이머 인터럽트 등)      | 실행 → 종료 또는 실행 → 대기                     |

</aside>

# CPU 스케줄링 알고리즘

## 1️⃣ 선입 선처리 (FCFS)

<aside>
💡

먼저 온 순서대로 처리.

</aside>

![image.png](attachment:21dd1c53-dbe1-4f6c-8faf-bfe04b1ddcb3:image.png)

### 단점🚫

- **호위 효과(Convoy Effect) :** 앞의 긴 작업 때문에 뒤의 짧은 작업들이 과하게 기다리는 현상.

## 2️⃣최단 작업 우선 (SJF)

<aside>
💡

CPU 버스트가 가장 짧은 프로세스부터 실행.

</aside>

![image.png](attachment:5381a1c9-ed26-474d-a5e3-911a5f63ebd0:image.png)

### **특징**

- 평균 대기 시간을 최소화하는 최적(Optimal) 알고리즘.

## 3️⃣ 라운드 로빈 (Round Robin)

<aside>
💡

현대 OS의 기본. 각 프로세스에 **타임 슬라이스(Time Slice)**를 부여.

- 타임 슬라이스 : 프로세스가 CPU를 사용하도록 정해진 시간
</aside>

<aside>
❗

타임 슬라이스가 너무 크면 FCFS와 다를 바 없고, 너무 작으면 문맥 교환 비용이 커져 성능이 저하됩니다.

</aside>

![image.png](attachment:8098884a-d861-4189-a403-58066eec08d1:image.png)

## 4️⃣ SRTF (최소 잔여 시간 우선)

<aside>
💡

남은 시간이 가장 적은 프로세스 실행

</aside>

## 5️⃣ 우선순위 스케줄링 (Priority)

<aside>
💡

우선순위 높은 프로세스 먼저 실행

</aside>

![image.png](attachment:716e3a01-e149-4c68-97c4-a8e32008de2a:image.png)

### 문제점🚫

- **아사(Starvation) 현상 :** 우선순위가 낮은 프로세스가 평생 실행되지 못함.

### 해결법✅

- **에이징(Aging) :** 기다린 시간에 비례해 우선순위를 점차 높여줌.

## 6️⃣ 다단계 큐 (Multi-Level Queue)

<aside>
💡

우선순위별로 큐 분리

</aside>

![image.png](attachment:6bff2ec9-46a7-4096-ab11-edb635b4a9b8:image.png)

### 특징

- 큐 간 이동 ❌

## 7️⃣ 다단계 피드백 큐 (Multi-level Feedback Queue) ⭐

<aside>
💡

- 현대 OS에서 가장 많이 사용하는 완성형 알고리즘.
</aside>

![image.png](attachment:c609bc67-ae59-4344-a737-b7e5e126ebd8:image.png)

![image.png](attachment:22f9d77c-268d-4c58-87b7-b91da6f65a04:image.png)

- 프로세스가 타임 슬라이스 내에 작업을 못 마치면 아래 단계 큐로 내려감 (CPU 집중 프로세스로 판단).

### **핵심 흐름🔄**

1. 처음 → 높은 우선순위 큐
2. 오래 실행 → 낮은 큐로 이동
3. 짧은 작업 → 높은 큐 유지

### 효과🎯

- I/O 프로세스 우선 처리
- CPU 프로세스 뒤로 밀림

### 장점✅

- CPU 집중 프로세스는 자연스럽게 우선순위가 낮아지고, 짧은 I/O 프로세스는 높은 우선순위에서 빠르게 처리됨.

# **리눅스 스케줄링: CFS (Completely Fair Scheduler)**

<aside>
💡

**가상 실행 시간(vruntime):**

CPU를 적게 사용한 프로세스일수록 `vruntime`이 작습니다. CFS는 이 값이 가장 작은 프로세스를 다음에 실행합니다.

- vruntime 수식
  $$
  vrumtime =CPU를 할당받아 실행된 시간 *  \frac{평균 가중치(상수)}{프로세스의 가중치}
  $$

</aside>

<aside>
💡

**CFS의 가중치:**

우선순위가 높은 프로세스는 실제 실행 시간보다 `vruntime`이 천천히 증가하게 설계되어 CPU를 더 오래 점유할 수 있게 합니다.

</aside>

<aside>
💡

**RB 트리 (Red-Black Tree):**

수많은 프로세스 중 `vruntime`이 가장 작은 놈을 $O(\log N)$ 시간 복잡도로 빠르게 찾기 위해 사용합니다.

</aside>
