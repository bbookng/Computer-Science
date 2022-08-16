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
    - 낮은 우선순위 프로세스들이 CPU를 무한히 대기하는 경우
  - CPU 사용 시간을 미리 알 수 없다. 
- **Solution**
  - Aging : as time progresses **increase the priority** of the process.
  - 오랫동안 시스템에서 대기하는 프로세스들의 우선순위를 점진적으로 증가시킴

- 다음 CPU burst time을 어떻게 알 수 있는가?
  - 추정(estimate)이 가능하다.
  - 과거의 CPU burst time을 이용해서 추정 (exponential averaging)



### 📢 Round Robin (RR)

- 각 프로세스는 동일한 크기와 할당 시간 (time quantum)을 가짐 (일반적으로 10-100 ms)

- 할당 시간이 지나면 프로세스는 선점(preempted) 당하고 ready queue 의 제일 뒤에 가서 다시 줄을 선다

- n개의 프로세스가 ready queue 에 있고 할당 시간이 q time unit 인 경우 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n 을 얻는다.

  → 어떤 프로세스도 (n-1)q time unit 이상 기다리지 않는다.

- Performance

  - q large → FIFO
  - q small → context swith 오버헤드가 커진다

- 일반적으로 SJF 보다 average turnaround time이 길지만 **response time은 더 짧다.**

  

기본 베이스는 **준비 큐가 선입선출 큐**로 동작하게 만든다. 그 후 CPU 스케줄러는 준비 큐에서 첫 번째 프로세스를 선택해 **한 번의 시간 할당량 이후에 인터럽트**를 걸도록 타이머를 설정한 후, 프로세스를 **디스패치** 한다.

 

이때 두 가지 상황이 발생할 수 있다.

- 프로세스의 CPU 버스트가 한 번의 시간 할당량보다 작다 -> 프로세스 스스로 CPU를 방출하게 되고, 다음 프로세스로 이동
- 한 번의 시간 할당량보다 긴 경우 -> 인터럽트가 발생하고, 실행하던 프로세스는 준비 큐의 꼬리에 넣어진다.

 

다음과 같은 프로세스들이 대기 큐에 있다고 가정해보자.

| 프로세스 | 버스트 시간 |
| -------- | ----------- |
| P1       | 24 ms       |
| P2       | 3           |
| P3       | 3           |

 

만약 시간 할당량을 4 ms로 한다면, P1은 처음 4 ms 사용 후 나머지 20 ms는 맨 뒤로 가고, P2, P3는 바로 실행 후 종료한다.

![img](https://blog.kakaocdn.net/dn/bfDiXS/btrb9G3dok4/OLXnSwYgxOfkoKOg4NGwqk/img.png)

 

라운드 로빈 알고리즘의 성능은 **시간 할당량의 크기에 매우 많은 영향**을 받는다.

극단적인 경우, 시간 할당량이 매우 크면, 선입 선처리 정책과 다를 바 없다.

반대로 시간 할당량이 매우 적다면, 매우 많은 문맥 교환을 야기할 수 있다.

따라서 일반적으로 **시간 할당량이 문맥 교환 시간과 비교해 더 크도록 설정**한다.



### 📢 Multilevel Queue

- Ready Queue 를 여러 개로 분할
  - foreground (interactive)
  - background (batch - no human interaction)
- 각 큐는 독립적인 스케줄링 알고리즘을 가짐
  - foreground - RR
  - background - FCFS (context switch overhead를 줄이기 위해)
- 큐에 대한 스케줄링이 필요
  - **Fixed priority scheduling**
    - serve alll from foreground then from background.
    - Possibility of starvation.
  - **Time slice**
    - 각 큐에 CPU time을 적절한 비율로 할당
    - Ex : 80% to foreground in RR, 20% to baackground in FCFS
    - Starvation을 막기 위해서.



### 📢 Multilevel Feedback Queue

- 프로세스가 다른 큐로 이동 가능
- 에이징(aging)을 이와 같은 방식으로 구현할 수 있다
- Multilevel-feedback-queue scheduler를 정의하는 파라미터들
  - Queue의 수
  - 각 큐의 scheduling algorithm
  - Process를 상위 큐로 보내는 기준
  - Process를 하위 큐로 내쫓는 기준
  - 프로세스가 CPU 서비스를 받으려 할 때 들어갈 큐를 결정하는 기준

#### [Example of Multilevel Feedback Queue]

- Three queues:
  - Q₀ - time quantum 8 ms
  - Q₁ - time quantum 16 ms
  - Q₂ - FCFS
- Scheduling
  - new job이 queue Q₀로 들어감
  - CPU를 잡아서 할당 시간 8 ms 동안 수행됨
  - 8 ms 동안 다 끝내지 못했으면 queue Q₁으로 내려감
  - Q₁ 에 줄서서 기다렸다가 CPU를 잡아서 16ms 동안 수행됨
  - 16 ms 에 끝내지 못한 경우 queue Q₂로 쫓겨남



### 📢 Multiple-Processor Scheduling

- CPU가 여러 개인 경우 스케줄링은 더욱 복잡해짐
- **Homeogeneous processor 인 경우**
  - Queue 에 한줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있다.
  - 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에는 문제가 더 복잡해짐
- **Load sharing**
  - 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘 필요
- **Symmetric Multiprocessing (SMP)**
  - 각 프로세서가 각자 알아서 스케줄링 결정
- **Asymmetric multiprocessing**
  - 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름



### 📢 Real-Time Scheduling

- **Hard real-time systems**
  - Hare real-time task는 정해진 시간 안에 반드시 끝내도록 스케줄링 해야 함
- **Soft real-time conputing**
  - Soft real-time tast는 일반 프로세스에 비해 높은 priority를 갖도록 해야 함
  - deadline을 보장 못함



### 📢 Thread Scheduling

- **Local Scheduling**
  - User level thread의 경우 사용자 수준의 thread library에 의해 어떤 thread를 스케줄할지 결정

- **Global Scheduling**
  - Kernel level thread의 경우 일반 프로세스와 마찬 가지로 커널 단기 스케줄러가 어떤 thread를 스케줄할지 결정



## ✨ Algorithm Evaluation

- **Queueing models**
  - 확률 분포로 주어지는 arrival rate와 service rate 등을 통해 각종 performance index 값을 계산
- **Implementation(구현) & Measurement (성능 측정)**
  - 실제 시스템에 알고리즘을 구현하여 실제 작업 (workload) 에 대해서 성능을 측정 비교
- **Simulation (모의 실험)**
  - 알고리즘을 모의 프로그램을 작성 후 trace를 입력으로 하여 결과 비교

