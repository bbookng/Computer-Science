# ๐ซ Process Synchronization

## โจ ๋ฐ์ดํฐ์ ์ ๊ทผ

![image-20220821145311246](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220821145311246.png)

- **Execution-box**

  1. CPU
  2. ์ปดํจํฐ๋ด๋ถ
  3. ํ๋ก์ธ์ค

- **Storage-box**

  1. Memory
  2. ๋์คํฌ
  3. ๊ทธ ํ๋ก์ธ์ค์ ์ฃผ์ ๊ณต๊ฐ

  

## โจ Race Condition

![image-20220821152408623](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220821152408623.png)

- **S-box(Memory, Address Space)**๋ฅผ ๊ณต์ ํ๋ **E-box(CPU, Process)**๊ฐ ์ฌ๋ฟ ์๋ ๊ฒฝ์ฐ **Race Condition**์ ๊ฐ๋ฅ์ฑ์ด ์์

- Memory, CPU - Multiprocessor system

- Address Space, Process 

  - ๊ณต์ ๋ฉ๋ชจ๋ฆฌ๋ฅผ ์ฌ์ฉํ๋ ํ๋ก์ธ์ค๋ค
  - ์ปค๋ ๋ด๋ถ ๋ฐ์ดํฐ๋ฅผ ์ ๊ทผํ๋ ๋ฃจํด๋ค ๊ฐ (ex : ์ปค๋๋ชจ๋ ์ํ ์ค ์ธํฐ๋ฝํธ๋ก ์ปค๋๋ชจ๋ ๋ค๋ฅธ ๋ฃจํด ์ํ ์)

  

### ๐ข OS์์ race condition์ ์ธ์  ๋ฐ์ํ๋๊ฐ ?

1. **kernel ์ํ ์ค ์ธํฐ๋ฝํธ ๋ฐ์ ์**

   ![image-20220821153107515](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220821153107515.png)

   - ์ปค๋๋ชจ๋ running ์ค interrupt๊ฐ ๋ฐ์ํ์ฌ ์ธํฐ๋ฝํธ ์ฒ๋ฆฌ๋ฃจํด์ด ์ํ
     - ์์ชฝ ๋ค ์ปค๋ ์ฝ๋์ด๋ฏ๋ก kernel address space ๊ณต์ 

2. **Process๊ฐ system call์ ํ์ฌ kernel mode๋ก ์ํ ์ค์ธ๋ฐ context switch๊ฐ ์ผ์ด๋๋ ๊ฒฝ์ฐ**

   ![image-20220821153302134](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220821153302134.png)

   - ๋ ํ๋ก์ธ์ค์ address space ๊ฐ์๋ data sharing์ด ์์
   - ๊ทธ๋ฌ๋  system call์ ํ๋ ๋์์๋ kernel address space์ data๋ฅผ accessํ๊ฒ ๋จ (share)
   - ์ด ์์ ์ค๊ฐ์ CPU๋ฅผ preempt ํด๊ฐ๋ฉด race condition ๋ฐ์

   ![image-20220821153436588](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220821153436588.png)

   - ํด๊ฒฐ์ฑ 
     - ์ปค๋ ๋ชจ๋์์ ์ํ ์ค์ผ ๋๋ CPU๋ฅผ preempt ํ์ง ์์
     - ์ปค๋ ๋ชจ๋์์ ์ฌ์ฉ์ ๋ชจ๋๋ก ๋์๊ฐ ๋ preempt

3. **Multiprocessor์์ shared memory ๋ด์ kernel data**

   ![image-20220821165044772](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220821165044772.png)

   - ์ด๋ค CPU๊ฐ ๋ง์ง๋ง์ผ๋ก count๋ฅผ store ํ๋๊ฐ ? > race condition
   - multiprocessor์ ๊ฒฝ์ฐ interrupt enable/disable ๋ก ํด๊ฒฐ๋์ง ์์
   - **๋ฐฉ๋ฒ**
     1. ํ๋ฒ์ ํ๋์ CPU๋ง์ด ์ปค๋์ ๋ค์ด๊ฐ ์ ์๊ฒ ํ๋ ๋ฐฉ๋ฒ
     2. ์ปค๋ ๋ด๋ถ์ ์๋ ๊ฐ ๊ณต์  ๋ฐ์ดํฐ์ ์ ๊ทผํ  ๋๋ง๋ค ๊ทธ ๋ฐ์ดํฐ์ ๋ํ lock / unlock์ ํ๋ ๋ฐฉ๋ฒ



