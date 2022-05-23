# TCP 3, 4 way handshake
- [플래그 정보](#플래그-정보)
- [TCP 3 way hand shake](#tcp-3-way-hand-shake)
- [TCP 4 way hand shake](#tcp-4-way-hand-shake)
- [참고](#참고)

</br>

# 플래그 정보

- TCP header에는 CONTROL BIT가 존재하며, 각 bit는 URG, ACK, PSH, RST, SYN, FIN의 의미를 가진다.
- SYN (SYNchronize sequence numbers)
    - 연결 확인을 위해 sequence number를 무작위의 값으로 보낸다.
- ACK (Acknowledgement)
    - SYN 값에 1을 더해서 보내준 SYN을 받았다고 알려준다.

</br>

# TCP 3 way hand shake

- TCP 3way handshake는 논리적인 접속을 성립하기 위해서 사용
- 클라이언트는 서버에 요청을 전송할 수 있는지, 서버는 클라이언트에게 응답을 전송할 수 있는지 확인하는 과정이다.
    - 데이터 전송 전에 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.
- SYN, ACK 패킷을 주고받으며, 임의의 난수로 SYN 플래그를 전송하고, ACK 플래그에는 1을 더한값을 전송한다.
- SYN(n) → ACK(n+1), SYN(m) → ACK(m+1) 순으로 일어난다.
</br>

# TCP 4 way hand shake

- TCP 4way handshake는 TCP 연결을 해제하는 단계이다.
- 클라이언트는 서버에게 연결해제를 통지하고 서버가 이를 확인하여 클라이언트에게 이를 받았다는 ack을 전송해주고 최종적으로 연결이 해제된다.
- 서버에서 소켓이 닫혔다고 통지해도 **클라이언트는 일정 시간 대기**한다.
    - 혹시 패킷이 나중에 도착할 수 있기 때문이다. 이 과정을 time_wait이라 함
- step 1
    - 클라이언트가 연결을 종료하겠다는 FIN 플래그를 전송
- step 2
    - 서버는 일단 ack을 보내고 자신의 통신이 끝날때까지 기다리는데 이 상태가 time_wait상태이다.
- step 3
    - 서버가 통신이 끝났0.2으면 연결이 종료되었다고 클라이언트에게 FIN 플래그를 전송한다.
- step 4
    - 클라이언트는 확인했다는 메시지를 보낸다.
</br>

# 참고
[[네트워크] TCP/UDP와 3 -Way Handshake & 4 -Way Handshake](https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake)