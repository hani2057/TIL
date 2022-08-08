# Transport Layer

## Multiplexing / Demultiplexing

- Multiplexing: sender 측에서 application layer의 프로세스들에서 transport layer로 전달된 message들을 묶어 segment로 만드는 작업
- Demultiplexing: receiver 측에서 segment로 전달된 message들을 application layer의 알맞은 프로세스(소켓)들로 올려보내는 작업
- segment의 header 내 source port와 dest(destination) port 필드에 적힌 정보를 통해 multiplexing을 한다.
- UDP 사용시 dest IP와 dest port 정보만 가지고 어떤 소켓으로 올릴지 결정한다. (IP 주소는 packet의 header 내 필드에 기록됨)
- TCP 사용시 src IP, src port, dest IP, dest port 의 네 개의 정보로 어떤 소켓으로 올릴지 결정한다.

<br>

## Piplined protocols

### Go-Back-N

- window size(N): sender가 한 번에 보내는 패킷 수의 단위
- cummulative ACK: “ACK 11” 이라고 하면 11번까지는 잘 받았다는 말
- receiver가 매우 단순해서 sender가 복잡한 프로세스를 다 처리한다.
- 중간에 패킷이 유실되면 도착한 다음 번호의 패킷들은 receiver가 모두 drop한다.
- sender는 유실된 패킷의 타이머 신호시 해당 패킷부터 윈도우 사이즈만큼 재전송한다.(go back N)

### Selective Repeat

- window size(N): sender가 한 번에 보내는 패킷 수의 단위
- individual ACK: “ACK 11” 이라고 하면 11번 패킷을 잘 받았다는 말
- 중간에 패킷이 유실되면 도착한 다음 번호의 패킷을 receiver가 저장해둔다.
- sender는 유실된 패킷의 타이머 신호시 해당 패킷만 재전송한다.
- receiver는 재전송된 패킷이 도착하여 패킷 순서가 올바르게 되면 app layer로 올려보낸다.
- 재전송된 패킷과 새로 보낸 패킷을 receiver가 구분하기 위해 seq #는 N보다 일정수준(필요한 만큼) 크게 설정해야 한다.

현실은 좀 더 복잡하기 때문에 go-back-N 방식과 selective repeat 방식이 적절히 섞여있다. 

- 타이머는 window size당 하나,
- cummulative ACK
- 유실된 패킷만 재전송 등

<br>

참고: header에 어떤 정보가 담기는지는 매우 중요하다 (TCP/UDP/IP)

## UDP

### segment header

- UDP의 header는 4개의 field를 가진다.
    - source port #
    - dest port # - demultiplexing에 사용
    - length
    - checksum - 데이터가 전송 도중에 에러가 있었는지 판단하여 에러 발생시 app layer로 올리지 않고 drop한다.
- UDP방식은 단순하지만 멀티플렉싱/디멀티플렉싱의 및 에러 체크 등 transport layer에서 필요한 기본적인 기능은 수행한다.

<br>

## TCP

### TCP: Overview

- point-to-point
    - 소켓과 소켓을 일대일로 연결
- reliable, in-order byte stream
- pipelined
- send & receive buffers
    - 유실되었을 때 재전송할 패킷들을 send buffer에 저장
    - 유실되었을 때 다음으로 들어오는 패킷들을 receive buffer에 저장
- full duplex data
    - 데이터가 양방향으로 전송됨
- connection-oriented
    - flow controlled


### 현실의 unreliable한 환경에서 어떤 프로세스를 통해 TCP에서 보장하는 reliability를 만들어줄 수 있는가?

1. channel이 reliable한 경우:
    - 그냥 보내면 됨
