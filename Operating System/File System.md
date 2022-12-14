# π« File System, File System Implementation

## β¨ File and File System

- **File**
  - "A named collection of related information"
  - μΌλ°μ μΌλ‘ λΉνλ°μ±μ λ³΄μ‘°κΈ°μ΅μ₯μΉμ μ μ₯ (ex. νλλμ€ν¬)
  - μ΄μμ²΄μ λ λ€μν μ μ₯ μ₯μΉλ₯Ό fileμ΄λΌλ λμΌν λΌλ¦¬μ  λ¨μλ‘ λ³Ό μ μκ² ν΄ μ€
  - Operation (μ°μ°)
    - create, read, write, reposition (lseek), delete, open, close λ±
- **File attribute (νΉμ νμΌμ metadata)**
  - νμΌ μμ²΄μ λ΄μ©μ΄ μλλΌ νμΌμ κ΄λ¦¬νκΈ° μν κ°μ’ μ λ³΄λ€
    - νμΌ μ΄λ¦, μ ν, μ μ₯λ μμΉ, νμΌ μ¬μ΄μ¦
    - μ κ·Ό κΆν (μ½κΈ°/μ°κΈ°/μ€ν), μκ° (μμ±/λ³κ²/μ¬μ©), μμ μ λ±
- **File system**
  - μ΄μμ²΄μ μμ νμΌμ κ΄λ¦¬νλ λΆλΆ
  - νμΌ λ° νμΌμ λ©νλ°μ΄ν°, λλ ν λ¦¬ μ λ³΄ λ±μ κ΄λ¦¬
  - νμΌμ μ μ₯ λ°©λ² κ²°μ 
  - νμΌ λ³΄νΈ λ±



## β¨ Directory and Logical Disk

- **Directory**
  - νμΌμ λ©νλ°μ΄ν° μ€ μΌλΆλ₯Ό λ³΄κ΄νκ³  μλ μΌμ’μ νΉλ³ν νμΌ
  - κ·Έ λλ ν λ¦¬μ μν νμΌ μ΄λ¦ λ° νμΌ attribute λ€
  - operation
    - search for a file, create a file, delete a file
    - list a directory, rename a file, traverse the file system
- **Partition (=Logical Disk)**
  - νλμ (λ¬Όλ¦¬μ ) λμ€ν¬ μμ μ¬λ¬ νν°μμ λλκ² μΌλ°μ 
  - μ¬λ¬ κ°μ λ¬Όλ¦¬μ μΈ λμ€ν¬λ₯Ό νλμ νν°μμΌλ‘ κ΅¬μ±νκΈ°λ ν¨
  - (λ¬Όλ¦¬μ ) λμ€ν¬λ₯Ό νν°μμΌλ‘ κ΅¬μ±ν λ€ κ°κ°μ νν°μμ file system μ κΉκ±°λ swapping λ± λ€λ₯Έ μ©λλ‘ μ¬μ©ν  μ μμ



## β¨ `open()`

- open("/a/b/c")
  - λμ€ν¬λ‘λΆν° νμΌ cμ λ©νλ°μ΄ν°λ₯Ό λ©λͺ¨λ¦¬λ‘ κ°μ§κ³  μ΄
  - μ΄λ₯Ό μνμ¬ directory path λ₯Ό search
    - λ£¨νΈ λλ ν λ¦¬ "/"λ₯Ό open νκ³  κ·Έ μμμ νμΌ "a" μ μμΉ νλ
    - νμΌ "a"λ₯Ό open ν ν read νμ¬ κ·Έ μμμ νμΌ "b"μ μμΉ νλ
    - νμΌ "b"λ₯Ό open ν ν read νμ¬ κ·Έ μμμ νμΌ "c"μ μμΉ νλ
    - νμΌ "c"λ₯Ό openνλ€
  - Directory pathμ searchμ λλ¬΄ λ§μ μκ° μμ
    - Openμ read / write μ λ³λλ‘ λλ μ΄μ μ
    - νλ² open ν νμΌμ read / write  μ directory search λΆνμ
  - Open file table
    - νμ¬ open λ νμΌλ€μ λ©νλ°μ΄ν° λ³΄κ΄μ (in memory)
    - λμ€ν¬μ λ©νλ°μ΄ν°λ³΄λ€ λͺ κ°μ§ μ λ³΄κ° μΆκ°
      - Open ν νλ‘μΈμ€μ μ
      - File Offset : νμΌ μ΄λ μμΉ μ κ·Ό μ€μΈμ§ νμ (λ³λ νμ΄λΈ νμ)
    - File descriptor (file handle, file control block)
      - Open file table μ λν μμΉ μ λ³΄ (νλ‘μΈμ€ λ³)

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220919143252143.png" alt="image-20220919143252143" style="zoom: 80%;" />



