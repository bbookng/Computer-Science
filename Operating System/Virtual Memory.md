# 💫 Virtual Memory

- 물리적인 메모리의 주소 변화는 운영 체제가 관여하지 않지만 가상 메모리 기법은 전적으로 운영체제가 관여하고 있다.
- Paging 기법을 사용하는 것으로 가정

## ✨ Demand Paging

- 실제로 필요할 때 page를 메모리에 올리는 것

  - I/O 양의 감소
  - Memory 사용량 감소
  - 빠른 응답 시간
  - 더 많은 사용자 수용

- Valid / Invalid bit의 사용

  - Invalid 의 의미

    - 사용되지 않는 주소 영역인 경우
    - 페이지가 물리적 메모리에 없는 경우

  - 처음에는 모든 page entry가 invalid로 초기화

  - Address translation 시에 invalid bit이 set 되어 있으면

    → **page fault** 



### 📌 Memory에 없는 Page의 Page Table

![image-20220912222456329](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912222456329.png)



### 📌 Page Fault

- invalid page 를 접근하면 MMU(주소전환을 해주는 하드웨어) 가 trap을 발생시킴 (page fault trap)
- Kernel mode 로 들어가서 page fault handler 가 invoke 됨
- 다음과 같은 순서로 page fault를 처리한다
  1. Invalid reference? (ex. bad address, protection viloation) → abort process.
  2. Get an empty page frame. (없으면 뺏어온다 : replace)
  3. 해당 페이지를 disk에서 memory로 읽어온다
     1. disk I/O가 끝나기까지 이 프로세스는 CPU를 preempt 당함 (block)
     2. Dist read가 끝나면 page tables entry 기록, valid/invalid bit = "valid"
     3. ready queue에 process를 insert → dispatch later
  4. 이 프로세스가 CPU를 잡고 다시 running
  5. 아까 중단되었던 instruction을 재개



### 📌 Steps in Handling a Page Fault

![image-20220912224514527](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912224514527.png)



### 📌 Performance of Demand Paging

- Page Fault Rate 0 <= p <= 1.0

  - if p = 0 no page faults
  - if p = 1, every reference is a fault

- Effective Access Time

  = (1 - p) x memory access

  +p (OS & HW page fault averhead
  +[swap page out if needed]
  +swap page in
  +OS & HW restart overhead)



### 📌 Free frame이 없는 경우

- **Page replacement**

  - 어떤 frame을 빼앗아올지 결정해야 함
  - 곧바로 사용되지 않을 page 를 쫓아내는 것이 좋음
  - 동일한 페이지가 여러 번 메모리에서 쫓겨났다가 다시 들어올 수 있음

- **Replacemetn Algorithm**

  - page-fault rate 를 최소화하는 것이 목표

  - 알고리즘의 평가

    - 주어진 page reference string에 대해 page fault를 얼마나 내는지 조사

  - reference string의 예

    1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5.



### 📌 Page Replacement

![image-20220912230315393](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912230315393.png)



### 📌 Optimal Algorithm

- MIN (OPT) : 가장 먼 미래에 참조되는 page 를 replace

- **4 frames example**

  ![image-20220912230414733](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912230414733.png)

- 미래의 참조를 어떻게 아는가?
  - Offline algorithm
- 다른 알고리즘의 성능에 대한 uppter bound 제공
  - Belady's optimal algorithm, MIN, OPT 등으로 불림



### 📌 FIFO (First In Fisrt Out) Algorithm

![image-20220912231145361](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912231145361.png)



### 📌 LRU (Least Recently Used) Algorithm

![image-20220912231454927](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912231454927.png)



### 📌 LFU (Least Frequently User) Algorithm

- LFU : 참조 횟수 (reference count) 가 가장 적은 페이지를 지움
  - 최저 참조 횟수인 page가 여럿 있는 경우
    - LFU 알고리즘 자체에서는 여러 page 중 임의로 선정한다
    - 성능 향상을 위해 가장 오래 전에 참조된 page를 지우게 구현할 수도 있다
  - 장단점
    - LRU 처럼 직전 참조 시점만 보는 것이 아니라 장기적인 시간 규모를 보기 때문에 page의 인기도를 좀 더 정확히 반영할 수 있음
    - 참조 시점의 최근성을 반영하지 못함
    - LRU 보다 구현이 복잡함



### 📌 LRU와 LFU 알고리즘 예제

![image-20220912232052655](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912232052655.png)



### 📌 LRU와 LFU 알고리즘의 구현

![image-20220912232551221](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912232551221.png)



### 📌 다양한 캐슁 환경

- 캐슁 기법
  - 한정된 빠른 공간(=캐쉬)에 요청된 데이터를 저장해 두었다가 후속 요청시 캐쉬로부터 직접 서비스하는 방식
  - paging system 외에도 cache memory, buffer caching, Web caching 등 다양한 분야에서 사용
- 캐쉬 운영의 시간 제약
  - 교체 알고리즘에서 삭제할 항목을 결정하는 일에 지나치게 많은 시간이 걸리는 경우 실제 시스템에서 사용할 수 없음
  - Buffer caching 이나 Web caching 의 경우
    - O(1) 에서 O(log n) 정도까지 허용
  - Paging system 인 경우
    - page fault 인 경우에만 OS가 관여함
    - 페이지가 이미 메모리에 존재하는 경우 참조시각 등의 정보를 OS가 알 수 없음
    - O(1)인 LRU의 list 조작조차 불가능