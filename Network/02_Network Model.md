# 02. 네트워크 모델

![](C:\Users\Bokyeong\Desktop\CS\Computer-Science\Network\assets\Untitled (1).png)

### 💡 `TCP / IP 모델`

- 1960 년대 말 미국방성의 연구에서 시작되어 1980 년대 초 프로토콜 모델로 공개,
  현재의 인터넷에서 컴퓨터들이 서로 정보를 주고받는데 쓰이는 통신 규약(프로토콜)의 모음이다.



### 💡 `OSI 7 계층`

- 1984 년 네트워크 통신을 체계적으로 다루는 ISO 에서 표준으로 지정한 모델 ,
  데이터를 주고받을 때 데이터 자체의 흐름을 각 구간별로 나누어 놓은 것.



### 💡 `TCP / IP 모델` vs `OSI 7 계층`

- 공통점
  - 계층적 네트워크 모델
  - 계층간 역할 정의
- 차이점
  - 계층의 수 차이
  - OSI 는 역할 기반, TCP/IP 는 프로토콜 기반
  - OSI 는 통신 전반에 대한 표준
  - TCP / IP 는 데이터 전송기술 특화



### 💡 패킷

- 네트워크 상에서 전달되는 데이터를 통칭하는 말
- 네트워크에서 전달하는 데이터의 형식화된 블록
- 제어 정보 (헤더, 풋터) + 사용자 데이터 (페이 로드)

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221010163041105.png" alt="image-20221010163041105" style="zoom: 80%;" />



#### 📌 캡슐화

- 여러 프로토콜을 이용해서 최종적으로 보낼 때, **패킷을 만드는 과정**
- 상위 계층에서 하위 계층으로 **내려가면서 프로토콜을 붙인다.**



#### 📌 디캡슐화

- 패킷을 받았을 때, 프로토콜들을 하나씩 확인하면서 **데이터를 확인하는 과정**



#### 📌 계층별로 이름이 다른 PDU (Protocol Data Unit)

- 4 계층의 PDU = `세그먼트`

  <img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221010163610397.png" alt="image-20221010163610397" style="zoom:67%;" />

- 3 계층의 PDU = `패킷`

  <img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221010163553476.png" alt="image-20221010163553476" style="zoom:67%;" />

- 2 계층의 PDU = `프레임`

<img src="C:\Users\Bokyeong\AppData\Roaming\Typora\typora-user-images\image-20221010163528378.png" alt="image-20221010163528378" style="zoom: 67%;" />



### 💡 실습

1. 프로토콜의 캡슐화 된 모습과 계층별 프로토콜들을 확인해보기
   - Wireshark 를 이용하여 패킷을 캡쳐 해보고 해당 패킷이 어떻게 캡슐화 되었는지 자세히 살펴본다.

- ping(Packet INternet Groper)
  - `네트워크 상태를 확인하려는 대상 노드를 향해 일정 크기의 패킷을 전송`
  - 해당 노드의 패킷 수신 상태와 도달하기까지 시간등을 알 수 있으며, 해당 노드까지 네트워크가 잘 연결되어 있는지 확인 할 수 있음
  - TCP/IP 프로토콜 중 ICMP를 통해 동작
    - → 해당 프로토콜을 지원하지 않는 기기를 대상으로는 실행 불가능
    - → 네트워크 정책상 ICMP나 traceroute를 차단하는 대상의 경우 pring 테스팅 불가능

![](C:\Users\Bokyeong\Desktop\CS\Computer-Science\Network\assets\Untitled (2).png)