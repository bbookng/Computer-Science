# 05_ARP ํ๋กํ ์ฝ

#### ๐ก ARP๊ฐ ํ๋ ์ผ

- **๊ฐ์ ๋คํธ์ํฌ ๋์ญ์์ ํต์ ์ ํ๊ธฐ ์ํด ํ์ํ MAC ์ฃผ์๋ฅผ IP ์ฃผ์๋ฅผ ์ด์ฉํด์ ์์๋**
- ๋ฐ์ดํฐ๋ฅผ ๋ณด๋ด๊ธฐ ์ํด์๋ 7๊ณ์ธต๋ถํฐ ์บก์ํ๋ฅผ ํตํด ๋ฐ์ดํฐ๋ฅผ ๋ณด๋ด๊ธฐ ๋๋ฌธ์ IP ์ฃผ์์ MAC ์ฃผ์๊ฐ ๋ชจ๋ ํ์ํ๋ค
  - ์ด ๋ IP ์ฃผ์๋ง ์๋ฉด ARP ๋ฅผ ํตํด ํต์ ์ด ๊ฐ๋ฅํ๋ค.



#### ๐กARP ํ๋กํ ์ฝ์ ๊ตฌ์กฐ

![image-20221017184629857](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017184629857.png)

- Hardware type : 0001 (์ด๋๋ท)

- protocol type : 0800 (IPv4)

- hardware address length : 06

- protocol address length : 04

  ์ด๊น์ง๋ ๋ณ๋์ฌํญ ์์

---

- Opcode(Operation Code) ์ด๋ป๊ฒ ๋์ํ๋์ง๋ฅผ ๋ํ๋ : 000X
  - ์์ฒญ (0001) or  ์๋ต (0002)
- ์ถ๋ฐ์ง, ๋์ฐฉ์ง์ MAC ์ฃผ์์ IP ์ฃผ์



#### ๐ก ARP ํ๋กํ ์ฝ์ ํต์  ๊ณผ์ 

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017195400598.png" alt="image-20221017195400598" style="zoom:67%;" />

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017195550166.png" alt="image-20221017195550166" style="zoom:67%;" />

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017195625900.png" alt="image-20221017195625900" style="zoom:67%;" />

- ๋ชจ๋ฅด๋ MAC ์ฃผ์๋ ARP(00000000) or ์ด๋๋ท (FFFFFFFF) ๋ก ์ฑ์
  - FFFFFFFF ๋ broadcast (์ ์ฒด์๊ฒ ๋ค ๋ณด๋)

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017195823069.png" alt="image-20221017195823069" style="zoom:67%;" />

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017195902422.png" alt="image-20221017195902422" style="zoom:67%;" />

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017200011787.png" alt="image-20221017200011787" style="zoom:67%;" />

- ๋ชฉ์ ์ง IP์ ๋ณธ์ธ IP๊ฐ ๊ฐ์ PC ์์ ์๊ธฐ MAC ์ฃผ์๋ฅผ ์จ์ ARP ์๋ต
- ๋๋จธ์ง PC ์์๋ ํจํท์ ๋ฒ๋ฆผ
- ์ถ๋ฐ์ง์์ ์๋ต์ ๋ฐ์ ํ MAC ์ฃผ์๋ฅผ ARP ์บ์ ํ์ด๋ธ์ ์ ์ฅ



#### ๐ก ARP ํ์ด๋ธ

- ๋์ ํต์ ํ๋ ์ปดํจํฐ๋ค์ ์ฃผ์๋ ARP ํ์ด๋ธ์ ๋จ๋๋ค.



#### ๐ก ์ค์ต

```bash
arp -a
```

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017200240315.png" alt="image-20221017200240315" style="zoom: 67%;" />