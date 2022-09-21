kocw 이화여대 반효경교수 운영체제 14년 1학기 수업

# 3장. Process

## 프로세스의 개념

- Process is a program in execution

### 프로세스의 문맥(context)

- 실행 중인 프로세스의 현재 상태를 나타내는 데 필요한 모든 요소
    - Program Counter가 code의 어디를 가리키고 있는 가
    - stack(유저 스택 또는 커널 스택)에 무언가 쌓여 있는가
    - data의 현재 값은 무엇인가
    - 운영체제가 현재 이 프로세스를 어떻게 평가하고 있는가
    - 등등
- 현대의 컴퓨터는 time sharing, multi tasking 등의 방식으로 동작하기 때문에 문맥의 파악이 중요함(백업같은 의미, 어디까지 실행했었는지 알아야 함)
- CPU 수행 상태를 나타내는 하드웨어 문맥
    - Program Counter
    - 각종 register
- 프로세스의 주소 공간
    - code, data, stack
- 프로세스 관련 커널 자료 구조
    - PCB(Process Control Block)
    - Kernel stack

### 프로세스의 상태(status)

- 프로세스는 상태(state)가 변경되며 수행된다
- Running
    - CPU를 잡고 instruction을 수행중인 상태
- Ready
    - CPU를 기다리는 상태(메모리 등 다른 조건을 모두 만족한 상태)
- Blocked(wait, sleep)
    - CPU를 주어도 당장 instruction을 수행할 수 없는 상태
    - Process 자신이 요청한 event(I/O 등)가 즉시 만족되지 않아 이를 기다리는 상태
    - 예. 디스크에서 file을 읽어와야 하는 경우
    - 프로세스 자신이 요청한 event가 완료되면 ready 상태가 된다
- Suspended(stopped)
    - 외부적인 이유로 프로세스의 수행이 정지된 상태(Swapper에 의해 쫓겨난 경우 등)
    - 프로세스는 통째로 디스크에 swap out 된다
    - 예. 사용자가 프로그램을 일시정지시킨 경우(break key) 시스템이 여러 이유로 프로세스를 잠시 중단시킴(메모리에 너무 많은 프로세스가 올라와 있을 때)
    - 외부 요인에 의해 중단되었기 때문에 외부에서 resume해줘야 active됨
- New: 프로세스가 생성중인 상태
- Terminated: 수행(execution)이 끝난 상태(정리 작업이 남음)
- 프로세스 상태도(running, ready, blocked)
    
    ![Screen Shot 2022-09-12 at 9.48.19 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a6754d0b-545c-44ec-92af-59306e10b290/Screen_Shot_2022-09-12_at_9.48.19_PM.png)
    
- 프로세스 상태도(suspended 추가 및 좀 더 자세히)
    
    ![Screen Shot 2022-09-13 at 9.03.03 AM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b2485952-5dd4-43af-9568-2a65ff05c970/Screen_Shot_2022-09-13_at_9.03.03_AM.png)
    

### Process Control Block(PCB)

- 운영체제가 각 프로세스를 관리하기 위해 프로세스당 유지하는 정보
- 다음의 구성 요소를 가진다(구조체로 유지)
    1. OS가 관리상 사용하는 정보
        - Process state, Process ID
        - scheduling information, priority
    2. CPU 수행 관련 하드웨어 값
        - Program Counter, registers
    3. 메모리 관련
        - code, data, stack의 위치 정도
    4. 파일 관련
        - Open file descriptors, etc.

### 문맥 교환(Context Switch)

- CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정
- CPU가 다른 프로세스에게 넘어갈 때 운영체제는 다음을 수행
    - CPU를 내어주는 프로세스의 상태를 그 프로세스의 PCB에 저장
    - CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어 옴
- 참고. System call이나 Interrupt 발생시 반드시 context switch가 일어나는 것은 아님
    - Sytem call이나 Interrupt 가 발생하면 사용자 프로세스로부터 운영체제로 CPU 제어권이 넘어감
    - 유저 모드에서 커널 모드로 바뀐 뒤 다시 유저 모드로 바뀔 때 동일한 프로세스를 이어서 실행하면 context switch가 아니다
    - timer interrupt 또는 I/O 요청 system call 등에 의한 유저 모드 → 커널 모드 전환의 경우 목적 및 효율 때문에 다른 프로세스로 CPU 권한이 넘어가 실행되고, 이 때는 실행되는 프로세스가 바뀌었기 때문에 context swtich이다.

### 프로세스를 스케줄링하기 위한 큐

- Job queue
    - 현재 시스템 내에 있는 모든 프로세스의 집합
