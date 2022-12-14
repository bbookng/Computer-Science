# ๐ซ Memory Management

## โจ Logical vs. Physical Address

#### ๐ Logical address (=virtual address)

- ํ๋ก์ธ์ค๋ง๋ค ๋๋ฆฝ์ ์ผ๋ก ๊ฐ์ง๋ ์ฃผ์ ๊ณต๊ฐ
- ๊ฐ ํ๋ก์ธ์ค๋ง๋ค 0๋ฒ์ง๋ถํฐ ์์
- CPU๊ฐ ๋ณด๋ ์ฃผ์๋ logical address์

#### ๐ Physical address

- ๋ฉ๋ชจ๋ฆฌ์ ์ค์  ์ฌ๋ผ๊ฐ๋ ์์น



- **์ฃผ์ ๋ฐ์ธ๋ฉ** : ์ฃผ์๋ฅผ ๊ฒฐ์ ํ๋ ๊ฒ

โ       Symbolic Address โ Logical Address โ Physical address



## โจ ์ฃผ์ ๋ฐ์ธ๋ฉ (Address Binding)

#### ๐ Compile time binding

- ๋ฌผ๋ฆฌ์  ๋ฉ๋ชจ๋ฆฌ ์ฃผ์(physical address)๊ฐ ์ปดํ์ผ ์ ์๋ ค์ง

- ์์ ์์น ๋ณ๊ฒฝ์ ์ฌ์ปดํ์ผ

- ์ปดํ์ผ๋ฌ๋ ์ ๋ ์ฝ๋ (absolute code) ์์ฑ

  

#### ๐ Load time binding

- Loader์ ์ฑ์ํ์ ๋ฌผ๋ฆฌ์  ๋ฉ๋ชจ๋ฆฌ ์ฃผ์ ๋ถ์ฌ

- ์ปดํ์ผ๋ฌ๊ฐ ์ฌ๋ฐฐ์น๊ฐ๋ฅ์ฝ๋(relocatable code)๋ฅผ ์์ฑํ ๊ฒฝ์ฐ ๊ฐ๋ฅ

  

#### ๐ Execution time binding (= Run time binding)

- ์ํ์ด ์์๋ ์ดํ์๋ ํ๋ก์ธ์ค์ ๋ฉ๋ชจ๋ฆฌ ์ ์์น๋ฅผ ์ฎ๊ธธ ์ ์์
- CPU๊ฐ ์ฃผ์๋ฅผ ์ฐธ์กฐํ  ๋๋ง๋ค binding์ ์ ๊ฒ (address mapping table)
- ํ๋์จ์ด์ ์ธ ์ง์์ด ํ์
  (ex. base and limit registers, MMU).

![image-20220904185259481](assets/image-20220904185259481.png)



#### ๐ Memeory-Management Unit (MMU)

- **MMU (Memory-Management Unit)**
  - logical address ๋ฅผ physical address๋ก ๋งคํํด ์ฃผ๋ Hardware device
-  MMU scheme
  - ์ฌ์ฉ์ ํ๋ก์ธ์ค๊ฐ CPU์์ ์ํ๋๋ฉฐ ์์ฑํด๋ด๋ ๋ชจ๋  ์ฃผ์๊ฐ์ ๋ํด base register (=relocation register)์ ๊ฐ์ ๋ํ๋ค
- user program
  - logical addresss ๋ง์ ๋ค๋ฃฌ๋ค
  - ์ค์  physical address๋ฅผ ๋ณผ ์ ์์ผ๋ฉฐ ์ ํ์๊ฐ ์๋ค



- Dynamic Relocation

![image-20220904191905626](assets/image-20220904191905626.png)



#### ๐Hardware Support for Address Translation

![image-20220904192435101](assets/image-20220904192435101.png)

 - ์ด์์ฒด์  ๋ฐ ์ฌ์ฉ์ ํ๋ก์ธ์ค ๊ฐ์ ๋ฉ๋ชจ๋ฆฌ ๋ณดํธ๋ฅผ ์ํด ์ฌ์ฉํ๋ ๋ ์ง์คํฐ
   - **Relocation register** : ์ ๊ทผํ  ์ ์๋ ๋ฌผ๋ฆฌ์  ๋ฉ๋ชจ๋ฆฌ ์ฃผ์์ ์ต์๊ฐ (=base register)
   - **Limit register** : ๋ผ๋ฆฌ์  ์ฃผ์์ ๋ฒ์



