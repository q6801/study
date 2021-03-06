# HTTPS
- [HTTP](#http)
- [HTTP의 문제점](#http의-문제점)
- [HTTPS](#https-1)
- [SSL Handshake](#ssl-handshake)
- [참고](#참고)
</br>

# HTTP

- www상에서 정보를 주고받을 수 있는 프로토콜
- 클라이언트와 서버 사이에 이뤄지는 요청/응답 프로토콜이다.
- 주로 HTML 문서를 주고받는 데 사용된다.
</br>

# HTTP의 문제점

- TCP/IP는 도청 가능한 네트워크이다.
    - TCP/IP는 통신 경로 상에서 엿볼 수 있다.
    - 즉, 패킷을 수집하는 것만으로 도청할 수 있다.
    - 그래서 암호화하여 통신해야한다.
- 통신 상대를 확인하지 않기 떄문에 위장이 가능하다.
    - HTTP는 상대가 누구인지 확인하는 처리가 없어서 누구든지 리퀘스트를 보낼 수 있다.
    - 반대로 리스폰스하는 게 의도한 웹서버인지도 확인할 수 없다.
    
    ```python
    1. 리퀘스트를 보낸 곳의 웹 서버가 원래 의도한 리스폰스를 보내야 하는 웹 서버인지 확인할 수 없다.
    2. 리스폰스를 반환한 곳의 웹서버가 원래 의도한 리스폰스를 보내야 하는 웹 서버인지 확인할 수 없다.
    3. 통신하고 있는 상대가 접근이 허가된 상대인지 확인할 수 없다.
    4. 어디에서 누가 리퀘스트 했는지 확인할 수 없다.
    5. 의미없는 리퀘스트도 수신한다. -> DoS 공격을 방지할 수 없다.
    ```
    
- 완전성을 증명할 수 없어서 변조가 가능하다.
    - 서버 또는 클라이언트가 수신한 내용이 수신측에서 보낸 내용과 일치하다는 것을 보장할 수 없는 것이다.
    - 공격자가 도중에 request나 response를 빼앗아 변조하는 중간자 공격을 당할 수 있다.
</br>

# HTTPS

- HTTP의 보안상의 문제를 해결하기 위해 등장한 프로토콜
- HTTP에 SSL 또는 TLS를 이용해 암호화하여 주고받는다.
- 모든 HTTP 요청과 응답 데이터는 네트워크로 보내지기 전에 전송 계층과 응용 계층 사이에서 암호화된다.
- 통신 패킷을 sniffing하더라도 정보를 보호 할 수 있다.
- SSL을 통해 상대를 확인할 수 있다. 확인하는 수단으로 제3기관 증명서를 제공하고 있다.
- 보안을 위해 사용하는 세션키(대칭키)를 생성 및 교환하는 과정이 필요하다
    - 그 과정이 SSL Handshake임
- 기존의 HTTP는 TCP와 직접 통신했지만 HTTPS는 HTTP ↔ SSL ↔ TCP 이렇게 통신이 이뤄진다.
- https는 제 3자 인증, 공개키 암호화, 비밀키 암호화를 사용한다.
    - 제 3자 인증 : 믿을 수 있는 인증 기관에 등록된 인증서만 신뢰하는 것
    - 공개키 암호화 : 비밀키를 공유하기 위해 사용
    - 비밀키 암호화 : 통신하는 데이터를 암호화할 때 사용
</br>

# SSL Handshake

- SSL Handshake의 핵심은 비대칭키를 이용한 인증과정을 통해 서로 공유하는 대칭키를 생성하는 것
- 이 과정에서 certificate authority를 통해 서버의 공개키를 확인하고 증명된 사이트의 경우 공개키 서명방식을 이용해 서로 인증하게된다.
</br>

### 과정

- 클라이언트는 TCP 3way handshake를 수행한 이후 브라우저가 사용하는 ssl/tls 버전 정보, 브라우저가 지원하는 암호화 방식 등을 보낸다.
- 서버는 인증서와 선택한 암호화 방식 등을 보내준다.
- 클라이언트는 받은 인증서를 신뢰하기 위해서 등록된 인증기관인지 확인합니다.
    - 이 인증서는 인증기관의 개인키로 암호화되어있고, 공개키로 검증할 수 있습니다.(브라우저에 내장되어있음)
    - 클라이언트는 사이트의 정보와, 서버의 공개키를 얻을 수 있습니다.
- 클라이언트는 웹 서버 인증서에 딸려온 공개키로 premaster secret을 암호화하여 서버로 전송한다.
- 서버는 premaster secret을 복호화하여 maseter secret으로 저장한다. 그리고 브라우저와 연결할 때 사용할 대칭키 암호화의 키인 세션키를 만든다.
- 서버의 공개키로 통신에 사용할 세션키를 암호화해서 서버에 보냅니다.
- 서버는 이를 개인키로 확인하고 이후 통신은 공유된 세션키로 암호화되어 통신합니다.
</br>

# 참고

[HTTPS 통신과정 쉽게 이해하기 #3(SSL Handshake, 협상)](https://aws-hyoh.tistory.com/entry/HTTPS-%ED%86%B5%EC%8B%A0%EA%B3%BC%EC%A0%95-%EC%89%BD%EA%B2%8C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-3SSL-Handshake)