# ๐ซ Virtual Memory

- ๋ฌผ๋ฆฌ์ ์ธ ๋ฉ๋ชจ๋ฆฌ์ ์ฃผ์ ๋ณํ๋ ์ด์ ์ฒด์ ๊ฐ ๊ด์ฌํ์ง ์์ง๋ง ๊ฐ์ ๋ฉ๋ชจ๋ฆฌ ๊ธฐ๋ฒ์ ์ ์ ์ผ๋ก ์ด์์ฒด์ ๊ฐ ๊ด์ฌํ๊ณ  ์๋ค.
- Paging ๊ธฐ๋ฒ์ ์ฌ์ฉํ๋ ๊ฒ์ผ๋ก ๊ฐ์ 

## โจ Demand Paging

- ์ค์ ๋ก ํ์ํ  ๋ page๋ฅผ ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ฆฌ๋ ๊ฒ

  - I/O ์์ ๊ฐ์
  - Memory ์ฌ์ฉ๋ ๊ฐ์
  - ๋น ๋ฅธ ์๋ต ์๊ฐ
  - ๋ ๋ง์ ์ฌ์ฉ์ ์์ฉ

- Valid / Invalid bit์ ์ฌ์ฉ

  - Invalid ์ ์๋ฏธ

    - ์ฌ์ฉ๋์ง ์๋ ์ฃผ์ ์์ญ์ธ ๊ฒฝ์ฐ
    - ํ์ด์ง๊ฐ ๋ฌผ๋ฆฌ์  ๋ฉ๋ชจ๋ฆฌ์ ์๋ ๊ฒฝ์ฐ

  - ์ฒ์์๋ ๋ชจ๋  page entry๊ฐ invalid๋ก ์ด๊ธฐํ

  - Address translation ์์ invalid bit์ด set ๋์ด ์์ผ๋ฉด

    โ **page fault** 



### ๐ Memory์ ์๋ Page์ Page Table

![image-20220912222456329](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912222456329.png)



### ๐ Page Fault

- invalid page ๋ฅผ ์ ๊ทผํ๋ฉด MMU(์ฃผ์์ ํ์ ํด์ฃผ๋ ํ๋์จ์ด) ๊ฐ trap์ ๋ฐ์์ํด (page fault trap)
- Kernel mode ๋ก ๋ค์ด๊ฐ์ page fault handler ๊ฐ invoke ๋จ
- ๋ค์๊ณผ ๊ฐ์ ์์๋ก page fault๋ฅผ ์ฒ๋ฆฌํ๋ค
  1. Invalid reference? (ex. bad address, protection viloation) โ abort process.
  2. Get an empty page frame. (์์ผ๋ฉด ๋บ์ด์จ๋ค : replace)
  3. ํด๋น ํ์ด์ง๋ฅผ disk์์ memory๋ก ์ฝ์ด์จ๋ค
     1. disk I/O๊ฐ ๋๋๊ธฐ๊น์ง ์ด ํ๋ก์ธ์ค๋ CPU๋ฅผ preempt ๋นํจ (block)
     2. Dist read๊ฐ ๋๋๋ฉด page tables entry ๊ธฐ๋ก, valid/invalid bit = "valid"
     3. ready queue์ process๋ฅผ insert โ dispatch later
  4. ์ด ํ๋ก์ธ์ค๊ฐ CPU๋ฅผ ์ก๊ณ  ๋ค์ running
  5. ์๊น ์ค๋จ๋์๋ instruction์ ์ฌ๊ฐ



### ๐ Steps in Handling a Page Fault

![image-20220912224514527](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912224514527.png)



### ๐ Performance of Demand Paging

- Page Fault Rate 0 <= p <= 1.0

  - if p = 0 no page faults
  - if p = 1, every reference is a fault

- Effective Access Time

  = (1 - p) x memory access

  +p (OS & HW page fault averhead
  +[swap page out if needed]
  +swap page in
  +OS & HW restart overhead)



### ๐ Free frame์ด ์๋ ๊ฒฝ์ฐ

- **Page replacement**

  - ์ด๋ค frame์ ๋นผ์์์ฌ์ง ๊ฒฐ์ ํด์ผ ํจ
  - ๊ณง๋ฐ๋ก ์ฌ์ฉ๋์ง ์์ page ๋ฅผ ์ซ์๋ด๋ ๊ฒ์ด ์ข์
  - ๋์ผํ ํ์ด์ง๊ฐ ์ฌ๋ฌ ๋ฒ ๋ฉ๋ชจ๋ฆฌ์์ ์ซ๊ฒจ๋ฌ๋ค๊ฐ ๋ค์ ๋ค์ด์ฌ ์ ์์

- **Replacemetn Algorithm**

  - page-fault rate ๋ฅผ ์ต์ํํ๋ ๊ฒ์ด ๋ชฉํ

  - ์๊ณ ๋ฆฌ์ฆ์ ํ๊ฐ

    - ์ฃผ์ด์ง page reference string์ ๋ํด page fault๋ฅผ ์ผ๋ง๋ ๋ด๋์ง ์กฐ์ฌ

  - reference string์ ์

    1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5.



### ๐ Page Replacement

![image-20220912230315393](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912230315393.png)



### ๐ Optimal Algorithm

- MIN (OPT) : ๊ฐ์ฅ ๋จผ ๋ฏธ๋์ ์ฐธ์กฐ๋๋ page ๋ฅผ replace

- **4 frames example**

  ![image-20220912230414733](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912230414733.png)

- ๋ฏธ๋์ ์ฐธ์กฐ๋ฅผ ์ด๋ป๊ฒ ์๋๊ฐ?
  - Offline algorithm
- ๋ค๋ฅธ ์๊ณ ๋ฆฌ์ฆ์ ์ฑ๋ฅ์ ๋ํ uppter bound ์ ๊ณต
  - Belady's optimal algorithm, MIN, OPT ๋ฑ์ผ๋ก ๋ถ๋ฆผ



### ๐ FIFO (First In Fisrt Out) Algorithm

![image-20220912231145361](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912231145361.png)



### ๐ LRU (Least Recently Used) Algorithm

![image-20220912231454927](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912231454927.png)



### ๐ LFU (Least Frequently User) Algorithm

- LFU : ์ฐธ์กฐ ํ์ (reference count) ๊ฐ ๊ฐ์ฅ ์ ์ ํ์ด์ง๋ฅผ ์ง์
  - ์ต์  ์ฐธ์กฐ ํ์์ธ page๊ฐ ์ฌ๋ฟ ์๋ ๊ฒฝ์ฐ
    - LFU ์๊ณ ๋ฆฌ์ฆ ์์ฒด์์๋ ์ฌ๋ฌ page ์ค ์์๋ก ์ ์ ํ๋ค
    - ์ฑ๋ฅ ํฅ์์ ์ํด ๊ฐ์ฅ ์ค๋ ์ ์ ์ฐธ์กฐ๋ page๋ฅผ ์ง์ฐ๊ฒ ๊ตฌํํ  ์๋ ์๋ค
  - ์ฅ๋จ์ 
    - LRU ์ฒ๋ผ ์ง์  ์ฐธ์กฐ ์์ ๋ง ๋ณด๋ ๊ฒ์ด ์๋๋ผ ์ฅ๊ธฐ์ ์ธ ์๊ฐ ๊ท๋ชจ๋ฅผ ๋ณด๊ธฐ ๋๋ฌธ์ page์ ์ธ๊ธฐ๋๋ฅผ ์ข ๋ ์ ํํ ๋ฐ์ํ  ์ ์์
    - ์ฐธ์กฐ ์์ ์ ์ต๊ทผ์ฑ์ ๋ฐ์ํ์ง ๋ชปํจ
    - LRU ๋ณด๋ค ๊ตฌํ์ด ๋ณต์กํจ



### ๐ LRU์ LFU ์๊ณ ๋ฆฌ์ฆ ์์ 

![image-20220912232052655](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912232052655.png)



### ๐ LRU์ LFU ์๊ณ ๋ฆฌ์ฆ์ ๊ตฌํ

![image-20220912232551221](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220912232551221.png)



### ๐ ๋ค์ํ ์บ์ ํ๊ฒฝ

- ์บ์ ๊ธฐ๋ฒ
  - ํ์ ๋ ๋น ๋ฅธ ๊ณต๊ฐ(=์บ์ฌ)์ ์์ฒญ๋ ๋ฐ์ดํฐ๋ฅผ ์ ์ฅํด ๋์๋ค๊ฐ ํ์ ์์ฒญ์ ์บ์ฌ๋ก๋ถํฐ ์ง์  ์๋น์คํ๋ ๋ฐฉ์
  - paging system ์ธ์๋ cache memory, buffer caching, Web caching ๋ฑ ๋ค์ํ ๋ถ์ผ์์ ์ฌ์ฉ
- ์บ์ฌ ์ด์์ ์๊ฐ ์ ์ฝ
  - ๊ต์ฒด ์๊ณ ๋ฆฌ์ฆ์์ ์ญ์ ํ  ํญ๋ชฉ์ ๊ฒฐ์ ํ๋ ์ผ์ ์ง๋์น๊ฒ ๋ง์ ์๊ฐ์ด ๊ฑธ๋ฆฌ๋ ๊ฒฝ์ฐ ์ค์  ์์คํ์์ ์ฌ์ฉํ  ์ ์์
  - Buffer caching ์ด๋ Web caching ์ ๊ฒฝ์ฐ
    - O(1) ์์ O(log n) ์ ๋๊น์ง ํ์ฉ
  - Paging system ์ธ ๊ฒฝ์ฐ
    - page fault ์ธ ๊ฒฝ์ฐ์๋ง OS๊ฐ ๊ด์ฌํจ
    - ํ์ด์ง๊ฐ ์ด๋ฏธ ๋ฉ๋ชจ๋ฆฌ์ ์กด์ฌํ๋ ๊ฒฝ์ฐ ์ฐธ์กฐ์๊ฐ ๋ฑ์ ์ ๋ณด๋ฅผ OS๊ฐ ์ ์ ์์
    - O(1)์ธ LRU์ list ์กฐ์์กฐ์ฐจ ๋ถ๊ฐ๋ฅ



### ๐ Clock Algorithm

- **Clock algorithm**
  - LRU์ ๊ทผ์ฌ(approximation) ์๊ณ ๋ฆฌ์ฆ
  - ์ฌ๋ฌ ๋ช์นญ์ผ๋ก ๋ถ๋ฆผ
    - Second chance algorithm
    - NUR (Not Used Recently) ๋๋ NRU(Not Recently Used)
  - Reference bit์ ์ฌ์ฉํด์ ๊ต์ฒด ๋์ ํ์ด์ง ์ ์  (circular list)
  - reference bit ๊ฐ 0์ธ ๊ฒ์ ์ฐพ์ ๋๊น์ง ํฌ์ธํฐ๋ฅผ ํ๋์ฉ ์์ผ๋ก ์ด๋
  - ํฌ์ธํฐ ์ด๋ํ๋ ์ค์ reference bit 1์ ๋ชจ๋ 0์ผ๋ก ๋ฐ๊ฟ
  - Reference bit ์ด 0 ์ธ ๊ฒ์ ์ฐพ์ผ๋ฉด ๊ทธ ํ์ด์ง๋ฅผ ๊ต์ฒด
  - ํ ๋ฐํด ๋๋์์์๋(=second chance) 0 ์ด๋ฉด ๊ทธ ๋์๋ replace ๋นํจ
  - ์์ฃผ ์ฌ์ฉ๋๋ ํ์ด์ง๋ผ๋ฉด second chance๊ฐ ์ฌ ๋ 1
- Clock algorithm ์ ๊ฐ์ 
  - reference bit ๊ณผ modified bit (dirty bit) ์ ํจ๊ป ์ฌ์ฉ
  - referenct bit = 1 : ์ต๊ทผ์ ์ฐธ์กฐ๋ ํ์ด์ง
  - modified bit = 1 : ์ต๊ทผ์ ๋ณ๊ฒฝ๋ ํ์ด์ง (I/O๋ฅผ ๋๋ฐํ๋ ํ์ด์ง)



### ๐ Page Frame์ Allocation

- Allocation problem : ๊ฐ process์ ์ผ๋ง๋งํผ์ page frame์ ํ ๋นํ  ๊ฒ์ธ๊ฐ ?
- Allocation์ ํ์์ฑ
  - ๋ฉ๋ชจ๋ฆฌ ์ฐธ์กฐ ๋ช๋ น์ด ์ํ์ ๋ช๋ น์ด, ๋ฐ์ดํฐ ๋ฑ ์ฌ๋ฌ ํ์ด์ง ๋์ ์ฐธ์กฐ
    - ๋ช๋ น์ด ์ํ์ ์ํด ์ต์ํ ํ ๋น๋์ด์ผ ํ๋ frame์ ์๊ฐ ์์
  - Loop๋ฅผ ๊ตฌ์ฑํ๋ page ๋ค์ ํ๊บผ๋ฒ์ allocate ๋๋ ๊ฒ์ด ์ ๋ฆฌํจ
    - ์ต์ํ์ allocation์ด ์์ผ๋ฉด ๋งค loop ๋ง๋ค page fault
- Allocation Scheme
  - **Eqaul allcation** : ๋ชจ๋  ํ๋ก์ธ์ค์ ๋๊ฐ์ ๊ฐฏ์ ํ ๋น
  - **Proportional allocation** : ํ๋ก์ธ์ค ํฌ๊ธฐ์ ๋น๋กํ์ฌ ํ ๋น
  - **Priority allocation** : ํ๋ก์ธ์ค์ priority์ ๋ฐ๋ผ ๋ค๋ฅด๊ฒ ํ ๋น



### ๐ Global vs. Local Replacement

- **Global replacement**
  - Replace  ์ ๋ค๋ฅธ process์ ํ ๋น๋ frame์ ๋นผ์์ ์ฌ ์ ์๋ค
  - Process ๋ณ ํ ๋น๋์ ์กฐ์ ํ๋ ๋ ๋ค๋ฅธ ๋ฐฉ๋ฒ์
  - FIFO, LRU, LFU ๋ฑ์ ์๊ณ ๋ฆฌ์ฆ์ global replacement๋ก ์ฌ์ฉ์์ ํด๋น
  - Working set, PFF ์๊ณ ๋ฆฌ์ฆ ์ฌ์ฉ
- **Local replacement**
  - ์์ ์๊ฒ ํ ๋น๋ frame ๋ด์์๋ง replacement
  - FIFO, LRU, LFU ๋ฑ์ ์๊ณ ๋ฆฌ์ฆ์ process ๋ณ๋ก ์ด์์



### ๐ Thrashing

- ํ๋ก์ธ์ค์ ์ํํ ์ํ์ ํ์ํ ์ต์ํ์ page frame ์๋ฅผ ํ ๋น ๋ฐ์ง ๋ชปํ ๊ฒฝ์ฐ ๋ฐ์
- Page fault rate์ด ๋งค์ฐ ๋์์ง
- CPU utilization ์ด ๋ฎ์์ง
- OS ๋ MPD (Multiprogramming degree) ๋ฅผ ๋์ฌ์ผ ํ๋ค๊ณ  ํ๋จ
- ๋ ๋ค๋ฅธ ํ๋ก์ธ์ค๊ฐ ์์คํ์ ์ถ๊ฐ๋จ (higher MPD)
- ํ๋ก์ธ์ค ๋น ํ ๋น๋ frame์ ์๊ฐ ๋์ฑ ๊ฐ์
- ํ๋ก์ธ์ค๋ page์ swap in / swap out ์ผ๋ก ๋งค์ฐ ๋ฐ์จ
- ๋๋ถ๋ถ์ ์๊ฐ์ CPU๋ ํ๊ฐํจ
- low throughput

![image-20220913191133360](assets/image-20220913191133360.png)



### ๐ Working-Set Model

- Locality of reference
  - ํ๋ก์ธ์ค๋ ํน์  ์๊ฐ ๋์ ์ผ์  ์ฅ์๋ง์ ์ง์ค์ ์ผ๋ก ์ฐธ์กฐํ๋ค
  - ์ง์ค์ ์ผ๋ก ์ฐธ์กฐ๋๋ ํด๋น page๋ค์ ์งํฉ์ locality set์ด๋ผ ํจ
- Working-set Model
  - Locality ์ ๊ธฐ๋ฐํ์ฌ ํ๋ก์ธ์ค๊ฐ ์ผ์  ์๊ฐ ๋์ ์ํํ๊ฒ ์ํ๋๊ธฐ ์ํด ํ๊บผ๋ฒ์ ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ผ์ ์์ด์ผ ํ๋ page๋ค์ ์งํฉ์ Working Set์ด๋ผ ์ ์ํจ
  - Working Set ๋ชจ๋ธ์์๋ process ์ working set ์ ์ฒด๊ฐ ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ผ์ ์์ด์ผ ์ํ๋๊ณ  ๊ทธ๋ ์ง ์์ ๊ฒฝ์ฐ ๋ชจ๋  frame์ ๋ฐ๋ฉํ ํ swap out (suspend)
  - Thrashing์ ๋ฐฉ์งํจ
  - Multiprogramming degree๋ฅผ ๊ฒฐ์ ํจ



### ๐ Working-Set Algorithm

- Working Set์ ๊ฒฐ์ 
  - Working set window๋ฅผ ํตํด ์์๋
  - window size๊ฐ โ์ธ ๊ฒฝ์ฐ
    - ์๊ฐ ti ์์์ working set WS (ti)
      - Time interval [ti-โ, ti] ์ฌ์ด์ ์ฐธ์กฐ๋ ์๋ก ๋ค๋ฅธ ํ์ด์ง๋ค์ ์งํฉ
    - Working set์ ์ํ page๋ ๋ฉ๋ชจ๋ฆฌ์ ์ ์ง, ์ํ์ง ์์ ๊ฒ์ ๋ฒ๋ฆผ
      (์ฆ, ์ฐธ์กฐ๋ ํ โ ์๊ฐ ๋์ ํด๋น page๋ฅผ ๋ฉ๋ชจ๋ฆฌ์ ์ ์งํ ํ ๋ฒ๋ฆผ)

![image-20220913194736624](assets/image-20220913194736624.png)

- Working-Set Algorithm

  - Process ๋ค์ working set size์ ํฉ์ด page frame ์ ์๋ณด๋ค ํฐ ๊ฒฝ์ฐ
    - ์ผ๋ถ process ๋ฅผ swap out ์์ผ ๋จ์ process ์ working set์ ์ฐ์ ์ ์ผ๋ก ์ถฉ์กฑ์์ผ ์ค๋ค (MPD๋ฅผ ์ค์)

  - Working set์ ๋ค ํ ๋นํ๊ณ ๋ page frame์ด ๋จ๋ ๊ฒฝ์ฐ
    - Swap out ๋์๋ ํ๋ก์ธ์ค์๊ฒ working set์ ํ ๋น (MPD๋ฅผ ํค์)

- Window size โ

  - Working set์ ์ ๋๋ก ํ์งํ๊ธฐ ์ํด์๋ window size๋ฅผ ์ ๊ฒฐ์ ํด์ผ ํจ
  - โ ๊ฐ์ด ๋๋ฌด ์์ผ๋ฉด locality set์ ๋ชจ๋ ์์ฉํ์ง ๋ชปํ  ์ฐ๋ ค
  - โ ๊ฐ์ด ํฌ๋ฉด ์ฌ๋ฌ ๊ท๋ชจ์ locality set ์์ฉ
  - โ ๊ฐ์ด infinite ์ด๋ฉด ์ ์ฒด ํ๋ก๊ทธ๋จ์ ๊ตฌ์ฑํ๋ page๋ฅผ working set ์ผ๋ก ๊ฐ์ฃผ



### ๐ PFF (Page-Fault Frequency) Scheme

![image-20220913200311256](assets/image-20220913200311256.png)

- page-fault rate์ ์ํ๊ฐ๊ณผ ํํ๊ฐ์ ๋๋ค
  - Page fault rate ์ด ์ํ๊ฐ์ ๋์ผ๋ฉด frame์ ๋ ํ ๋นํ๋ค
  - Page fault rate์ด ํํ๊ฐ ์ดํ์ด๋ฉด ํ ๋น frame ์๋ฅผ ์ค์ธ๋ค
- ๋น frame์ด ์์ผ๋ฉด ์ผ๋ถ ํ๋ก์ธ์ค๋ฅผ swap out



### ๐Page Size ์ ๊ฒฐ์ 

- Page size๋ฅผ ๊ฐ์์ํค๋ฉด
  - ํ์ด์ง ์ ์ฆ๊ฐ
  - ํ์ด์ง ํ์ด๋ธ ํฌ๊ธฐ ์ฆ๊ฐ
  - Internal fragmentation ๊ฐ์
  - Disk transfer ์ ํจ์จ์ฑ ๊ฐ์
    - Seek/rotation vs. transfer
  - ํ์ํ ์ ๋ณด๋ง ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ผ์ ๋ฉ๋ชจ๋ฆฌ ์ด์ฉ์ด ํจ์จ์ 
    - Locality์ ํ์ฉ ์ธก๋ฉด์์๋ ์ข์ง ์์
- Trend
  - Larger page size