# 06. 원거리 네트워크

#### 💡 IPv4 프로토콜

- 20 byte



#### 💡 IPv4 가 하는 일

- **네트워크 상에서 데이터를 교환하기 위한 프로토콜**

- 정확하게 전달될 것을 **보장하지 않는다.**

  - 중복된 패킷을 전달 or 패킷의 순서를 잘못 전달할 가능성도 있다.
  - 악의적인 이용시 DDOS

- 데이터의 정확하고 순차적인 전달은 그보다 상위 프로토콜인 TCP 에서 보장한다.

  ![](C:\Users\Bokyeong\Desktop\CS\Computer-Science\Network\assets\ip45.png)

- Version : 4
- HearderLength : 20byte ~ 60byte
  - 4 bit 로 어떻게 표현 ? 나누기 4 해서 표현한다. 5 ~ 15
- TOS (Type of Service) : 옛날에 쓰던거 (비워둠)
- Total Length : Payload 를 포함한 전체 길이
- Identification : 패킷의 데이터 식별자
- IP Flag (bit)
  - D(Dont' Fragmentation) : 데이터를 안쪼개서 보내겠다. (최대 단위보다 크면 전송 안됨)
  - M(More Fragmentation) : 최대 전송단위보다 큰걸 보내면 무조건 설정
    - 뒤에 있으면 1, 없으면 0
- Fragment Offset : 데이터 순서
  - 현재까지 전송된 용량 / 8
- TTL (Time to Live) : 패킷이 살아있는 시간을 지정한다.
  - 만약 정상적으로 패킷이 routing 되지 않았을 때, 순환하면 일정 시간 후 사라지게 함 (버림)
  - 운영체제마다 다름, window : 128, Linux : 64
- Protocol : 상위 프로토콜이 무엇인지 말해주는 것 ICMP(1), TCP(6), UDP(17)
- Header Checksum : Header 에 오류가 있는지 없는지 확인하기 위한 값
- 출발지, 도착지 IP 주소 (4byte 씩)



### 📌 ICMP 프로토콜

- ICMP (Internet Control Message Protocol, 인터넷 제어 메시지 프로토콜)
- 네트워크 컴퓨터 위에서 돌아가는 운영체제에서 **오류 메시지**를 전송 받는 데 주로 쓰인다.
- 프로토콜 구조의 Type 과 Code 를 통해 오류 메시지를 전송 받는다.

#### 💡 ICMP 프로토콜의 구조

- **ICMP 프로토콜** : 특정 대상과 내가 통신이 잘되는지 확인하기 위한 프로토콜

![](C:\Users\Bokyeong\Desktop\CS\Computer-Science\Network\assets\1023.png)

- **Type (대분류)**
  - 8 (요청), 0 (응답)
  - 3 : 목적지 도달 불가능 ( Destination Unreachable )
    - 경로 설정이 잘못 되었음
  - 11 : 목적지까지 갔지만 응답을 받지 못함 ( Time Exceeded)
    - 상대방에 문제가 있음 ex) 방화벽 때문에 컷 당함
  - 5 : ICMP redirect
    - 남의 Routing Table 바꾸는 것
    - 현재 사용 X
- **Code (소분류)**
- Checksum



#### 💡 라우팅 테이블

- 최적의 경로를 저장해 놓은 테이블

```bash
netstat -r
```

- routing table 에 네트워크 대상이 없으면 찾아갈 수 없다.
  - 0.0.0.0 (기본값) => 기본 게이트웨이로 보냄



#### 💡 다른 네트워크와의 통신 과정

![](C:\Users\Bokyeong\Desktop\CS\Computer-Science\Network\assets\234.png)

- **요청**
  - 개인 PC(A) → 스위치 → 공유기(이더넷 프로토콜 다시 짬) → 라우터(이더넷 프로토콜 다시 짬) → 공유기(이더넷 프로토콜 다시 짬) → 스위치 → 개인 PC(B)
  - 만약 Routing table에 주소가 없으면 ARP 시켜서 알아왔다가 다시 보냄



### 📌 IPv4의 조각화

#### 💡 조각화

- 큰 IP 패킷들이 적은 `MTU(Maximum Transmission Unit)` 을 갖는 링크를 통하여 전송되려면 **여러 작은 패킷으로 쪼개어 (조각화 되어) 전송되어야 한다.**
  - MTU 는 IPv4 헤더 (20~60byte) 포함
- 목적지까지 패킷을 전달하는 과정에 통과하는 각 라우터마다 전송에 적합한 프레임으로 변환이 필요하다.
- 조각화 후, 최종 목적지까지 가야 재조립된다.
  - IPv4 에서는 발신지 뿐만 아니라 중간 라우터에서도 IP 조각화가 가능
  - IPv6 에서는 IP 단편화가 발신지에서만 가능
  - 재조립은 항상 최종 목적지에서만 가능

![](C:\Users\Bokyeong\Desktop\CS\Computer-Science\Network\assets\sddsf.png)

- 보통 MTU의 값은 1500 byte + 이더넷 프로토콜 (14 byte) = 패킷의 크기 (1514byte)