## โจDynamic Loading

- ํ๋ก์ธ์ค ์ ์ฒด๋ฅผ ๋ฉ๋ชจ๋ฆฌ์ ๋ฏธ๋ฆฌ ๋ค ์ฌ๋ฆฌ๋ ๊ฒ์ด ์๋๋ผ ํด๋น ๋ฃจํด์ด ๋ถ๋ ค์ง ๋ ๋ฉ๋ชจ๋ฆฌ์ load ํ๋ ๊ฒ
- memory utilization์ ํฅ์
- ๊ฐ๋์ ์ฌ์ฉ๋๋ ๋ง์ ์์ ์ฝ๋์ ๊ฒฝ์ฐ ์ ์ฉ
  ์ ) ์ค๋ฅ ์ฒ๋ฆฌ ๋ฃจํด
- ์ด์์ฒด์ ์ ํน๋ณํ ์ง์ ์์ด ํ๋ก๊ทธ๋จ ์์ฒด์์ ๊ตฌํ ๊ฐ๋ฅ (OS๋ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ํตํด ์ง์ ๊ฐ๋ฅ)
  โ **Overlay ์์ ์ฐจ์ด์ **
- Loading : ๋ฉ๋ชจ๋ฆฌ๋ก ์ฌ๋ฆฌ๋ ๊ฒ



## โจ Overlay

- ๋ฉ๋ชจ๋ฆฌ์ ํ๋ก์ธ์ค์ ๋ถ๋ถ ์ค ์ค์  ํ์ํ ์ ๋ณด๋ง์ ์ฌ๋ฆผ
- ํ๋ก์ธ์ค์ ํฌ๊ธฐ๊ฐ ๋ฉ๋ชจ๋ฆฌ๋ณด๋ค ํด ๋ ์ ์ฉ
- ์ด์์ฒด์ ์ ์ง์์์ด ์ฌ์ฉ์์ ์ํด ๊ตฌํ โ **Dynamic Loading๊ณผ์ ์ฐจ์ด์ **
- ์์ ๊ณต๊ฐ์ ๋ฉ๋ชจ๋ฆฌ๋ฅผ ์ฌ์ฉํ๋ ์ด์ฐฝ๊ธฐ ์์คํ์์ ์์์์ผ๋ก ํ๋ก๊ทธ๋๋จธ๊ฐ ๊ตฌํ
  - "Manual Overlay"
  - ํ๋ก๊ทธ๋๋ฐ์ด ๋งค์ฐ ๋ณต์ก



## โจ Swapping

- **Swapping**

  - ํ๋ก์ธ์ค๋ฅผ ์ผ์์ ์ผ๋ก ๋ฉ๋ชจ๋ฆฌ์์ backing store๋ก ์ซ์๋ด๋ ๊ฒ

- **Backing store (=swap area)**

  - ํ๋๋์คํฌ์์ ์ซ๊ฒจ๋์ ๊ฐ๋ ๊ณต๊ฐ

  - ๋์คํฌ
    - ๋ง์ ์ฌ์ฉ์์ ํ๋ก์ธ์ค ์ด๋ฏธ์ง๋ฅผ ๋ด์ ๋งํผ ์ถฉ๋ถํ ๋น ๋ฅด๊ณ  ํฐ ์ ์ฅ ๊ณต๊ฐ

- **Swap in / Swap out**

  - ์ผ๋ฐ์ ์ผ๋ก ์ค๊ธฐ ์ค์ผ์ค๋ฌ (swapper)์ ์ํด **swap out** ์ํฌ ํ๋ก์ธ์ค ์ ์ 
  - pirority-based CPU scheduling algorith
    - priority ๊ฐ ๋ฎ์ ํ๋ก์ธ์ค๋ฅผ swapped out ์ํด
    - priority๊ฐ ๋์ ํ๋ก์ธ์ค๋ฅผ ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ ค ๋์
  - Compile time ํน์ load time binding ์์๋ ์๋ ๋ฉ๋ชจ๋ฆฌ ์์น๋ก **swap in** ํด์ผ ํจ
  - Execution time binding์์๋ ์ถํ ๋น ๋ฉ๋ชจ๋ฆฌ ์์ญ ์๋ฌด ๊ณณ์๋ ์ฌ๋ฆด ์ ์์
  - swap time์ ๋๋ถ๋ถ transfer time (swap๋๋ ์์ ๋น๋กํ๋ ์๊ฐ)์

