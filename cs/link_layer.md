# Link Layer

- host와 host 사이의 연결은 전용선이 아니라 여러 host끼리 연결된 모습의 채널의 형태(broadcast medium)이다.
- signal은 전기신호이므로 모든 가능한 경로로 퍼져나간다.
- 두 개 이상의 시그널이 동시에 채널에 전파되면 시그널이 섞여(collision, 충돌) 알아들을 수 없게 된다.

## Medium Access Control (MAC) protocol

- 충돌 방지 프로토콜

### An ideal multiple access protocol (이상적인 mac protocol)

- given: broadcast channel of rate R bps
- desire:
    - when one node wants to transmit, it can send at rate R
    - when M nodes want to transmit, each can sent at average rate R/M
    - fully decentralized(분산처리):
        - no special node to coordinate transmissions
        - no synchronization of clocks, slots
    - simple

### channel partitioning MAC protocols

- TDMA
    - TDMA: time division multiple access
    - 각 유저에게 사용할 수 있는 시간을 배분하는 방식
    - 자원이 낭비되는 문제 발생
- FDMA
    - FDMA: frequency division multiple access
    - 유저별로 특정 주파수를 배정하는 방식
    - TDMA와 마찬가지로 효율 문제 발생

### random access MAC protocols

- 유저가 원할 때 랜덤하게 채널에 access
- 충돌을 어떻게 방지할 것이며, 충돌이 발생했을 때 어떻게 해결할 것인지(처리방식)가 중요
- ALOHA
    - 가장 최초의 random access MAC protocols
- CSMA
    - CSMA: carrier sense multiple access
    - listen before transmit
    - 그럼에도 불구하고 전파 속도에 따른 딜레이(빛의 속도)를 0으로 만들 수 없기 때문에 충돌은 발생한다.
- CSMA/CD
    - backoff:
        - after nth collision, NIC chooses K at random from {0, 1, 2, … 2**(n-1)} (범위 중 랜덤한 시간을 기다리게 하여 또다른 충돌 가능성을 줄임)
        - longer backoff interval with more collisions (채널을 사용하고자 하는 복잡도가 증가할수록 오래 기다리도록 설계)

### taking turns MAC protocols

- polling
    - master node가 컨트롤
    - 이 경우 master node가 죽으면 전체가 다 죽는 단점이 있다
- token passing
    - token을 돌리면서 token을 가진 node가 transmit
    - 이 경우 token이 유실될 경우 문제 발생

각 방식이 다 장단점이 있다. 현실에서는 주로 random access 방식을 많이 사용함

## LAN (Local Area Network)

### Ethernet

- physical topology(구조)
    - bus: popular through mid 90s
    - star: prevails today
        - active: switch in center
- Ethernet uses CSMA/CD
    - collision detect시 frame을 재전송
    - 유선 인터넷 상황이기 때문에 collision이 발생하지 않을시 거의 100% 시그널이 도달한다고 본다.(케이블에 의해 보호받고 있기 때문)
    - Is it possible that: 충돌이 발생했는데 감지가 되지 않는 상황이 가능할까?
        - 다른 host에서 충돌 발생 감지 전까지 전송한 frame의 일부가 최초 frame 전송한 host에 도달하기 전에 최초 전송한 frame의 전송이 모두 완료되는 경우
        - 이런 코너 케이스를 방지하기 위해 이더넷에서 강제하는 minimum frame size (64byte)가 생김. 이보다 frame이 짧으면 padding 넣어줘서 frame을 전송함

### MAC addresses and ARP

- MAC address
    - 48bit
    - interface 자체의 고유 address
    - frame의 헤더정보에는 source MAC address와 destination MAC address가 들어간다.
- ARP
    - ARP table: IP address와 MAC address를 매핑해둔 표
    - frame에서 MAC address를 몰라서 DHCP를 통해 알게 된 Gateway router의 IP address 정보로 frame을 전송하면, Gateway router는 ARP table을 통해 전송된 frame의 헤더에 기록된 IP 주소가 자신의 것이면 frame을 전송받는다.
    - ARP tabled의 IP address 와 MAC address column은 캐시값이다. TTL column의 시간이 지나면 expire 되고 다시 캐시함