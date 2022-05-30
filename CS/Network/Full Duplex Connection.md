# 양방향 통신 (polling, long poling, websocket)
- [HTTP와 양방향 통신](#http와-양방향-통신)
- [polling](#polling)
- [long polling](#long-polling)
- [websocket](#websocket)
- [참고](#참고)

# HTTP와 양방향 통신

- http는 특성상 request가 없는 response를 보낼 수 없다. (양방향이 아닌 단방향만 가능한 프로토콜이다.)
- 그래도 서버가 클라이언트의 요청 없이도 보내주기 위해서 사용한 방법이 polling과 long polling이다.
  
</br>

# polling

- “일일히 상태를 저장하는 곳에서 상태를 살펴본다”라는 뜻 (여러 곳에서 자주 사용되는 단어임)
    - OS에서 나오는 I/O를 체크하는 밥법과 관련해서 사용될 때는 interrupt와 대비되는 개념으로 등장한다.
    - 비동신 통신이나 이벤트 기반 통신에서는 호출 프로세스에 작업 완료를 알리기 위한 방식으로 callback과 대비되는 단어이다.
- http에서는 클라이언트에게 지속적으로 연결을 끊지 않도록 평범한 http request를 날리는 방식을 의미한다.
    - 그러면 server에서 변경 사항에 대한 결과를 response로 보내준다.
- 이 방식의 구현을 주로 ajax를 사용하여 하기 때문에, ajax poling이라고도 한다.
- 가장 간단하게 동기화가 가능하지만, 지속적으로 request를 보내여 하는 만큼 서버의 부담이 커진다.
- 실시간에 가깝게 하려면 서버와 클라이언트 모두에게 부담이 심해서 어느정도 간격이 존재한다.

</br>


# long polling

- poling에서는 client가 resource를 server에 요청하면, client에게 resource를 **바로 넘겨줌**
- 하지만 long poling은 자원이 **준비될 때까지 대기**한다. (혹은 요청이 timeout 될 때까지)
- 일정 시간동안 이벤트가 일어나지 않아서 돌려줄 자원이 준비되지 않으면, 클라이언트로부터 새로운 request를 받아서 다시 대기한다.
- 이벤트가 발생하여 상태 변경이 일어나면, 해당 내용을 response로 돌려주고, 다시 request를 받아서 대기한다.
- request를 보내놓고 이벤트가 발생할 때까지 대기하는 방식이기 때문에 오버헤드가 존재한다.
- 실시간 통신이 가능하다.
    - 하지만 서버에서 데이터 변화가 자주 있어서 client의 요청 간격이 줄어든다면 request 횟수가 늘어 polling보다 오버헤드가 커질 수 있다.
- 동시 접속자 수가 늘어난다는 단점이 있다. (접속 유지 시간이 길다보니)

</br>

# websocket

- 가장 진화된 형태의 양방향 실시간 연결을 위한 프로토콜
- tcp/ip 기반의 application layer protocol이다.
- 클라이언트가 서버에 연결되어 있으면 이벤트가 발생할 때 마다 client의 request 없이, 해당 자원을 바로 client에게 전송할 수 있다.
  
</br>

# 참고
[[Network 05] Persistent Connection을 위한 기술 02 - poling, long poling (feat 양방향 통신)](https://blog.naver.com/PostView.naver?blogId=whdgml1996&logNo=222153682983&parentCategoryNo=&categoryNo=83&viewDate=&isShowPopularPosts=false&from=postList)

[코멧(Comet) #3 - Ajax 롱폴링(Ajax Long polling) 채팅방 예제로 배우기](https://dev.epiloum.net/1453)