![image-20220904211717058](assets/image-20220904211717058.png)



## โจ Dynamic Linking

- Linking์ ์คํ ์๊ฐ (execution time) ๊น์ง ๋ฏธ๋ฃจ๋ ๊ธฐ๋ฒ
- **Static linking**
  - ๋ผ์ด๋ธ๋ฌ๋ฆฌ๊ฐ ํ๋ก๊ทธ๋จ์ ์คํ ํ์ผ ์ฝ๋์ ํฌํจ๋จ
  - ์คํ ํ์ผ์ ํฌ๊ธฐ๊ฐ ์ปค์ง
  - ๋์ผํ ๋ผ์ด๋ธ๋ฌ๋ฆฌ๋ฅผ ๊ฐ๊ฐ์ ํ๋ก์ธ์ค๊ฐ ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ฆฌ๋ฏ๋ก ๋ฉ๋ชจ๋ฆฌ ๋ญ๋น
    (ex. printf ํจ์์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ ์ฝ๋)
- **Dynamic linking**
  - ๋ผ์ด๋ธ๋ฌ๋ฆฌ๊ฐ ์คํ์ ์ฐ๊ฒฐ(link) ๋จ
  - ๋ผ์ด๋ธ๋ฌ๋ฆฌ ํธ์ถ ๋ถ๋ถ์ ๋ผ์ด๋ธ๋ฌ๋ฆฌ ๋ฃจํด์ ์์น๋ฅผ ์ฐพ๊ธฐ ์ํ stub์ด๋ผ๋ ์์ ์ฝ๋๋ฅผ ๋ 
  - ๋ผ์ด๋ธ๋ฌ๋ฆฌ๊ฐ ์ด๋ฏธ ๋ฉ๋ชจ๋ฆฌ์ ์์ผ๋ฉด ๊ทธ ๋ฃจํด์ ์ฃผ์๋ก ๊ฐ๊ณ  ์์ผ๋ฉด ๋์คํฌ์์ ์ฝ์ด์ด
  - ์ด์์ฒด์ ์ ๋์์ด ํ์



## โจ Allocation of Physical Memory

- ๋ฉ๋ชจ๋ฆฌ๋ ์ผ๋ฐ์ ์ผ๋ก ๋ ์์ญ์ผ๋ก ๋๋์ด ์ฌ์ฉ

  - **OS ์์ฃผ ์์ญ**
    - interrupt vector์ ํจ๊ป ๋ฎ์ ์ฃผ์ ์์ญ ์ฌ์ฉ
  - **์ฌ์ฉ์ ํ๋ก์ธ์ค ์์ญ**
    - ๋์ ์ฃผ์ ์์ญ ์ฌ์ฉ

- ์ฌ์ฉ์ ํ๋ก์ธ์ค ์์ญ์ ํ ๋น ๋ฐฉ๋ฒ

  - **Contiguous allocation**
    : ๊ฐ๊ฐ์ ํ๋ก์ธ์ค๊ฐ ๋ฉ๋ชจ๋ฆฌ์ ์ฐ์์ ์ธ ๊ณต๊ฐ์ ์ ์ฌ๋๋๋ก ํ๋ ๊ฒ

    - Fixed partition allocation
    - Variable partition allocation

  - **Noncontiguous allocation**
    : ํ๋์ ํ๋ก์ธ์ค๊ฐ ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ฌ ์์ญ์ ๋ถ์ฐ๋์ด ์ฌ๋ผ๊ฐ ์ ์์

    - Paging
    - Segmentation
    - Paged Segmentation

    

### ๐ Contiguous Allocation

#### - ๊ณ ์ ๋ถํ (Fixed partition) ๋ฐฉ์

