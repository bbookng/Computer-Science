# 05_ARP 프로토콜

#### 💡 ARP가 하는 일

- **같은 네트워크 대역에서 통신을 하기 위해 필요한 MAC 주소를 IP 주소를 이용해서 알아냄**
- 데이터를 보내기 위해서는 7계층부터 캡슐화를 통해 데이터를 보내기 때문에 IP 주소와 MAC 주소가 모두 필요하다
  - 이 때 IP 주소만 알면 ARP 를 통해 통신이 가능하다.



#### 💡ARP 프로토콜의 구조

![image-20221017184629857](C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017184629857.png)

- Hardware type : 0001 (이더넷)

- protocol type : 0800 (IPv4)

- hardware address length : 06

- protocol address length : 04

  이까지는 변동사항 없음

---

- Opcode(Operation Code) 어떻게 동작하는지를 나타냄 : 000X
  - 요청 (0001) or  응답 (0002)
- 출발지, 도착지의 MAC 주소와 IP 주소



#### 💡 ARP 프로토콜의 통신 과정

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017195400598.png" alt="image-20221017195400598" style="zoom:67%;" />

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017195550166.png" alt="image-20221017195550166" style="zoom:67%;" />

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017195625900.png" alt="image-20221017195625900" style="zoom:67%;" />

- 모르는 MAC 주소는 ARP(00000000) or 이더넷 (FFFFFFFF) 로 채움
  - FFFFFFFF 는 broadcast (전체에게 다 보냄)

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017195823069.png" alt="image-20221017195823069" style="zoom:67%;" />

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017195902422.png" alt="image-20221017195902422" style="zoom:67%;" />

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017200011787.png" alt="image-20221017200011787" style="zoom:67%;" />

- 목적지 IP와 본인 IP가 같은 PC 에서 자기 MAC 주소를 써서 ARP 응답
- 나머지 PC 에서는 패킷을 버림
- 출발지에서 응답을 받은 후 MAC 주소를 ARP 캐시 테이블에 저장



#### 💡 ARP 테이블

- 나와 통신했던 컴퓨터들의 주소는 ARP 테이블에 남는다.



#### 💡 실습

```bash
arp -a
```

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221017200240315.png" alt="image-20221017200240315" style="zoom: 67%;" />