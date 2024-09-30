Overlapped 모델을 사용한 asynchronous nonblocking socket

이벤트를 사용하여 이벤트의 flag를 확인하여 소켓을 사용하는 것이 특징

WSAevent 와 WSAOverlapped를 사용한 소켓

비동기식 논 블로킹 소켓 이라는 특징을 가지고 있다.

장점 - 확실히 처음 아무 모델도 사용하지않은 논블로킹 소켓보다는 코드가 깔끔해젔다.
       비동기식이라 recv()가 완료되었는지 매번 체크 할 필요없이 필요한 순간만 체크하면된다.

단점 - 그렇다고 이방법이 모든 걸 해결한 것은아니다.
       메인코드의 진행이 먼저 끝나서 결국 async로 처리한 recv()를 함수를 사용하여 기다리고있다.


주의 점
WSABUF wsaBuf;
wsaBuf.buf = session.recvBuffer;
wsaBuf.len = BUFSIZE;
를 사용중에
::WSAWaitForMultipleEvents(1, &wsaEvent, TRUE, WSA_INFINITE, FALSE);
::WSAGetOverlappedResult(session.socket, &session.overlapped, &recvLen, FALSE, &flags); 함수등을 사용하여 비동기식 일처리가 끝날떄가지 대기하고 있는 것이다.
그런데 만약 이렇게 기다리는 도중에 recvbuffer의 값이 오염된다거나 하는 상황이와서는 절때 안된다. 
ex) recv를 여러번 호출하고싶은 경우 서로 다른 recvbuffer를 사용해야한다.