## โจ Process Synchronization ๋ฌธ์ 

- ๊ณต์  ๋ฐ์ดํฐ (shared data)์ ๋์ ์ ๊ทผ (concurrent access)์ ๋ฐ์ดํฐ์ ๋ถ์ผ์น ๋ฌธ์ (inconsistency)๋ฅผ ๋ฐ์์ํฌ ์ ์๋ค
- ์ผ๊ด์ฑ(consistency) ์ ์ง๋ฅผ ์ํด์๋ ํ๋ ฅ ํ๋ก์ธ์ค (cooperating process) ๊ฐ์ ์คํ ์์ (orderly execution)๋ฅผ ์ ํด์ฃผ๋ ๋ฉ์ปค๋์ฆ ํ์
- **Race condition**
  - ์ฌ๋ฌ ํ๋ก์ธ์ค๋ค์ด ๋์์ ๊ณต์  ๋ฐ์ดํฐ๋ฅผ ์ ๊ทผํ๋ ์ํฉ
  - ๋ฐ์ดํฐ์ ์ต์ข ์ฐ์ฐ ๊ฒฐ๊ณผ๋ ๋ง์ง๋ง์ ๊ทธ ๋ฐ์ดํฐ๋ฅผ ๋ค๋ฃฌ ํ๋ก์ธ์ค์ ๋ฐ๋ผ ๋ฌ๋ผ์ง
- race condition์ ๋ง๊ธฐ ์ํด์๋ concurrent process๋ ๋๊ธฐํ (synchronize) ๋์ด์ผ ํ๋ค



## โจ The Critical-Section Problem

- n๊ฐ์ ํ๋ก์ธ์ค๊ฐ ๊ณต์  ๋ฐ์ดํฐ๋ฅผ ๋์์ ์ฌ์ฉํ๊ธฐ๋ฅผ ์ํ๋ ๊ฒฝ์ฐ
- ๊ฐ ํ๋ก์ธ์ค์ code segment์๋ ๊ณต์  ๋ฐ์ดํฐ๋ฅผ ์ ๊ทผํ๋ ์ฝ๋์ธ critical section์ด ์กด์ฌ
- **Problem**
  - ํ๋์ ํ๋ก์ธ์ค๊ฐ critical section์ ์์ ๋ ๋ค๋ฅธ ๋ชจ๋  ํ๋ก์ธ์ค๋ critical section์ ๋ค์ด๊ฐ ์ ์์ด์ผ ํ๋ค.



## โจ Initial Attempts to Solve Problem

- ๋ ๊ฐ์ ํ๋ก์ธ์ค๊ฐ ์๋ค๊ณ  ๊ฐ์  Pโ, Pโ
- ํ๋ก์ธ์ค๋ค์ ์ผ๋ฐ์ ์ธ ๊ตฌ์กฐ

![image-20220821170410325](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220821170410325.png)

- ํ๋ก์ธ์ค๋ค์ ์ํ์ ๋๊ธฐํ(synchronize)๋ฅผ ์ํด ๋ช๋ช ๋ณ์๋ฅผ ๊ณต์ ํ  ์ ์๋ค โ synchronization variable



## โจ ํ๋ก๊ทธ๋จ์  ํด๊ฒฐ๋ฒ์ ์ถฉ์กฑ ์กฐ๊ฑด

