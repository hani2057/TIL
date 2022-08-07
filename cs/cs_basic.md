# 컴퓨터 네트워크 기본

## Network Structure

### network edge

- applications and hosts

### network core

- routers
- network of networks

<br>

## Network edge: connection-oriented service

### TCP

- 특징
    - reliable, in-order byte-stream data transfer
        - 데이터 전송에 대한 신뢰성 확보
    - flow control
        - receiver의 능력에 따라 전송속도 조절
    - congestion control
        - network의 능력에 따라 전송속도 조절
- 대부분의 경우 TCP 사용

### UDP

- 특징
    - unreliable data transfer
    - no flow control
    - no congestion control
- 예. 전화통신(음성 패키지)

<br>

## Network core

### Circuit switching

- 회선을 지속적으로 돌면서 정보 전달

### Packet switching

- 패킷을 순서대로 전달
- 라우터에서 circuit 방식보다 훨씬 많은 유저에게 정보 전달 가능
- Four sources of packet delay
    1. nodal processing 
        - check errors, determine output link
        - 라우터 성능 개선으로 딜레이 줄일 수 있음
    2. queueing 
        - depends on congestion level of router
        - 사용자 의존적(인터넷 사용량이 많을수록 딜레이 발생)
        - 큐의 허용용량보다 처리해야 할 패킷의 양이 많을 경우 유실이 일어남
        - 유실이 일어난 경우 직전 라우터가 아닌 패킷이 처음 출발한 TCP에서 재전송함
    3. transmission delay 
        - packet length / link bandwidth
        - 회선 개선으로 딜레이 줄일 수 있음
    4. propagation delay 
        - 다음 라우터까지 도달하는 데 걸리는 시간
        - 빛의 속도 - 컨트롤X

<br>

## Protocol

- 통신 방식에 대한 약속

<br>

## 네트워크 계층구조

- App layer
    - HTTP 등
    - message
- transport layer
    - TCP / UDP
    - segment
- Network layer
    - IP 등
    - packet
- Link layer
    - WiFi / LTE / Ethernet(유선인터넷) 등
    - frame
- Physical layer