- Ready queue
    - 현재 메모리 내에 있으면서 실행되기를 기다리는 프로세스의 집합
- Device queues
    - I/O device의 처리를 기다리는 프로세스의 집합
- 프로세스들은 각 큐들을 오가며 수행된다

### 스케줄러(Scheduler)

- 각각의 자원별로
- Long-term scheduler(장기 스케줄러 or job scheduler)
    - 메모리(및 각종 자원) 스케줄링
    - 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정(프로세스 상태도에서 new → ready queue 과정의 admitted는 메모리에 올라가는 것이 승인되었다는 의미로, job scheduler가 결정)
    - degree of Multiprogramming을 제어
    - time sharing system에는 보통 장기 스케줄러가 없음(무조건 ready) - 요즘 컴퓨터 시스템에는 장기 스케줄러가 없음
- Short-term scheduler(단기 스케줄러 or CPU scheduler)
    - CPU 스케줄링
    - 어떤 프로세스를 다음번에 running시킬지(CPU를 줄 지) 결정
    - 충분히 빨라야 함(millisecond 단위)
- Medium-term scheduler(중기 스케줄러 or Swapper)
    - 프로세스에서 메모리를 빼앗는다
    - 여유 공간 마련을 위해 프로세스를 통째로 메모리에서 디스크로 쫓아냄
    - 일단 메모리에 모든 프로세스가 다 올라가고, 그 중 일부를 쫓아냄으로써 메모리에 올라가 있는 프로세스의 개수를 조절
    - degree of Multiprogramming을 제어

## Thread

- A thread (or lightweight process) is a basic unit of CPU utilizaiton.
- 프로세스 내부에 CPU 수행 단위가 여러 개 있는 경우 thread라고 부른다.
- 동일한 프로세스의 각기 다른 부분을 실행하고 싶을 때 동일한 프로세스를 여러 개 만들면 메모리 공간이 낭비된다. 이때 프로세스를 하나만 만들고 pc를 여러 개 두어 각각 code의 다른 부분을 가리키게 하면 효율적으로 사용 가능하다.
- 예를 들어 웹브라우저에 창을 여러 개 띄우거나 워드 작업 파일을 여러 개 띄울 때 프로세스 하나로 스레드 사용 가능
    
    ![Screen Shot 2022-09-13 at 1.50.11 PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e58502fd-9d2e-4822-8f6c-3c31f4ad6eea/Screen_Shot_2022-09-13_at_1.50.11_PM.png)
    
- Thread의 구성
    - program counter
    - register set
    - stack space
- Thread가 동료 thread와 공유하는 부분(=task)
    - code section
    - data section
    - OS resources
- 전통적인 개념의 heavyweight process는 하나의 thread를 가지고 있는 task로 볼 수 있다.

### Thread 사용의 장점

- 다중 스레드로 구성된 태스크 구조에서는 하나의 서버 스레드가 blocked(waiting) 상태인 동안에도 동일한 태스크 내의 다른 스레드가 실행(running)되어 빠른 처리를 할 수 있다. (결과적으로 프로세스가 blocked 상태가 되지 않고 사용자 응답성이 좋아짐)
- 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율(throughput)과 성능 향상을 얻을 수 있다.
- 스레드를 사용하면 병렬성을 높일 수 있다.
- Responsiveness(응답성)
    - 예. multi-threaded Web - if one thread is blocked(eg network) another thread continues(eg display)
- Resource Sharing(자원의 공유)
    - n threads can share binay code, data, resource of the process
- Economy(경제적, 즉 빠름)
    - 프로세스 하나를 만드는 것보다 스레드 하나를 만드는 게 overhead가 훨씬 크다. 문맥 교환의 overhead도 스레드 스위칭보다 훨씬 크다.
    - creating & CPU switching thread (rather than a process)
    - Solaris의 경우 위 두 가지 overhead가 각각 30배, 5배
- Utilizaion of MP(Multiprocessor) Architectures
    - 프로세서 즉 CPU가 여러개일 때 각각의 스레드가 서로 다른 CPU에서 병렬적으로 작업 수행하여 효율 높아짐
    - each thread may be running in parallel on a different processor

### Implementation of Threads

- Some are supported by kernel → Kernel Threads
    - 커널 스레드는 스레드가 여러 개 있다는 사실을 운영체제가 알고 있다. 그래서 스레드를 바꿔주는 것도 커널이 지원해준다.
- Others are supported by library → User Threads
    - 유저 스레드는 프로세스 내부에서 스레드를 관리함
- Some are real-time threads
    - 리얼타임 기능을 지원하는 스레드