1. **Mutual Exclusion (์ํธ ๋ฐฐ์ )**
   - ํ๋ก์ธ์ค Pi๊ฐ critical section ๋ถ๋ถ์ ์ํ ์ค์ด๋ฉด ๋ค๋ฅธ ๋ชจ๋  ํ๋ก์ธ์ค๋ค์ ๊ทธ๋ค์ critical section์ ๋ค์ด๊ฐ๋ฉด ์๋๋ค
2. **Progress (์งํ)**
   - ์๋ฌด๋ critical section์ ์์ง ์์ ์ํ์์ critical section์ ๋ค์ด๊ฐ๊ณ ์ ํ๋ ํ๋ก์ธ์ค๊ฐ ์์ผ๋ฉด critical section์ ๋ค์ด๊ฐ๊ฒ ํด์ฃผ์ด์ผ ํ๋ค
3. **Bounded Waiting (์ ํ ๋๊ธฐ)**
   - ํ๋ก์ธ์ค๊ฐ critical section์ ๋ค์ด๊ฐ๋ ค๊ณ  ์์ฒญํ ํ๋ถํฐ ๊ทธ ์์ฒญ์ด ํ์ฉ๋  ๋๊น์ง ๋ค๋ฅธ ํ๋ก์ธ์ค๋ค์ด critical section์ ๋ค์ด๊ฐ๋ ํ์์ ํ๊ณ๊ฐ ์์ด์ผ ํ๋ค

- ๊ฐ์ 
  - ๋ชจ๋  ํ๋ก์ธ์ค์ ์ํ ์๋๋ 0๋ณด๋ค ํฌ๋ค
  - ํ๋ก์ธ์ค๋ค ๊ฐ์ ์๋์ ์ธ ์ํ ์๋๋ ๊ฐ์ ํ์ง ์๋๋ค


### ๐ข Algorithm 1

- Synchronization variable

  ```
  int turn;
  initially turn = 0; # Pi can enter its critical section if (turn == i)
  ```

- Process Pโ

  ![image-20220821181454012](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220821181454012.png)

- Satisfies mutual exclusion, but **not progress** >> Progress ์กฐ๊ฑด์ ๋ง์กฑํ์ง ๋ชปํจ

- **๊ณผ์ ์๋ณด**

  - ๋ฐ๋์ ํ๋ฒ์ฉ ๊ต๋๋ก ๋ค์ด๊ฐ์ผ๋ง ํจ (swap-turn)
  - ๊ทธ๊ฐ turn ์ ๋ด๊ฐ์ผ๋ก ๋ฐ๊ฟ์ค์ผ๋ง ๋ด๊ฐ ๋ค์ด๊ฐ ์ ์์
  - ํน์  ํ๋ก์ธ์ค๊ฐ ๋ ๋น๋ฒํ critical section์ ๋ค์ด๊ฐ์ผ ํ๋ค๋ฉด ?



### ๐ข Algorithm 2

- Synchronization variables

  - **boolean flag[2];**

    initially flat [๋ชจ๋] == false /* no one is in CS*/

  - "Pi ready to enter its critical section" if (flag [i] == true)

- Process Pi

![image-20220821184817364](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220821184817364.png)

- Satisfies mutual exclusion, but **not progress requirement**
- ๋ ๋ค 2ํ๊น์ง ์ํ ํ ๋์์์ด ์๋ณดํ๋ ์ํฉ ๋ฐ์ 
- ๋ ๋ค ๊น๋ฐ๋ง ๋ค์์ง ๊ธฐ๋ค๋ฆฌ๋ค๊ฐ ๋ชป๋ค์ด๊ฐ



### ๐ข Algorithm 3 ( Peterson's Algorithm )

- Combined synchronization variables of algorithms 1 and 2.
- Process Pi

![image-20220821185110586](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220821185110586.png)

- Meets all three requirements; solves the critical section problem for two processes.
- **Busy Waiting!**(=spin lock) (๊ณ์ CPU์ memory๋ฅผ ์ฐ๋ฉด์ wait)



## โจ Synchronization Hardware

- ํ๋์จ์ด์ ์ผ๋ก **Test & modify** ๋ฅผ atomic ํ๊ฒ ์ํํ  ์ ์๋๋ก ์ง์ํ๋ ๊ฒฝ์ฐ ์์ ๋ฌธ์ ๋ ๊ฐ๋จํ ํด๊ฒฐ

