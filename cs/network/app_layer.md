# Application Layer

## Client-server architecture

- server:
    - always-on host
    - permanent IP address(IP 주소가 고정되어 있어야 함)
    - data centers for scaling
- clients:
    - ..

<br>

## Web and HTTP

### HTTP overview

- HTTP: hypertext transfer protocol
- request와 response의 두 가지로 통신
- uses TCP
- HTTP is ‘stateless’
- non-persistent HTTP / persistent HTTP
    - TCP connection을 지속적으로 유지하는지 여부에 따른 구분
    - 웹브라우저는 persistent HTTP를 디폴트로 사용하고 있음

<Br>

## Socket

### Socket

- 프로세스와 프로세스를 연결하기 위한 API의 일종
- 소켓의 타입: TCP Socket 과 UDP Socket의 두 가지가 있다.
- IP address: 인터넷상에서 컴퓨터의 위치를 나타냄
- port: 컴퓨터에서 특정 프로세스의 위치를 나타냄
- (IP주소, port #) 형태로 특정 컴퓨터의 특정 프로세스 주소 입력하여 소켓에 접근함

### Socket Functions

- TCP Server
    - socket()
        - int socket (int domain, int type, int protocol);
        - Creates a socket
        - socket의 id를 return
    - bind()
        - int bind (int sockfd, struct sockaddr*myaddr, int addrlen);
        - 특정 id의 소켓을 특정 포트의 소켓과 연결해줌
    - listen()
        - int listen (int sockfd, int backlog);
        - 바인딩된 소켓에 큐를 몇개까지 저장할 건지 지정
    - accept()
        - int accept (int sockfd, struct sockaddr*cliaddr, int* addrlen);
        - 연결을 기다림
        - 클라이언트의 주소가 서버에 저장됨(두 번째 인자)
    - read() 등등
    - close()
        - int close (int sockfd);
        - 데이터 교환이 끝난 후 연결을 종료
- TCP Client
    - socket()
    - connect()
        - 클라이언트측에서 bind 메서드를 쓰지 않는 이유는 아무 포트나 사용해도 되기 때문 (웹서버는 :80번 포트로 통일하여 사용하는 등 특정 포트에 바인딩하는 과정 필요)
    - write() 등등