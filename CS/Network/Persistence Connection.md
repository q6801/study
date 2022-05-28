# Persistent connection (TCP, HTTP)
- [지속적인 연결이 필요한 이유 (HTTP)](#지속적인-연결이-필요한-이유-http)
- [TCP의 keep alive 사용](#tcp의-keep-alive-사용)
- [http/1.1과 keep alive](#http11과-keep-alive)
- [HTTP/2.0과 persistent connection](#http20과-persistent-connection)
- [Connection 끊기](#connection-끊기)
  
</br>

# 지속적인 연결이 필요한 이유 (HTTP)

- HTTP 프로토콜은 connectionless라는 특징을 갖는다.
    - 이 뜻은 클라이언트가 서버에 요청을 하고 응답을 받으면 TCP/IP 연결을 끊어 연결을 유지하지 않는다는 것이다.
    - 빠르게 클라이언트의 TCP 연결을 끊어서 서버의 자원이 빠른 순환을 할 수 있게 돕는다.
    - 즉, connection을 계속 유지하면 더욱 많은 클라이언트의 접속에 대처할 수 없다.
- 하지만 연결을 유지한다면 다음과 같은 장점이 있다.
    - `네트워크 혼잡 감소` : TCP나 SSL/TCP의 connection request의 수가 줄어들기 때문
    - `네트워크 비용 감소` : 계속해서 client가 요청을 하는 경우 여러 개의 connection으로 전송하는 것보다 한 개의 connection을 사용하는 것이 더 효율적이다.
    - `latency 감소` : 매 요청마다 3 way hand shake로 연결을 할 필요가 없어지니 latency 감소
- 그래서 기본적으로 http는 connetionless라도 연결을 유지하는 방법이 필요하다.
  
</br>

# TCP의 keep alive 사용

- tcp에서 keep alive를 사용하는 이유는 두 가지다.
    - 죽은 peer를 구분해내기 위함
    - 네트워크 비활성화로 인한 연결 해제 방지
- TCP에서 연결 해지를 위해 4 way handshake 사용
    - 하지만 실제로는 한 쪽 시스템이 다운되어 연결이 완전히 끊겼지만, 다른 한 쪽은 정상적으로 동작하는 줄 아는 경우가 생긴다.
    - 이렇게 연결이 되어있는 줄 알고 계속 살아있는 세션을 **유령 세션**이라고 한다.
    - 한 쪽이 비정상 종료가 되었으면, 다른 한 쪽은 보낸 packet에 대한 ack이 오지 않아서 timeout된 packet을 계속 보내주는 문제점이 생긴다.
- **TCP keep alive는** 일정한 시간 간격으로 payload가 없는 **데이터를 전송하여 커넥션이 살아있는지 확인**한다.
    - 확인되면 미리 설정된 일정 시간만큼 tcp 세션을 연장한다.
  
</br>

# http/1.1과 keep alive

- HTTP 1.0의 connection은 하나의 request에 응답할 때마다 connection을 close하였다.
    - 그래서 persistent connection을 사용하기 위해 connection:keep-alvie 메시지를 헤더에 추가해 사용하였다.
- http1.1에서는 Connection: keep-alive를 http request, response에 포함하지 않아도, **모든 connections이 kee-alive 상태가 된다.**
    - 하위 호환성을 위해서 keep-alive를 request, response에 넣을 수도 있다.
- **근데 http1.1의 표준에는 포함되지 못함**
    - http/1.0+를 지원하지 않는 프록시 때문 (connection 헤더를 모르는 프록시)
    - 이러한 프록시가 네트워크의 중간에 있으면 keep-alive로 tcp 연결을 서버와 클라이언트가 지속해도 프록시는 뭔 말인지 몰라서 요청을 안받는다.
    - 즉, client가 connection:keep-alive를 보내면 client는 전송, 서버는 대기, 프록시는 데이터를 무시하는 상황이 벌어진다.
  
</br>


# HTTP/2.0과 persistent connection

- 2도 마찬가지로 기본적으로 persistent connection을 사용한다.
  
</br>

# Connection 끊기

- 어떤 client, server, proxy도 언제든지 TCP 커넥션을 끊을 수 있다.
- 하지만 서버는 커넥션을 끊는 시점에 클라이언트가 데이터를 보내지 않는다는 확신을 못한다.
    - 끊는 순간 클라이언트가 request를 보내면 문제가 생긴다.
- TCP 커넥션은 서버의 출력 채널만 끊는 것이 안전하다.
    - 입력 채널을 끊는다면 client에서 끊긴 입력 채널로 데이터를 전송할 수 있다.
    - 그러면 서버에서는 connection reset by peer 에러를 발생시키고, 버퍼에 저장된 아직 읽히지 않은 모든 request, response 데이터를 삭제한다.
- 즉, 클라이언트가 출력 채널을 끊으면 (혹은 더 이상의 데이터 전송이 없을것이라 알려주면) 커넥션을 안전하게 종료할 수 있다.
- TCP 4 handshake 참고

</br>

# 참고
[[Network 04] Persistent Connection을 위한 기술 01 - Keep Alive (TCP, HTTP)](https://blog.naver.com/PostView.naver?blogId=whdgml1996&logNo=222153047879&parentCategoryNo=&categoryNo=83&viewDate=&isShowPopularPosts=false&from=postList)

[How to make http2 requests with persistent connection ? (Any language)](https://stackoverflow.com/questions/37007100/how-to-make-http2-requests-with-persistent-connection-any-language)

[HTTP connection 관리](https://wiki.lucashan.space/http-guide/03-2.http-connection-management/)

[오류를 잡자 : TCP에는 우아한 종료라는 것은 없다.](https://sunyzero.tistory.com/269)

- graceful shutdown은 잘못된 해석으로 사용된다.