![image-20220821185611549](assets/image-20220821185611549.png)

- Mutual Exclusion with Test & Set

  ![image-20220821185642479](assets/image-20220821185642479.png)



## โจ Semaphores

- ์์ ๋ฐฉ์๋ค์ ์ถ์ํ์ํด
- **Semaphore S**
  - integer variable
  - ์๋์ ๋ ๊ฐ์ง atomic ์ฐ์ฐ์ ์ํด์๋ง ์ ๊ทผ ๊ฐ๋ฅ

![image-20220821190011277](assets/image-20220821190011277.png)



### ๐ข Critical Section of n Processes

![image-20220821190519065](assets/image-20220821190519065.png)

- busy-wait ๋ ํจ์จ์ ์ด์ง ๋ชปํจ (= spin lock)
- Block & Wakeup ๋ฐฉ์์ ๊ตฌํ (= sleep lock)
- 

### ๐ข Block / Wakeup Implementation

- Semaphore๋ฅผ ๋ค์๊ณผ ๊ฐ์ด ์ ์

![image-20220821190854752](assets/image-20220821190854752.png)

- block ๊ณผ wakeup์ ๋ค์๊ณผ ๊ฐ์ด ๊ฐ์ 
  - **block**
    - ์ปค๋์ block์ ํธ์ถํ ํ๋ก์ธ์ค๋ฅด๋ฅผ suspend ์ํด
    - ์ด ํ๋ก์ธ์ค์ PCB๋ฅผ semaphore์ ๋ํ wait queue์ ๋ฃ์
  - **wakeup(P)**
    - block ๋ ํ๋ก์ธ์ค P๋ฅผ wakeup ์ํด
    - ์ด ํ๋ก์ธ์ค์ PCB๋ฅผ ready queue๋ก ์ฎ๊น

![image-20220821191020933](assets/image-20220821191020933.png)



#### [ Implementation ] - block / wakeup version of P() & ใ()

- Semaphore ์ฐ์ฐ์ด ์ด์  ๋ค์๊ณผ ๊ฐ์ด ์ ์๋จ

![image-20220821205534845](assets/image-20220821205534845.png)

- ์์์ ๋ฐ๋ฉํ๊ณ  ๋๋๋ ๊ฒ์ด ์๋๋ผ ํน์ ์์์ ๊ธฐ๋ค๋ฆฌ๋ ์ฐ์ฐ์ด ์๋ค๋ฉด ๊ทธ ์ฐ์ฐ์ ๊นจ์์ค



### ๐ข Which is better ?

- **Busy-wait** v.s. **Block/wakeup**

- **Block/wakeup overhead** v.s. **Critical section** ๊ธธ์ด
  - Critical section์ ๊ธธ์ด๊ฐ ๊ธด ๊ฒฝ์ฐ Block/Wakeup์ด ์ ๋น
  - Critical section์ ๊ธธ์ด๊ฐ ๋งค์ฐ ์งง์ ๊ฒฝ์ฐ Block/Wakeup ์ค๋ฒํค๋๊ฐ busy-wait ์ค๋ฒํค๋๋ณด๋ค ๋ ์ปค์ง ์ ์์
  - ์ผ๋ฐ์ ์ผ๋ก๋ Block/Wakeup ๋ฐฉ์์ด ๋ ์ข์



### ๐ข Two Types of Semaphores

1. **Counting semaphore**
   - ๋๋ฉ์ธ์ด 0 ์ด์์ธ ์์์ ์ ์๊ฐ
   - ์ฃผ๋ก resource counting์ ์ฌ์ฉ
2. **Binary semaphore**
   - 0 ๋๋ 1 ๊ฐ๋ง ๊ฐ์ง ์ ์๋ semaphore
   - ์ฃผ๋ก mutual exclusion (lock/unlock) ์ ์ฌ์ฉ



### ๐ข Deadlock and Starvation

- **Deadlock**

  - ๋ ์ด์์ ํ๋ก์ธ์ค๊ฐ ์๋ก ์๋๋ฐฉ์ ์ํด ์ถฉ์กฑ๋  ์ ์๋ event๋ฅผ ๋ฌดํํ ๊ธฐ๋ค๋ฆฌ๋ ํ์

- S์ Q๊ฐ 1๋ก ์ด๊ธฐํ๋ semaphore๋ผ ํ์.

  ![image-20220821212221836](assets/image-20220821212221836.png)

- **Starvation**
  - **indefinite blocking**. ํ๋ก์ธ์ค๊ฐ suspend๋ ์ด์ ์ ํด๋นํ๋ ์ธ๋งํฌ์ด ํ์์ ๋น ์ ธ๋๊ฐ ์ ์๋ ํ์

- ์์์ ํ๋ํ๋ ์์๋ฅผ ๋๊ฐ์ด ๋ง์ถฐ์ฃผ๋ฉด ํด๊ฒฐํ  ์ ์์



## โจ Classical Problems of Synchronization

1. Bounded-Buffer Problem (Producer-Consumer Problem)
2. Readers and Writers Problem
3. Dining-Philosophers Problem

### ๐ข Bounded-Buffer Problem (Producer-Consumer Problem)

![image-20220822104310366](assets/image-20220822104310366.png)

- **Shared data**
  - buffer ์์ฒด ๋ฐ buffer ์กฐ์ ๋ณ์ (empty / full buffer์ ์์ ์์น)
- **Synchronization variables**
  - mutual exclusion โ Need binary semaphore (shared data์ mutual exclusion์ ์ํด)
  - resource count โ Need integer semaphore (๋จ์ full/empty buffer์ ์ ํ์)

![image-20220822120839477](assets/image-20220822120839477.png)

### ๐ข Readers-Writers Problem

- ํ process๊ฐ DB์ write ์ค์ผ ๋ ๋ค๋ฅธ process๊ฐ ์ ๊ทผํ๋ฉด ์๋จ
- read๋ ๋์์ ์ฌ๋ฟ์ด ํด๋ ๋จ
- **solution**
  - Writer๊ฐ DB์ ์ ๊ทผ ํ๊ฐ๋ฅผ ์์ง ์ป์ง ๋ชปํ ์ํ์์๋ ๋ชจ๋  ๋๊ธฐ์ค์ธ Reader๋ค์ ๋ค DB์ ์ ๊ทผํ๊ฒ ํด์ค๋ค
  - Writer๋ ๋๊ธฐ ์ค์ธ Reader๊ฐ ํ๋๋ ์์ ๋ DB ์ ๊ทผ์ด ํ์ฉ๋๋ค
  - ์ผ๋จ Writer๊ฐ DB์ ์ ๊ทผ์ค์ด๋ฉด Reader๋ค์ ์ ๊ทผ์ด ๊ธ์ง๋๋ค
  - Writer๊ฐ DB์์ ๋น ์ ธ๋๊ฐ์ผ๋ง Reader์ ์ ๊ทผ์ด ํ์ฉ๋๋ค

#### ๐ก Shared data

- DB ์์ฒด
- readcount
  - ํ์ฌ DB์ ์ ๊ทผ ์ค์ธ Reader์ ์ 

#### ๐ก Synchronization variables

- mutex 
  - ๊ณต์  ๋ณ์ readcount๋ฅผ ์ ๊ทผํ๋ ์ฝ๋(critical section)์ mutual exclusion ๋ณด์ฅ์ ์ํด ์ฌ์ฉ
- db
  - Reader์ writer๊ฐ ๊ณต์  DB ์์ฒด๋ฅผ ์ฌ๋ฐ๋ฅด๊ฒ ์ ๊ทผํ๊ฒ ํ๋ ์ญํ 

![image-20220822120820292](assets/image-20220822120820292.png)



### ๐ข Dining-Philosophers Problem

- **Synchronization variables**

