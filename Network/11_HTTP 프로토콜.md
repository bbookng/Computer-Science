# 11_HTTP 프로토콜



**웹을 만들기 위해 사용되는 다양한 기술들**

![img](assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Febc9c4bf-b39b-426f-9eef-43fb76536a26%2F_2022-07-30__11.50.15.png)

- HTTP : HTML, JS, CSS와 같은 파일을 웹 서버에 요청하고 받아오는 프로토콜

### HTTP 프로토콜 (HyperText Transfer Protocol)

- 특징
  - www에서 쓰이는 핵심 프로토콜
  - 문서의 전송, 이미지, 음성 등 여러 종류의 데이터를 MIME로 정의하여 전송 가능
  - Request / Response 동작에 기반하여 서비스 제공

**HTTP 요청 프로토콜**

- 요청하는 방식을 정의하고 클라이언트의 정보를 담고 있다.

  ![img](assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F208dd17b-7071-4a74-a3bb-01efe8767218%2F_2022-07-31__12.23.42.png)

- 리퀘스트

  ![img](assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F122253a9-d2f2-4b27-ba3c-0009185ee958%2F_2022-07-31__12.26.02.png)

  **요청 타입**

  - GET, POST, PUT, PATCH, DELETE

    ![img](assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F03c6d706-7e72-4d28-93d9-ba3988469607%2F_2022-07-31__12.27.08.png)

    GET : 주소창에 같이 보냄

    POST: Body에 파라미터를 담아서 보냄

    - 하지만 이것도 보일 수 있으니 https를 사용하여야 한다.

  **URI (Uniform Resource Identifier)**

  - scheme://host[:port][/path][?query]
  - 도메인 주소를 IP주소로 바꿈(DNS)

- 헤더

- 바디

**HTTP 응답 프로토콜**

- 사용자가 볼 웹 페이지를 담고 있음

  <img src="assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8c7d93ce-dfd3-41e2-9864-69ee66d834fd%2F_2022-07-31__12.56.08.png" alt="img" style="zoom:50%;" />

- Status Line

  ![img](assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fde2943b1-6864-4918-8122-43df93740f3b%2F_2022-07-31__12.56.41.png)

  - 상태 코드

    ![img](assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F6b5773e2-e085-4a57-bdb6-7e3f1b980bf8%2F_2022-07-31__12.57.52.png)

**HTTP 헤더**

![img](assets/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F712a083a-ca2f-4fd6-a8df-543577abf3a4%2F_2022-07-31__1.00.41.png)

- 일반 헤더

  - 일반적인 정보를 담고 있다.
  - Content-Length, Content-Type(ex. text/html)

- 요청 헤더

  - 클라이언트의 정보를 담고 있다.

  - `Cookie`: 서버로부터 받은 쿠키를 다시 서버에게 보내주는 역할

  - `Host` : 요청된 URL에 나타난 호스트명을 상세하게 표시(HTTP 1.1은 필수)

  - ```
    User-Agent
    ```

    : Client Program에 대한 식별가능 정보를 제공

    - 디바이스, 브라우저 등의 정보를 제공