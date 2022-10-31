# 07. 클라이언트 서버

### 4계층 프로토콜

**4계층에서 하는 일**

- 전송 계층은 송신자의 **프로세스**와 수신자의 **프로세스를 연결하는 통신 서비스**를 제공
- 전송 계층은 `연결 지향 데이터 스트림 지원`, `신뢰성`, `흐름 제어`, `다중화` 등의 서비스 제공
- `TCP`, `UDP`

**4계층 프로토콜의 종류**

- UDP
  - 보내고 끝
- TCP
  - 안전한 연결을 지향

### 포트번호

**포트번호의 특징**

- 특정 프로세스와 특정 프로세스가 통신을 하기 위해 사용

- 하나의 포트는 

  하나의 프로세스만 사용 가능

  - 내 포트 하나가 상대방의 포트 여러개와 연결 가능

- 하나의 프로세스가 **여러개의 포트 사용 가능**

**Well-Known 포트**

![img](assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fef7a72f5-b856-40f0-ade9-a226deb9cc76%2F스크린샷_2022-07-23_오후_5.10.44.png)

**Register 포트**

![img](assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd46553cf-01c8-4046-9358-e3dce09276e4%2F스크린샷_2022-07-23_오후_5.15.34.png)

**Dynamic 포트**

![img](assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fd9d15acc-512a-4f49-8bc1-b9ce7bf0860e%2F스크린샷_2022-07-23_오후_5.11.48.png)

- Client는 위번호 중 랜덤 포트를 이용하여 외부 프로세스 이용

**포트 주소 + IP 주소 + MAC주소**