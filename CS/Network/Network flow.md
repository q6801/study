# 계층 캡슐화 + 웹 통신 흐름
- [PDU (Protocol Data Unit)](#pdu-protocol-data-unit)
- [데이터 캡슐화 (계층간 이동)](#데이터-캡슐화-계층간-이동)
- [웹 통신 흐름](#웹-통신-흐름)
- [참고](#참고)

</br>

# PDU (Protocol Data Unit)

- 각 계층(프로토콜)의 데이터 단위를 의미한다. (OSI 7 기준)
- 5, 6, 7 계층 : Message(Data)
- 4(전송) 계층 : Segment
- 3(네트워크) 계층 : Packets (Datagram)
- 2(데이터링크) 계층 : Frame
- 1(물리) 계층 : Bit
</br>

### PDU = SDU(Service Data Unit) + PCI(Protocol Control Information)

- SDU : 실제 서비스되는 data unit을 의미 (데이터 그 자체)
- PCI : Header로 PDU에 더해지는 제어 정보를 의미한다.
    - 흐름 제어 정보, 오류 제어 정보, 주소 정보 등이 포함되어있다.
</br>    

# 데이터 캡슐화 (계층간 이동)

- 데이터 송신자는 상위 계층에서 하위 계층으로 데이터를 보낸다. (application 계층부터 physical 계층으로)
- 각 계층의 **인코더**를 통해 **데이터를 캡슐화**하며 수신자 측의 하위 계층 디코더로 전송된다.
- 즉, 송신자 측에서 데이터를 보내면서 **헤더가 추가되는 과정**을 **캡슐화**라 하고 수신자가 **캡슐화된 헤더를 벗기는 것**을 **역캡슐화**라고 한다.

</br> 

# 웹 통신 흐름

### In 브라우저

- 브라우저가 URL에 적힌 값을 파싱하여 HTTP Request Message를 만들고, OS에 전송 요청을 한다.
- Domain으로 요청을 할 수 없으니 DNS Lookup을 수행한다.
    - dns 룩업 과정은 크롬의 경우 브라우저, hosts 파일, dns cache의 순서로 도메인에 매칭되는 ip를 찾는다.
    - 일반적으로 dns lookup은 루트 도메인 서버에서부터 서브도메인 서버 순으로 찾는다.

### in 프로토콜 스택, LAN 어댑터

- 프로토콜 스택(운영체제에 내장된 네트워크 제어용 소프트웨어)이 브라우저로부터 메시지를 받는다.
- 브라우저로부터 받은 메시지를 패킷 속에 저장한다.
- 패킷에 제어 정보를 덧붙여 lan 어댑터에 전송하고, lan 어댑터는 다음 Hop의 MAC주소를 붙인 프레임을 전기신호로 변환시켜 송출한다.

### in 허브, 스위치, 라우터

- LAN 어댑터가 송신한 프레임은 스위칭 허브를 경유하여 인터넷 접속용 라우터에 도착한다.
- 라우터는 패킷을 프로바이더(통신사, ISP)에게 전달한다.
- 인터넷으로 들어가게 된다.

### in 엑세스 회선, 프로바이더

- 패킷은 인터넷의 입구에 있는 액세스 회선(통신 회선)에 의해 POP(Point Of Presence, 통신사용 라우터)까지 운반된다.
- POP 를 거쳐 인터넷의 핵심부로 들어가게 된다.
- 수 많은 고속 라우터들 사이로 패킷이 목적지를 향해 흘러가게 된다.

### in 방화벽, 캐시서버

- 패킷은 인터넷 핵심부를 통과하여 웹 서버측의 LAN 에 도착한다.
- 기다리고 있던 방화벽이 도착한 패킷을 검사한다.
- 패킷이 웹 서버까지 가야하는지 가지 않아도 되는지를 판단하는 캐시서버가 존재한다. (이건 웹서버에 도착하지 않는 경우를 의미)

### in 웹 서버

- 패킷이 물리적인 웹 서버에 도착하면 웹 서버의 프로토콜 스택은 패킷을 추출하여 메시지를 복원하고 웹 서버 애플리케이션에 넘긴다.
- 메시지를 받은 웹 서버 애플리케이션은 요청 메시지에 따른 데이터를 응답 메시지에 넣어 클라이언트로 회송한다.
- 왔던 방식대로 응답 메시지가 클라이언트에게 전달된다.

</br> 

# 참고

[https://github.com/ksundong/backend-interview-question](https://github.com/ksundong/backend-interview-question)

[네트워크의 흐름 2단계](https://willseungh0.tistory.com/144)

- 프로토콜 스택