- ๋ฌผ๋ฆฌ์  ๋ฉ๋ชจ๋ฆฌ๋ฅผ ๋ช ๊ฐ์ ์๊ตฌ์  ๋ถํ (partition)๋ก ๋๋
- ๋ถํ ์ ํฌ๊ธฐ๊ฐ ๋ชจ๋ ๋์ผํ ๋ฐฉ์๊ณผ ์๋ก ๋ค๋ฅธ ๋ฐฉ์์ด ์กด์ฌ
- ๋ถํ ๋น ํ๋์ ํ๋ก๊ทธ๋จ ์ ์ฌ
- ์ตํต์ฑ์ด ์์
  - ๋์์ ๋ฉ๋ชจ๋ฆฌ์ load ๋๋ ํ๋ก๊ทธ๋จ์ ์๊ฐ ๊ณ ์ ๋จ
  - ์ต๋ ์ํ ๊ฐ๋ฅ ํ๋ก๊ทธ๋จ ์ ์ฌ
- Internal fragmentation ๋ฐ์ (external fragmentation๋ ๋ฐ์)

#### - ๊ฐ๋ณ๋ถํ (Variable partition) ๋ฐฉ์

- ํ๋ก๊ทธ๋จ์ ํฌ๊ธฐ๋ฅผ ๊ณ ๋ คํด์ ํ ๋น
- ๋ถํ ์ ํฌ๊ธฐ, ๊ฐ์๊ฐ ๋์ ์ผ๋ก ๋ณํจ
- ๊ธฐ์ ์  ๊ด๋ฆฌ ๊ธฐ๋ฒ ํ์
- External fragmentation ๋ฐ์

![image-20220904230347895](assets/image-20220904230347895.png)

#### - Hole

- ๊ฐ์ฉ ๋ฉ๋ชจ๋ฆฌ ๊ณต๊ฐ
- ๋ค์ํ ํฌ๊ธฐ์ hole ๋ค์ด ๋ฉ๋ชจ๋ฆฌ ์ฌ๋ฌ ๊ณณ์ ํฉ์ด์ ธ ์์
- ํ๋ก์ธ์ค๊ฐ ๋์ฐฉํ๋ฉด ์์ฉ๊ฐ๋ฅํ hole์ ํ ๋น
- ์ด์์ฒด์ ๋ ๋ค์์ ์ ๋ณด๋ฅผ ์ ์ง

โ	a) ํ ๋น ๊ณต๊ฐ 	b) ๊ฐ์ฉ ๊ณต๊ฐ (hole)

![image-20220904230552722](assets/image-20220904230552722.png)



#### ๐ก Dynamic Storage-Allocation Problem

โ	: ๊ฐ๋ณ ๋ถํ  ๋ฐฉ์์์ size n์ธ ์์ฒญ์ ๋ง์กฑํ๋ ๊ฐ์ฅ ์ ์ ํ hole์ ์ฐพ๋ ๋ฌธ์ 

- **First-fit**
  - Size๊ฐ n ์ด์์ธ ๊ฒ ์ค ์ต์ด๋ก ์ฐพ์์ง๋ hole์ ํ ๋น
- **Best-fit**
  - Size๊ฐ n ์ด์์ธ ๊ฐ์ฅ ์์ hole์ ์ฐพ์์ ํ ๋น
  - Hole๋ค์ ๋ฆฌ์คํธ๊ฐ ํฌ๊ธฐ์์ผ๋ก ์ ๋ ฌ๋์ง ์์ ๊ฒฝ์ฐ ๋ชจ๋  hole์ ๋ฆฌ์คํธ๋ฅผ ํ์ํด์ผํจ
  - ๋ง์ ์์ ์์ฃผ ์์ hole๋ค์ด ์์ฑ๋จ
- **Worst-fit**
  - ๊ฐ์ฅ ํฐ hole์ ํ ๋น
  - ์ญ์ ๋ชจ๋  ๋ฆฌ์คํธ๋ฅผ ํ์ํด์ผ ํจ
  - ์๋์ ์ผ๋ก ์์ฃผ ํฐ hole ๋ค์ด ์์ฑ๋จ
- First-fit๊ณผ best-fit์ด worst-fit ๋ณด๋ค ์๋์ ๊ณต๊ฐ ์ด์ฉ๋ฅ  ์ธก๋ฉด์์ ํจ๊ณผ์ ์ธ ๊ฒ์ผ๋ก ์๋ ค์ง (์คํ์  ๊ฒฐ๊ณผ)