2. channel with packet errors:
    1. when packet has errors:
        - Error detection - add checksum bits
        - Feedback - receiver to sender, positive(ACK, acknowledgement) or negative(NAK, negative acknowledgement)
        - Retransmission
    2. when ACK or NAK has errors:
        - feedback에 에러가 있을 경우 패킷을 재전송
            - 이때 동일한 패킷을 재전송하는지 여부를 확인하기 위해 패킷별로 sequence #를 붙여 구분한다.
            - 예를 들어 receiver가 seq #0 의 패킷을 받았다면 다음번에 seq #1 의 패킷을 기대한다. 이때 seq #0의 패킷이 전송된다면 재전송이라는 걸 알 수 있다.
            - sequence # field를 포함한 header 내 필드의 크기는 최소화해야 한다.
        - NAK-free protocol
            - NAK 없이 ACK로만 피드백을 보낸다. 이때 마지막으로 정상적으로 도달한 seq #를 ACK를 보낼 때 같이 보낸다.
            - seq #1의 패킷을 보냈는데 seq #0의 ACK가 도착하면 전송 중 에러가 발생했다는 것을 sender가 알 수 있다.
3. channel with loss & packet errors:
    - sender가 packet을 전송할 때마다 timer를 작동시켜 time-out 발생시 packet을 재전송한다.
    - timer가 너무 빠른 시간으로 설정되면 불필요한 재전송이 일어날 수 있다.
    - timer가 너무 느린 시간으로 설정되면 loss 발생시 처리 속도가 늦어진다.

단, 효율(Utilization)을 위해 한 번에 여러개의 패킷을 보내는 Pipelined protocols 방식을 이용해야 한다.

### TCP Segment Structure

- segment: Transport Protocol의 전송단위 (packet은 Network Protocol의 전송단위임)
- source port # (16bit)
- dest port # (16bit)
- sequence # (32bit)
    - segment data(app layer로부터 전달된 message)의 가장 앞 byte #
- acknowledgement # (32bit)
    - cummulative ACK
- checksum (16bit)
- Receive window (16bit)
- 추가로 정보들이 header에 담김

### TCP seq # and ACKs

- sender의 send buffer seq #를 receiver의 receive buffer가 트래킹하여 따라간다.
- A와 B가 통신할 때 A의 app layer의 data의 첫 바이트를 통해 만든 seq #를 B의 receive buffer가 트래킹한다. (이때 예를 들어 42~49번까지 receive했다면 B가 A에게 보내는 통신의 ACK #는 그 다음 숫자인 50이 된다 - 49번까지 잘 받았고 50번을 기다리고 있음을 통지)
- 반대로 B의 app layer의 data의 첫 바이트를 통해 만든 seq #를 A의 receive buffer가 트래킹한다.
- 양방향으로 sender 및 receiver 역할을 수행할 때, 전송받은 패킷에 대한 ACK만 보낼 것인지 아니면 sender로서 보낼 data와 전송받은 패킷에 대한 ACK정보를 함께 보낼 것인지는 타이밍(약 500ms)에 따라 구분되어 통신이 진행된다.

### Timeout - function of RTT

- RTT(Round Trip Time): 패킷이 전송되고 ACK가 전송될 때까지 걸리는 시간
- segment마다 RTT가 다르다
- EstimatedRTT (보정된 RTT)값을 사용함
- TimeoutInterval = EstimatedRTT + 4*DevRTT
    - 편차가 큰 RTT를 수용 및 timeout은 실제 유실이 되었을 때 발생하도록 하기 위해 편차의 4배정도 되는 값을 더해서 실제 timeout interval value로 사용함

### TCP reliable data transfer

- TCP creates rdt service on top of IP’x unreliable service
- Pipelined segments
- Cummulative acks
- TCP uses single retransmission timer

### Fast retransmit

- 동일한 번호의 ACK가 3번 연달아 도착한다면 타이머가 터지기 전에도 해당 번호의 패킷이 유실되었다고 보아도 무방하다고 판단할 수 있다는 방식. (정상 + 3연속 duplicated ack ⇒ 총 4번 연속)