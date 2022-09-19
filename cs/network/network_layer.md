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

<br>

## Network Address Translation(NAT)

- 배경
    - IPv4는 32-bit 주소로 허용되는 host의 개수는 2**32개 약 40억개이다.
    - 1995년 미국의 인터넷 상용화로 host 개수 고갈이 예측되어 IPv6 (128bit)를 만들었다.(96년)
    - 현재 사용중인 인터넷은 IPv4임. 즉 한정된 host 개수를 어떠한 트릭을 이용해서 공유하여 사용중인 것.
- NAT 지원 라우터를 통해 공용 IPv4주소(WAN side address)와 내부 비공개 IPv4주소(LAN side address)를 변환하여 내부 네트워크에서 동일한 공용 IPv4주소를 공유하여 사용하도록 함
- 예시
    1. 클라이언트에서 TCP SYN 메시지를 웹 서버로 보낸다. 
    2. 클라이언트의 패킷은 NAT 라우터에 의해 개인 네트워크 인터페이스에서 수신된다. 아웃바운드 트래픽 규칙이 패킷에 적용된다. 즉, 발신자(클라이언트)의 주소는 NAT 라우터의 공용 IP 주소로 변환된고 발신자(클라이언트) 원본 포트 번호는 공용 인터페이스의 TCP 포트 번호로 변환된다.
    3. 패킷이 인터넷을 통해 전송되고 대상 호스트에 도달한다.
    4. 대상 호스트는 NAT 라우터의 인터넷 주소를 대상으로 하는 응답 패킷을 보낸다.
    5. 응답 패킷이 NAT 라우터에 도달한다. 이는 인바운드 패킷이므로 바인딩된 변환 규칙이 적용된다. 대상 주소가 원래 발신자(클라이언트)의 IP주소와 포트 번호로 다시 변경된다.
    6. 패킷이 내부 네트워크에 연결된 인터페이스를 통해 클라이언트에 전달된다.
- NAT 사용시 내부 네트워크의 인터넷 네트워크 주소와 발신자의 포트 번호는 공용 인터넷의 다른 호스트에 노출되지 않는다.
- 발생하는 문제점
    - 라우터가 forwarding 기능만 수행하지 않고 네트워크 주소 및 포트 번호 변환 기능을 수행한다.
    - Layer disturb 발생: NAT 라우터가 헤더 부분의 네트워크 주소 및 데이터 부분의 포트 번호까지 건드리게 됨

<br>

## Dynamic Host Configuration Protocol(DHCP)

- allows host to dynamically obtain its IP address from network server when it joins network
- dynamically get address from as server: “plug-and-play”
- address pool을 유연하게 분배하여 사용가능함

### DHCP client-server scenario

- client → server DHCP discover massege
    - src: 0.0.0.0. 68 (68번 포트를 열고 전송 후 기다림)
    - dest: 255.255.255.255. 67
        - broadcast 즉 subnet 내의 모든 host에게 전송함
        - 67번 포트가 열려있는 DHCP server 이외에는 전송된 메시지가 무시됨
    - yiaddr: 0.0.0.0
    - transaction ID: 654
- server → client DHCP offer
    - src: 223.1.2.5. 67 (src는 서버 자기자신)
    - dest: 255.255.255.255. 68
        - broadcast 즉 subnet 내의 모든 host에게 전송함 - 주소 확정 전
        - 68번 포트가 열려있는 client host 이외에는 전송된 메시지가 무시됨
    - yiaddr: 223.1.2.4
    - transaction ID: 654
    - lifetime: 3600secs
    - 여기에 subnet mask, router, DNS 등의 주소 정보도 전달됨
- client → server DHCP request
    - src: 0.0.0.0. 68
    - dest: 255.255.255.255. 67
    - yiaddr: 223.1.2.4
    - transaction ID: 655
    - lifetime: 3600secs
- server → client DHCP ACK
    - src: 223.1.2.5. 67
    - dest: 255.255.255.255. 68
    - yiaddr: 223.1.2.4
    - transaction ID: 655
    - lifetime: 3600secs
- 이까지 완료되면 host의 IP 주소가 확정된다.

<br>

## IP fragmentation, reassembly

- network links have MTU(maximum transfer size)
- large IP datagram divided(fragmented) within net
    - MTU 보다 큰 사이즈의 데이터는 여러개로 나뉘어 전송됨
    - 최종 목적지에서 다시 합쳐짐(reassembled only at fianl destination)
    - IP header bits used to identify order
        - fragflag: 나뉘었는지 여부. 뒤따르는 fragment가 있으면 1
        - offset: fragments 중 순서를 나타냄
- 예시
    - 4000 byte datagram (20byte header + 3980 byte data)
    - MTU = 1500 bytes
    - length=4000 / ID=x / fragflag=0 / offset=0
    - 나뉜다.
        1. length=1500 / ID=x / fragflag=1 / offset=0
        2. length=1500 / ID=x / fragflag=1 / offset=185 (185 = 1480/8 비트수 줄이기 위해 8로 나눠서 기록함)
        3. length=1040 / ID=x / fragflag=0 / offset=370 (370 = 2960/8)