#### ๐ก compaction

- extenal fragmentation ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๋ ํ ๊ฐ์ง ๋ฐฉ๋ฒ
- ์ฌ์ฉ ์ค์ธ ๋ฉ๋ชจ๋ฆฌ ์์ญ์ ํ๊ตฐ๋ฐ๋ก ๋ชฐ๊ณ  hole ๋ค์ ๋ค๋ฅธ ํ ๊ณณ์ผ๋ก ๋ชฐ์ ํฐ block์ ๋ง๋๋ ๊ฒ
- ๋งค์ฐ ๋น์ฉ์ด ๋ง์ด ๋๋ ๋ฐฉ๋ฒ์
- ์ต์ํ์ ๋ฉ๋ชจ๋ฆฌ ์ด๋์ผ๋ก compaction ํ๋ ๋ฐฉ๋ฒ (๋งค์ฐ ๋ณต์กํ ๋ฌธ์ )
- Compaction์ ํ๋ก์ธ์ค์ ์ฃผ์๊ฐ ์คํ ์๊ฐ์ ๋์ ์ผ๋ก ์ฌ๋ฐฐ์น ๊ฐ๋ฅํ ๊ฒฝ์ฐ์๋ง ์ํ๋  ์ ์๋ค
- Run-time-binding ๊ฒฝ์ฐ์๋ง ๊ฐ๋ฅ



### ๐ Noncontiguous Allocation

#### ๐ก Paging

- **Paging**
  - Process์ virtual memory ๋ฅผ ๋์ผํ ์ฌ์ด์ฆ์ page ๋จ์๋ก ๋๋
  - Virtual memory์ ๋ด์ฉ์ด page ๋จ์๋ก noncontiguousํ๊ฒ ์ ์ฅ๋จ
  - ์ผ๋ถ๋ backing storage์, ์ผ๋ถ๋ physical memory์ ์ ์ฅ
- **Basic Method**
  - physical memeory๋ฅผ ๋์ผํ ํฌ๊ธฐ์ frame์ผ๋ก ๋๋
  - logical memory๋ฅผ ๋์ผ ํฌ๊ธฐ์ page๋ก ๋๋ (frame๊ณผ ๊ฐ์ ํฌ๊ธฐ)
  - ๋ชจ๋  ๊ฐ์ฉ frame ๋ค์ ๊ด๋ฆฌ
  - page table์ ์ฌ์ฉํ์ฌ logical address๋ฅผ physical address๋ก ๋ณํ
  - External fragmentation ๋ฐ์ ์ํจ
  - Internal fragmentation ๋ฐ์ ๊ฐ๋ฅ
  
  

![image-20220905231851024](assets/image-20220905231851024.png)

![image-20220905233744808](assets/image-20220905233744808.png)



#### ๐ก Implementation of Page Table

- Page table์ main memory์ ์์ฃผ
- **Page-table base register (PTBR)**๊ฐ page table ์ ๊ฐ๋ฆฌํด
- **Page-table length register (PTLR)**๊ฐ ํ์ด๋ธ ํฌ๊ธฐ๋ฅผ ๋ณด๊ด
- ๋ชจ๋  ๋ฉ๋ชจ๋ฆฌ ์ ๊ทผ ์ฐ์ฐ์๋ **2๋ฒ์ memory access** ํ์
- **page table** ์ ๊ทผ 1๋ฒ, ์ค์  **data/instruction** ์ ๊ทผ 1๋ฒ
- ์๋ ํฅ์์ ์ํด
  **associative register** ํน์ **translation look-aside buffer (TLB)** ๋ผ ๋ถ๋ฆฌ๋ ๊ณ ์์ lookup hardware cache ์ฌ์ฉ



#### ๐ก Paging Hardware with TLB

![image-20220906000016279](assets/image-20220906000016279.png)

- ์ฃผ์ ๋ณํ์ ์ํ ์บ์ ๋ฉ๋ชจ๋ฆฌ



#### ๐ก Associative Register

