# 💫 CPU Scheduling

- 비선점형 nonpreemptive
- 선점형 preemptive



## ✨ Scheduling Criteria (성능 척도)

#### Performance Index ( = Performance Measuer)

- **CPU utilization (이용률)**
  - CPU를 최대한 바쁘게 유지시키는 것, CPU가 놀지 않고 일한 시간을 나타내는 비율
- **Throughput (처리량)**
  - CPU 작업량 측정의 한 방법, **단위 시간 당 완료된 프로세스의 개수**를 의미
- **Turnaround time (총 처리 시간)**
  - 프로세스의 제출 시간과 완료 시간의 간격
  - (1) 준비 큐에서 대기한 시간, (2) CPU에서 실행하는 시간, (3)  I/O 시간 (1)~(3)을 합한 시간
- **Waiting time (대기 시간)**
  - 준비 큐에서 대기하면 보낸 시간의 합
- **Response tirme (응답 시간)**
  - 응답이 시작되는 데까지 걸리는 시간. 하나의 요구를 제출한 후 첫 번째 응답이 나올때까지의 시간
  - 레디 큐에서 처음으로 CPU 를 얻기 까지의 시간



- 위에 두개는  System 입장에서의 성능척도
- 밑에 세개는 Program 입장에서의 성능척도



## ✨ CPU Algorithm

### 📢 FCFS (First-Come First-Served)

- 비선점형 스케줄링

- 선입선출 큐

다음과 같은 프로세스들이 대기 큐에 있다고 가정해보자.

| 프로세스 | 버스트 시간 |
| -------- | ----------- |
| P1       | 24 ms       |
| P2       | 3           |
| P3       | 3           |

만약 프로세스들이 P1 ~ P3 순으로 선입 선출 방식이 적용된다면, 각 프로세스의 대기 시간은 다음과 같다.

![img](https://blog.kakaocdn.net/dn/dhl2zR/btrb5owcDxB/Z4T2OdneiHKc8RJL7pUcZ0/img.png)

P1 = 0 ms, P2 = 24 ms, P3 = 27 ms -> **평균 17 ms**

그러나 프로세스들이 P2, P3, P1 순으로 도착하면, 각 프로세스들의 대기 시간은 다음과 같다.

![img](https://blog.kakaocdn.net/dn/nthjz/btrb2lmetXn/tOoVx5PrckECkqmscxa2oK/img.png)

P1 = 6, P2 = 0, P3 = 3 -> **평균 3 ms**

따라서 선입 선처리 스케줄링의 **평균대기 시간은 일반적으로 최소가 아니다.**

- 호위효과 (convoy effect)
  - 모든 다른 프로세스들이 하나의 긴 프로세스가 CPU를 양도하기를 기다리는 것
  - 짧은 프로세스들이 먼저 처리되도록 허용될 때 이득

### 📢 SJF (Shortest-Job-First)

- 각 프로세스와 다음번 **CPU burst time**을 가지고 스케줄링에 활용
- **CPU burst time**이 가장 짧은 프로세스를 제일 먼저 스케줄
- **Two schemes**:
  - **Nonpreemptive**
    - 일단 CPU를 잡으면 이번 CPU brust가 완료될 때까지 CPU를 선점(preemptive) 당하지 않음
  - **Preemptive**
    - 현재 수행중인 프로세스의 남은 burst time보다 더 짧은 CPU burst time을 가지는 새로운 프로세스가 도착하면  CPU를 빼앗김
    - 이 방법을 Shortest-Remaining-Time-First (SRTF) 이라고도 부른다
- **SJF is optimal**
  - 주어진 프로세스들에 대해 minimum average waiting time을 보장
  - Preemptive 



### 📢 Priority Scheduling

- A priority number (integer) is associated with each process
- **hightest priority** 를 가진 프로세스에게 CPU 할당 (smallest integer = highest priority).
  - Preemptive
  - NonPreemptive
- SJF는 일종의 우선순위 스케줄링이다.
  - priority = predicted next CPU burst time
- **Problem** 
  - Starvation : low priority processes may **never execute**
    - 낮은 우선수위 프로세스들이 CPU를 무한히 대기하는 경우
  - CPU 사용 시간을 미리 알 수 없다. 
- **Solution**
  - Aging : as time progresses **increase the priority** of the process.

- 다음 CPU burst time을 어떻게 알 수 있는가?
  - 추정(estimate)이 가능하다.
  - 과거의 CPU burst time을 이용해서 추정 (exponential averaging)