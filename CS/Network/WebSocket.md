# WebSocket
- [개념](#개념)
- [웹소켓 핸드쉐이크](#웹소켓-핸드쉐이크)

</br>

# 개념
- 웹 애플리케이션과 서버 측 프로세스와의 양방향 통신을 간단한 방식으로 처리함
- HTTP 프로토콜을 이용해 연결을 수립하며 연결 된 이후에도 연결할 때 사용했던 80, 443 포트를 이용한다.
- 작동 방식
    - 서버와 클라이언트간의 웹소켓 연결을 HTTP 프로토콜을 통해 이룬다.
    - 연결이 정상적으로 이뤄진다면 서버와 클라이언트 간에 웹소켓 연결 (TCP/IP기반)이 이뤄지고 일정 시간이 지나면 HTTP 연결은 끊어진다.
- 문제점
    - 프로그램 구현에 보다 많은 복잡성 초래
        - http와 달리 stateful protocol이기 때문에 연결이 비정상적으로 끊기면 적잘한 대응이 필요하다.
    - 서버와 클라이언트 간의 socket 연결 유지 비용
        - 계속 연결을 유지한다는 것은 비용이 든다. 특히 트래픽 많은 서버의 경우 CPU에 큰 부담이 될 수 있다.

</br>

# 웹소켓 핸드쉐이크
- 브라우저는 헤더를 사용해 서버에게 웹소켓 지원 여부를 물어봄
- 서버가 지원한다고 응답을 하면 서버 - 브라우저간 통신은 HTTP가 아닌 웹소켓 프로토콜을 사용해 진행된다.
- 연결 과정
    - opening handshake
        - 웹소켓 클라이언트에서 핸드쉐이크 요청 (HTTP Upgrade)을 전송하고 이에 대한 응답으로 핸드 쉐이크 응답을 받는다.
        - 이 때 101 응답 코드를 받음, 101은 프로토콜 전환을 서버가 승인했다는 뜻
    - Data Transfer
        - 핸드쉐이크를 통해 웹소켓 연결이 수립되면, 데이터 전송 파트가 시작된다.
        - 핸드 쉐이크가 끝난 시점부터 서버와 클라이언트는 서로가 살아 있는지 확인하기 위해 heartbeat 패킷을 보내주며, 주기적으로 ping을 보내 체크한다.
    - close handshake
        - 클라이언트와 서버 모두 커넥션을 종료하기 위한 컨트롤 프레임을 전송할 수 있다.
        - ex) 서버가 커넥션을 종료한다는 프레임을 보내고, 클라이언트는 이에 대한 응답으로 close 프레임을 전송한다.
  
# 참고
- [[웹소켓] WebSocket의 개념 및 사용이유, 작동원리, 문제점](https://choseongho93.tistory.com/266)

- [WebSocket의 동작원리](https://kellis.tistory.com/65)