- **Associative registers** (TLB): parallel search๊ฐ ๊ฐ๋ฅ
  - TLB์๋ page table ์ค ์ผ๋ถ๋ง ์กด์ฌ
- Address translation
  - page table ์ค ์ผ๋ถ๊ฐ associative register ์ ๋ณด๊ด๋์ด ์์
  - ๋ง์ฝ ํด๋น page # ๊ฐ associative register์ ์๋ ๊ฒฝ์ฐ ๊ณง๋ฐ๋ก frame #์ ์ป์
  - ๊ทธ๋ ์ง ์์ ๊ฒฝ์ฐ main memory์ ์๋ page table ๋ก๋ถํฐ frame #์ ์ป์
  - TLB๋ context switch ๋ flush (remove old entries)



#### ๐ก Effective Access Time

- Associative register lookup time = ษ
- memory cycle time = 1
- **Hit ratio** = ษ
  - associative register ์์ ์ฐพ์์ง๋ ๋น์จ
- Effictive Access Time (EAT)

![image-20220906000926243](assets/image-20220906000926243.png)



#### ๐ก Two-Level Page Table

![image-20220906001149985](assets/image-20220906001149985.png)

- ํ๋์ ์ปดํจํฐ๋ address space๊ฐ ๋งค์ฐ ํฐ ํ๋ก๊ทธ๋จ ์ง์

  - 32 bit address ์ฌ์ฉ์ : 2ยณยฒ (4G) ์ ์ฃผ์ ๊ณต๊ฐ
    - page size๊ฐ 4K์ 1M๊ฐ์ page table entry ํ์
    - ๊ฐ page entry๊ฐ 4B์ ํ๋ก์ธ์ค๋น 4M์ page table ํ์
    - ๊ทธ๋ฌ๋, ๋๋ถ๋ถ์ ํ๋ก๊ทธ๋จ์ 4G์ ์ฃผ์ ๊ณต๊ฐ ์ค ์ง๊ทนํ ์ผ๋ถ๋ถ๋ง ์ฌ์ฉํ๋ฏ๋ก page table ๊ณต๊ฐ์ด ์ฌํ๊ฒ ๋ญ๋น๋จ

  โ  page table ์์ฒด๋ฅผ page๋ก ๊ตฌ์ฑ

  โ ์ฌ์ฉ๋์ง ์๋ ์ฃผ์ ๊ณต๊ฐ์ ๋ํ outer page table์ ์ํธ๋ฆฌ ๊ฐ์ NULL
       (๋์ํ๋ inner page table์ด ์์)



#### ๐ก Two-Level Paging Example

- logical address (on 32-bit machine with 4K page size)์ ๊ตฌ์ฑ
  - 20 bit ์ page number
  - 12 bit ์ page offset
- page table ์์ฒด๊ฐ page๋ก ๊ตฌ์ฑ๋๊ธฐ ๋๋ฌธ์ page number๋ ๋ค์๊ณผ ๊ฐ์ด ๋๋๋ค (๊ฐ page table entry๊ฐ 4B)
  - 10-bit์ page number.
  - 10-bit์ page offset.
- ๋ฐ๋ผ์, logical address๋ ๋ค์๊ณผ ๊ฐ๋ค
  ![image-20220906003238173](assets/image-20220906003238173.png)
- Pโ์ outer page table์ index ์ด๊ณ 
- Pโ๋ outer page table์ page์์์ ๋ณ์(displacement)



#### ๐ก Address-Translation Scheme

- 2๋จ๊ณ ํ์ด์ง์์์ Address-translation scheme

![image-20220906003056890](assets/image-20220906003056890.png)



#### ๐ก Multilevel Paging and Performance

- Address space๊ฐ ๋ ์ปค์ง๋ฉด ๋ค๋จ๊ณ ํ์ด์ง ํ์ด๋ธ ํ์

- ๊ฐ ๋จ๊ณ์ ํ์ด์ง ํ์ด๋ธ์ด ๋ฉ๋ชจ๋ฆฌ์ ์กด์ฌํ๋ฏ๋ก logical address์ physical address ๋ณํ์ ๋ ๋ง์ ๋ฉ๋ชจ๋ฆฌ ์ ๊ทผ ํ์

