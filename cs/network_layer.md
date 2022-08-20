# Network layer

- 라우터를 통해 segment를 sending host로부터 receiving host로 전달한다.
- 네트워크에서 데이터를 전송하는 역할을 하는 라우터에는 PHY - Link - Network layer까지 쌓인 스택이 존재한다. (그 위 Transport, Application layer는 없음)
- 네트워크 레이어에는 프로토콜이 IP(Internet Protocol) 한 종류밖에 없다

<br>

## Router에서 하는 일

- Forwarding
    - 들어온 패킷의 목적지 주소와 forwarding table의 엔트리를 매칭시켜 엔트리에 해당하는 목적지로 패킷을 보내는 작업
    - forwarding table은 주소 범위로 분류되어 있다.
    - Longest prefix matching
        - 여러 엔트리에 동시에 해당되는 경우, 가장 길게 매칭이 된(가장 구체적으로 잘 맞는) 엔트리에 해당하는 인터페이스로 보낸다.
- Routing
    - Forwarding table을 만들어주는 작업
    - 해당 작업을 routing algorithm이라고 한다.

<br>

## IP datagram format(헤더에 적힌 정보)

- ver
    - IP버전. 현재는 버전4
- length
    - 데이터의 길이
- source IP address
- destination IP address
- checksum
    - 에러체크
- time to live(ttl)
    - 라우터를 하나씩 거칠 때마다 -1되어 0이 되면 해당 패킷은 소멸함
- upper layer
    - TCP인지 UDP인지
- 기타

<br>

## IP Address(IPv4)

- A unique 32-bit number
- Identifies an interface (on a host, on a router, …)
- Represented in dotted-quad notation
- 사람이 읽을 때는 사람이 읽기 편하도록 8bit씩 십진수로 바꿔서 읽는다

### Hierarchical Addressing: IP Prefixes

- IP Address는 Network ID와 Host ID의 두 부분으로 구분된다.
- 32bit IP 주소 중 Network ID는 앞부분부터이고, 나머지 뒷부분이 Host ID이다.
    - Network ID == Subnet ID == prefix
- Network ID의 bit수는 필요한 크기만큼 유연하게 설정된다.
    - Subnet Mask - Network ID의 길이를 식별하기 위한 32bit number
    - 예시. Network ID가 24bit일 경우 Subnet Mask는 255.255.255.0 (11111111  11111111  11111111  00000000)
- IP Address를 부여할 때 같은 네트워크에 속한 host들에게 동일한 prefix(Network ID)를 부여한다.
    - forwarding table이 단순해짐(구분 용이)
    - easy to add new hosts

### History of IP Address Allocation

- Classful Addressing
    - Class A: Network ID /8bit → 2**24개의 host를 가질 수 있다. 전 세계에 128개만 Class A를 가질 수 있음(시작은 0으로 해야 해서 7bit 즉 2**7만큼)
    - Class B: Network ID /16bit → 2**16개의 host를 가질 수 있다.
    - Class C: Network ID /24bit → 2**8개의 host를 가질 수 있다.
    - 불합리하고 불편해서 사용하지 않게 됨
- Classless Inter-Domain Routing(CIDR) - 현재 사용 방식
    - Network ID bit수를 자유롭게 조절할 수 있음
    - Use two 32-bit numbers to represent a network.
        - Network number = IP address + Mask
    - 예시
        - IP Address: 12.4.0.0 (00001100  00000100  00000000  00000000)
        - IP Mask: 255.254.0.0 (11111111  11111110  00000000  00000000)
        - Written as 12.4.0.0/15 (mask에서 표현된 prifix가 앞의 15bit)

### Longest Prefix Match Forwarding

- Destination-based forwarding
    - Packet has a destination address
    - Router identifies longest-matching prefix
        - matching되는 것 중 prefix의 길이가 가장 긴 것
    - Cute algorithmic problem: very fast lookups

<br>

## Subnets

- what’s a subnet?
    - 같은 prefix를 가진 device의 집합(device interfaces with same subnet part of IP address)
    - 라우터를 거치지 않고 접근이 가능한 host들의 집합(can physically reach each other without intervening router)