## β¨ File Protection

- κ° νμΌμ λν΄ λκ΅¬μκ² μ΄λ€ μ νμ μ κ·Ό (read/write/execution)μ νλ½ν  κ²μΈκ° ?
- **Access Control λ°©λ²**

#### π‘ Access control Matrix

![image-20220919152719749](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220919152719749.png)

		- Access control list : νμΌλ³λ‘ λκ΅¬μκ² μ΄λ€ μ κ·Ό κΆνμ΄ μλμ§ νμ
		- Capability list : μ¬μ©μλ³λ‘ μμ μ΄ μ κ·Ό κΆνμ κ°μ§ νμΌ λ° ν΄λΉ κΆν νμ



#### π‘ Grouping

- μ μ²΄ userλ₯Ό owner, group, publicμ μΈ κ·Έλ£ΉμΌλ‘ κ΅¬λΆ
- κ° νμΌμ λν΄ μΈ κ·Έλ£Ήμ μ κ·Ό κΆν (rwx) μ 3λΉνΈμ©μΌλ‘ νμ
- μ ) UNIX![image-20220919153047861](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220919153047861.png)



#### π‘Password

- νμΌλ§λ€ passwordλ₯Ό λλ λ°©λ² ( λλ ν λ¦¬ νμΌμ λλ λ°©λ²λ κ°λ₯ )
- λͺ¨λ  μ κ·Ό κΆνμ λν΄ νλμ password : all-or-nothing
- μ κ·Ό κΆνλ³ password: μκΈ° λ¬Έμ , κ΄λ¦¬ λ¬Έμ 



## β¨ File Systemμ Mounting

![image-20220919153307842](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220919153307842.png)



## β¨ Access Methods

- μμ€νμ΄ μ κ³΅νλ νμΌ μ λ³΄μ μ κ·Ό λ°©μ
  - μμ°¨ μ κ·Ό (sequential access)
    - μΉ΄μΈνΈ νμ΄νλ₯Ό μ¬μ©νλ λ°©μμ²λΌ μ κ·Ό
    - μ½κ±°λ μ°λ©΄ offset μ μλμ μΌλ‘ μ¦κ°
  - μ§μ  μ κ·Ό (direct access, random access)
    - LP λ μ½λ νκ³Ό κ°μ΄ μ κ·Όνλλ‘ ν¨
    - νμΌμ κ΅¬μ±νλ λ μ½λλ₯Ό μμμ μμλ‘ μ κ·Όν  μ μμ



## β¨ Allocation of File Data in Disk

- Contiguous Allocation (μ°μ ν λΉ)
- Linked Allocation (μ°κ²° ν λΉ)
- Indexed Allocation 



### π’ Contiguous Allocation (μ°μ ν λΉ)

- λ©λͺ¨λ¦¬ κ΄λ¦¬ νμ΄μ§ κΈ°λ²κ³Ό μ μ¬

![image-20220919153722558](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220919153722558.png)

- μ₯μ 
  - Fast I/O
    - νλ²μ seek / rotation μΌλ‘ λ§μ λ°μ΄νΈ transfer
    - Realtime file μ©μΌλ‘, λλ μ΄λ―Έ run μ€μ΄λ processμ swapping μ©
  - Direct access(=random access) κ°λ₯
- λ¨μ 
  - external fragmentation
  - File grow κ° μ΄λ €μ (νμΌ ν¬κΈ°λ₯Ό ν€μ°κΈ° μ΄λ €μ)
    - file μμ±μ μΌλ§λ ν° holeμ λ°°λΉν  κ²μΈκ° ?
    - grow κ°λ₯ vs λ­λΉ (internal fragmentation)



### π’ Linked Allocation

![image-20220919155833585](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20220919155833585.png)

- μ₯μ 
  - External fragmentation λ°μ μ ν¨
- λ¨μ 
  - No random access
  - Reliability λ¬Έμ 
    - ν sectorκ° κ³ μ₯λ pointerκ° μ μ€λλ©΄ λ§μ λΆλΆμ μμ
  - Pointerλ₯Ό μν κ³΅κ°μ΄ blockμ μΌλΆκ° λμ΄ κ³΅κ° ν¨μ¨μ±μ λ¨μ΄λ¨λ¦Ό
    - 512 bytes/sector, 4 bytes/pointer
- λ³ν
  - File-allocation table (FAT) νμΌ μμ€ν
    - ν¬μΈν°λ₯Ό λ³λμ μμΉμ λ³΄κ΄νμ¬ reliabilityμ κ³΅κ°ν¨μ¨μ± λ¬Έμ  ν΄κ²°



### π’ Indexed Allocation

![image-20220919170642399](assets/image-20220919170642399.png)

- μ₯μ 
  - External fragmentation μ΄ λ°μνμ§ μμ
  - Direct access κ°λ₯
- λ¨μ 
  - Small fileμ κ²½μ° κ³΅κ° λ­λΉ (μ€μ λ‘ λ§μ fileλ€μ΄ small)
  - Too Large file μ κ²½μ° νλμ block μΌλ‘ index λ₯Ό μ μ₯νκΈ°μ λΆμ‘±
    - ν΄κ²° λ°©μ
      1. linked scheme (λ λ€λ₯Έ indexλ₯Ό κ°λ¦¬ν€κ² νλ κ²)
      2. multi-level index



## β¨UNIX νμΌμμ€νμ κ΅¬μ‘°

![image-20220919174141207](assets/image-20220919174141207.png)

#### π‘ μ λμ€ νμΌ μμ€νμ μ€μ κ°λ

- **Boot block** (μ΄λ€ νμΌ μμ€νμ΄λ  λͺ¨λ boot blockμ΄ κ°μ₯ μμ μμ)
  - λΆνμ νμν μ λ³΄ (bootstrap loader)
- **Superblock**
  - νμΌ μμ€νμ κ΄ν μ΄μ²΄μ μΈ μ λ³΄λ₯Ό λ΄κ³  μλ€.
- **Inode list**
  - νμΌ μ΄λ¦μ μ μΈν νμΌμ λͺ¨λ  λ©ν λ°μ΄ν°λ₯Ό μ μ₯
- **Data block**
  - νμΌμ μ€μ  λ΄μ©μ λ³΄κ΄



## β¨ FAT File System

![image-20220919174908383](assets/image-20220919174908383.png)



## β¨ Free-Space Management

#### π‘Bit map or bit vector

![image-20220919181207135](assets/image-20220919181207135.png)

- Bit map μ λΆκ°μ μΈ κ³΅κ°μ νμλ‘ ν¨
- μ°μμ μΈ n κ°μ free block μ μ°Ύλλ° ν¨κ³Όμ 



#### π‘ Linked list

- λͺ¨λ  free block λ€μ λ§ν¬λ‘ μ°κ²° (free list)
- μ°μμ μΈ κ°μ©κ³΅κ°μ μ°Ύλ κ²μ μ½μ§ μλ€
- κ³΅κ°μ λ­λΉκ° μλ€



#### π‘ Grouping

- linked list λ°©λ²μ λ³ν
- μ²«λ²μ§Έ free blockμ΄ nκ°μ pointerλ₯Ό κ°μ§
  - n-1 pointer λ free data block μ κ°λ¦¬ν΄
  - λ§μ§λ§ pointerκ° κ°λ¦¬ν€λ blockμ λ λ€μ n pointer λ₯Ό κ°μ§



#### π‘ Counting