- ์บ์ฌ ๋ฉ๋ชจ๋ฆฌ๋ฅผ ํตํด ๋ฉ๋ชจ๋ฆฌ ์ ๊ทผ ์๊ฐ์ ์ค์ผ ์ ์์

- 4๋จ๊ณ ํ์ด์ง ํ์ด๋ธ์ ์ฌ์ฉํ๋ ๊ฒฝ์ฐ

  - ๋ฉ๋ชจ๋ฆฌ ์ ๊ทผ ์๊ฐ์ด 100ns, ์บ์ฌ ๋ฉ๋ชจ๋ฆฌ ์ ๊ทผ ์๊ฐ์ด 20ns์ด๊ณ  
  - ์บ์ฌ ์ ์ค๋ฅ ์ด 98%์ธ ๊ฒฝ์ฐ
    effective memory access time = 0.98 x 120 + 0.02 x 520 = 128 nanoseconds.

  ๊ฒฐ๊ณผ์ ์ผ๋ก ๋ฉ๋ชจ๋ฆฌ ์ ๊ทผ ์๊ฐ์ 28%๋ง down ์ํด



#### ๐ก Valid (v) / Invalid (i) Bit in a Page Table

![image-20220906162608105](assets/image-20220906162608105.png)



#### ๐ก Memory Protection

- Page table์ ๊ฐ entry ๋ง๋ค ์๋์ bit๋ฅผ ๋๋ค

  - **Protection bit**

    - page์ ๋ํ ์ ๊ทผ ๊ถํ (read/write/read-only)
    - ์ด๋ค ์ฐ์ฐ์ ๊ดํ ์ ๊ทผ ๊ถํ

  - **Valid-invalid bit**

    - "valid"๋ ํด๋น ์ฃผ์์ frame์ ๊ทธ ํ๋ก์ธ์ค๋ฅผ ๊ตฌ์ฑํ๋ ์ ํจํ ๋ด์ฉ์ด ์์์ ๋ปํจ (์ ๊ทผ ํ์ฉ)
    - "invalid"๋ ํด๋น ์ฃผ์์ frame์ ์ ํจํ ๋ด์ฉ์ด ์์*์ ๋ปํจ (์ ๊ทผ ๋ถํ)

    

    *****

    '*' i) ํ๋ก์ธ์ค๊ฐ ๊ทธ ์ฃผ์ ๋ถ๋ถ์ ์ฌ์ฉํ์ง ์๋ ๊ฒฝ์ฐ
         ii) ํด๋น ํ์ด์ง๊ฐ ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ผ์ ์์ง ์๊ณ  swap area์ ์๋ ๊ฒฝ์ฐ



#### ๐ก Inverted Page Table

- page table์ด ๋งค์ฐ ํฐ ์ด์ 
  - ๋ชจ๋  process ๋ณ๋ก ๊ทธ logical address์ ๋์ํ๋ ๋ชจ๋  page์ ๋ํด page table entry๊ฐ ์กด์ฌ
  - ๋์ํ๋ page๊ฐ ๋ฉ๋ชจ๋ฆฌ์ ์๋  ์๋๋  ๊ฐ์ page table์๋ entry๋ก ์กด์ฌ
- Inverted page table
  - Page frame ํ๋๋น page table์ ํ๋์ entry๋ฅผ ๋ ๊ฒ (system-wide)
  - ๊ฐ page table entry๋  ๊ฐ๊ฐ์ ๋ฌผ๋ฆฌ์  ๋ฉ๋ชจ๋ฆฌ์ page frame์ด ๋ด๊ณ  ์๋ ๋ด์ฉ ํ์
    (process-id, process์ logical address)
  - ๋จ์ 
    - ํ์ด๋ธ ์ ์ฒด๋ฅผ ํ์ํด์ผ ํจ
  - ์กฐ์น
    - associative register ์ฌ์ฉ (expensive)

![image-20220906171932696](assets/image-20220906171932696.png)



#### ๐ก Shared Page

- **Shared code**
  - **Re-entrant Code (=Pure code)**
  - read-only ๋ก ํ์ฌ ํ๋ก์ธ์ค ๊ฐ์ ํ๋์ code๋ง ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ฆผ
    (ex. text editors, compilers, window systems).
  - Shared code๋ ๋ชจ๋  ํ๋ก์ธ์ค์ logical address space์์ ๋์ผํ ์์น์ ์์ด์ผ ํจ