```c
semaphore chopstick[5];
/* Initially all values are 1*/
```

- **Philosopher i**

```c
do{
    P(chopstick[i];)
    P(chopstick[(i+1)%5]);
    ...
    eat();
    ...
    V(chopstick[i]);
    V(chopstick[(i+1)%5]);
    ...
    think();
    ...
} while(1);
```

- ์์ solution์ ๋ฌธ์ ์ 
  - Deadlock ๊ฐ๋ฅ์ฑ์ด ์๋ค
  - ๋ชจ๋  ์ฒ ํ์๊ฐ ๋์์ ๋ฐฐ๊ฐ ๊ณ ํ์ ธ ์ผ์ชฝ ์ ๊ฐ๋ฝ์ ์ง์ด๋ฒ๋ฆฐ ๊ฒฝ์ฐ
- ํด๊ฒฐ๋ฐฉ์
  - 4๋ช์ ์ฒ ํ์๋ง์ด ํ์ด๋ธ์ ๋์์ ์์ ์ ์๋๋ก ํ๋ค
  - ์ ๊ฐ๋ฝ์ ๋ ๊ฐ ๋ชจ๋ ์ง์ ์ ์์ ๋์๋ง ์ ๊ฐ๋ฝ์ ์ง์ ์ ์๊ฒ ํ๋ค
  - ๋น๋์นญ
    - ์ง์(ํ์) ์ฒ ํ์๋ ์ผ์ชฝ(์ค๋ฅธ์ชฝ) ์ ๊ฐ๋ฝ๋ถํฐ ์ง๋๋ก

#### ๐กMonitor

- Semaphore์ ๋ฌธ์ ์ 
  - ์ฝ๋ฉํ๊ธฐ ํ๋ค๋ค
  - ์ ํ์ฑ(correctness)์ ์์ฆ์ด ์ด๋ ต๋ค
  - ์๋ฐ์  ํ๋ ฅ(voluntary cooperation)์ด ํ์ํ๋ค
  - ํ๋ฒ์ ์ค์๊ฐ ๋ชจ๋  ์์คํ์ ์น๋ช์  ์ํฅ

![image-20220822171150022](assets/image-20220822171150022.png)

- ๋์ ์ํ์ค์ธ ํ๋ก์ธ์ค ์ฌ์ด์์ abstract data type์ ์์ ํ ๊ณต์ ๋ฅผ ๋ณด์ฅํ๊ธฐ ์ํ high-level synchronization construct

- ๋ชจ๋ํฐ ๋ด์์๋ ํ๋ฒ์ ํ๋์ ํ๋ก์ธ์ค๋ง์ด ํ๋ ๊ฐ๋ฅ

- ํ๋ก๊ทธ๋๋จธ๊ฐ ๋๊ธฐํ ์ ์ฝ ์กฐ๊ฑด์ ๋ช์์ ์ผ๋ก ์ฝ๋ฉํ  ํ์์์

- ํ๋ก์ธ์ค๊ฐ ๋ชจ๋ํฐ ์์์ ๊ธฐ๋ค๋ฆด ์ ์๋๋ก ํ๊ธฐ ์ํด condition variable ์ฌ์ฉ

  `condition x, y;`

- Condition variable์ wait์ signal ์ฐ์ฐ์ ์ํด์๋ง ์ ๊ทผ ๊ฐ๋ฅ 

  `x.wait();`

  - `x.wait()` ์ invoke ํ ํ๋ก์ธ์ค๋ ๋ค๋ฅธ ํ๋ก์ธ์ค๊ฐ `x.signal()`์ invoke ํ๊ธฐ ์ ๊น์ง suspend ๋๋ค

  `x.signal()`

  - `x.signal()` ์ ์ ํํ๊ฒ ํ๋์ suspend ๋ ํ๋ก์ธ์ค๋ฅผ resume ํ๋ค.
  - Suspend ๋ ํ๋ก์ธ์ค๊ฐ ์์ผ๋ฉด ์๋ฌด ์ผ๋ ์ผ์ด๋์ง ์๋๋ค.