- νλ‘κ·Έλ¨λ€μ΄ μ’μ’ μ¬λ¬ κ°μ μ°μμ μΈ blockμ ν λΉνκ³  λ°λ©νλ€λ μ±μ§μ μ°©μ
- (first free block, # of contiguous free blocks) μ μ μ§



## β¨ Directory Implementation

- Linear list
  - <file name, fileμ metadata>μ list
  - κ΅¬νμ΄ κ°λ¨
  - λλ ν λ¦¬ λ΄μ νμΌμ΄ μλμ§ μ°ΎκΈ° μν΄μλ linear search νμ (time-consuming)
- Hash Table
  - linear list + hashing
  - Hash tableμ file nameμ μ΄ νμΌμ linear listμ μμΉλ‘ λ°κΎΈμ΄μ€
  - search timeμ μμ°
  - Collision λ°μ κ°λ₯ 

![image-20220919182044877](assets/image-20220919182044877.png)



- Fileμ metadata μ λ³΄κ΄ μμΉ
  - λλ ν λ¦¬ λ΄μ μ§μ  λ³΄κ΄
  - λλ ν λ¦¬μλ ν¬μΈν°λ₯Ό λκ³  λ€λ₯Έ κ³³μ λ³΄κ΄
    - inode, FAT λ±
- Long file nameμ μ§μ
  - <file name, file μ metadata>μ listμμ κ° entryλ μΌλ°μ μΌλ‘ κ³ μ  ν¬κΈ°
  - file nameμ΄ κ³ μ  ν¬κΈ°μ entry κΈΈμ΄λ³΄λ€ κΈΈμ΄μ§λ κ²½μ° entry μ λ§μ§λ§ λΆλΆμ μ΄λ¦μ λ·λΆλΆμ΄ μμΉν κ³³μ ν¬μΈν°λ₯Ό λλ λ°©λ²
  - μ΄λ¦μ λλ¨Έμ§ λΆλΆμ λμΌν directory fileμ μΌλΆμ μ‘΄μ¬

![image-20220919191058230](assets/image-20220919191058230.png)



## β¨ VFS and NFS

- Virtual File System (VFS)
  - μλ‘ λ€λ₯Έ λ€μν file system μ λν΄ λμΌν μμ€ν μ½ μΈν°νμ΄μ€ (API) λ₯Ό ν΅ν΄ μ κ·Όν  μ μκ² ν΄μ£Όλ OSμ layer
- Network File System (NFS)
  - λΆμ° μμ€νμμλ λ€νΈμν¬λ₯Ό ν΅ν΄ νμΌμ΄ κ³΅μ λ  μ μμ
  - NFSλ λΆμ° νκ²½μμμ λνμ μΈ νμΌ κ³΅μ  λ°©λ²μ

![image-20220919191947289](assets/image-20220919191947289.png)



## β¨ Page Cache and Buffer Cache

- **Page Cache**

  - 4kb

  - Virtual memory μ paging system μμ μ¬μ©νλ page frame μ cachingμ κ΄μ μμ μ€λͺνλ μ©μ΄
  - Memory-Mapped I/Oλ₯Ό μ°λ κ²½μ° fileμ I/O μμλ page cache μ¬μ©

- **Memory-Mapped I/O**

  - Fileμ μΌλΆλ₯Ό virtual memoryμ mapping μν΄
  - λ§€νμν¨ μμ­μ λν λ©λͺ¨λ¦¬ μ κ·Ό μ°μ°μ νμΌμ μμΆλ ₯μ μννκ² ν¨
  - λ©λͺ¨λ¦¬μ μ΄λ―Έ μ¬λΌμ¨ κ²μ μ»€λμ μ°μ§ μκ³  μμ μ΄ μ§μ  ν  μ μλ€

- **Buffer Cache**

  - νμΌμμ€νμ ν΅ν I/O μ°μ°μ λ©λͺ¨λ¦¬μ νΉμ  μμ­μΈ buffer cache μ¬μ©
  - File μ¬μ©μ locality νμ©
    - ν λ² μ½μ΄μ¨ blockμ λν νμ μμ²­ μ buffer cacheμμ μ¦μ μ λ¬
  - λͺ¨λ  νλ‘μΈμ€κ° κ³΅μ©μΌλ‘ μ¬μ©
  - Replacement algorithm νμ (LRU, LFU λ±)

- **Unified Buffer Cache**

  - μ΅κ·Όμ OSμμλ κΈ°μ‘΄μ buffer cache κ° page cacheμ ν΅ν©λ¨

![image-20220919193541816](assets/image-20220919193541816.png)

![image-20220919193627900](assets/image-20220919193627900.png)



## β¨ νλ‘κ·Έλ¨μ μ€ν

![image-20220919195500464](assets/image-20220919195500464.png)

![image-20220919195658065](assets/image-20220919195658065.png)