- **Private code and data**
  - ๊ฐ ํ๋ก์ธ์ค๋ค์ ๋์์ ์ผ๋ก ๋ฉ๋ชจ๋ฆฌ์ ์ฌ๋ฆผ
  - Private data๋ logical address space์ ์๋ฌด ๊ณณ์ ์๋ ๋ฌด๋ฐฉ

![image-20220906174401917](assets/image-20220906174401917.png)



#### ๐ก Segmentation

- ํ๋ก๊ทธ๋จ์ ์๋ฏธ ๋จ์์ธ ์ฌ๋ฌ ๊ฐ์ segment๋ก ๊ตฌ์ฑ

  - ์๊ฒ๋ ํ๋ก๊ทธ๋จ์ ๊ตฌ์ฑํ๋ ํจ์ ํ๋ํ๋๋ฅผ ์ธ๊ทธ๋จผํธ๋ก ์ ์
  - ํฌ๊ฒ๋ ํ๋ก๊ทธ๋จ ์ ์ฒด๋ฅผ ํ๋์ ์ธ๊ทธ๋จผํธ๋ก ์ ์ ๊ฐ๋ฅ
  - ์ผ๋ฐ์ ์ผ๋ก๋ code, data, stack ๋ถ๋ถ์ด ํ๋์ฉ์ ์ธ๊ทธ๋จผํธ๋ก ์ ์๋จ

- Segment๋ ๋ค์๊ณผ ๊ฐ์ *logical unit* ๋ค์

  ```
  main(),
  function,
  global variables,
  stack,
  symbol table, arrays
  ```



#### ๐ก Segmentation Architecture

- Logical address ๋ ๋ค์์ ๋ ๊ฐ์ง๋ก ๊ตฌ์ฑ `< segment-number, offset >`
- **Segment table**
  - each table entry has:
    - base - starting physical address of the segment
    - limit - length of the segment (segment์ ๊ธธ์ด)
- **Segment-table base register (STBR)**
  - ๋ฌผ๋ฆฌ์  ๋ฉ๋ชจ๋ฆฌ์์์ segment table์ ์์น

- **Segment-table lenth register (STLR)**
  - ํ๋ก๊ทธ๋จ์ด ์ฌ์ฉํ๋ segment์ ์
    `segment number s is legal if s < STLR`



#### ๐ก Segmentation Hardware

![image-20220906175316520](assets/image-20220906175316520.png)



#### ๐กSegmentation Architecture (Cont.)

- **Protection**

  - ๊ฐ ์ธ๊ทธ๋จผํธ ๋ณ๋ก protection bit๊ฐ ์์
  - Each entry:
    - Valid bit = 0 โ illegal segment
    - Read/Write/Execution ๊ถํ bit

- **Sharing**

  - shared segment
  - same segment number

  *** segment๋ ์๋ฏธ ๋จ์์ด๊ธฐ ๋๋ฌธ์ ๊ณต์ (sharing)์ ๋ณด์(protection)์ ์์ด paging๋ณด๋ค ํจ์ฌ ํจ๊ณผ์ ์ด๋ค.

- **Allocation**

  - first fit / best fit
  - external fragmentation ๋ฐ์

  *** segment ์ ๊ธธ์ด๊ฐ ๋์ผํ์ง ์์ผ๋ฏ๋ก ๊ฐ๋ณ๋ถํ  ๋ฐฉ์์์์ ๋์ผํ ๋ฌธ์ ์ ๋ค์ด ๋ฐ์

#### ๐ก Sharing of Segment

![image-20220906194313180](assets/image-20220906194313180.png)



#### ๐ก Segmentation with Paging

- pure segmentation ๊ณผ์ ์ฐจ์ด์ 
  - segment-table entry๊ฐ segment์ base address ๋ฅผ ๊ฐ์ง๊ณ  ์๋ ๊ฒ์ด ์๋๋ผ segment๋ฅผ ๊ตฌ์ฑํ๋ page table์ base address ๋ฅผ ๊ฐ์ง๊ณ  ์์

![image-20220906195403414](assets/image-20220906195403414.png)







