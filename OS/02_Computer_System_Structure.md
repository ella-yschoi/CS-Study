# 02. 컴퓨터 시스템의 구조

> Reference: [이화여자대학교 운영체제 강의 - 반효경](http://www.kocw.net/home/cview.do?cid=4b9cd4c7178db077)

<br/>

## 운영체제의 종류

### 운영체제에 따른 오픈/비공개 소스 여부

- Windows: Closed Source Software
- Linux: Open Source Software
- Android:  Open Source Software (최하단에 Linux Kernel 포함)

### 어떻게 Open Source가 가능한가?

- Software 특성상 인건비를 제외한 원재료비가 들지 않기 때문
- 독점 체제가 가능한 시장이므로 1등 소프트웨어는 잘 팔고 나면 시장 장악하지만, 1등 외의 경쟁자들은 도태됨
- 2등 소프트웨어는 도태되기 전에 오픈하여, 사용자들이 여러 환경에 맞게 소스코드를 적용하여 점차 점유율이 높아지도록 함

<br/>

## 운영체제란 무엇인가?

### 운영체제란?

- 컴퓨터 하드웨어 바로 위에 설치되어, 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층

### 운영체제의 의미

- 좁은 의미의 운영체제 (커널)
  - 운영체제의 핵심으로, 메모리에 상주하는 부분
- 넓은 의미의 운영체제
  - 커널 뿐만 아니라, 각종 주변 시스템 유틸리티를 포함한 개념

<br/>

## 운영체제의 목적

- 컴퓨터를 **편리하게** 사용할 수 있는 인터페이스 제공
- 컴퓨터 시스템의 자원을 **효율적**으로 관리 (소프트웨어, 하드웨어 모두)
- 각각의 사용자는 하드웨어가 어떻게 실행되는지 모르도록 **운영체제가 대행**

### CPU의 역할

- CPU가 컴퓨터의 두뇌이긴 하나, 판단하는 능력은 없고 써져있는대로 빠르게 계산하고 실행함
- 따라서 어떻게 보면 컴퓨터의 두뇌는 CPU보단, 운영체제라고 볼 수도 있음
- CPU는 **계산**을 하고, 메모리는 **기억**을 하는 존재

### 운영체제의 역할

- 운영체제는 효율적으로 관리 및 운영하는 **통치자의 역할**이라고 보면 됨
- 한정된 메모리 공간에 동시에 여러 프로그램이 올라가면 적절히 할당하는 역할

<br/>

## 운영체제의 분류

### 동시 작업 가능 여부

- 단일 작업 (single tasking)
  - 한번에 **하나의 작업만** 처리
  - e.g. MS-DOS 프롬프트 상에서는 한 명령의 수행을 끝내기 전에 다른 명령을 수행시킬 수 없었음
- 다중 작업 (multi tasking)
  - 동시에 **두 개 이상**의 작업 처리
  - 다중 사용자가 쓸 수 있도록 보안/권한 문제에 대한 해결 필요
  - e.g. UNIX, MS Windows 등에서는 한 명령의 수행이 끝나기 전에 다른 명령이나 프로그램 수행 가능

### 사용자 수

- 단일 사용자 (single user)
  - e.g. MS-DOS, MS Windows
- 다중 사용자 (multi user): 동시 접속
  - UNIX, NT server
  - 다중 사용자가 쓸 수 있도록 보안/권한 문제에 대한 해결 필요

### 처리 방식

- 일괄 처리 시스템 (batch processing OS)
  - 작업 요청의 **일정량을 모아서 한꺼번에** 처리
  - 작업이 완전 종료될 때까지 기다려야 함
  - e.g. 초기 입출력하던 Punch Card 처리 시스템
  - e.g. ‘사용자를 생각해서 바로 결과를 보여주지 않고, 내가 편한대로 하겠다!’
- 시분할 시스템 (time sharing OS)
  - 여러 작업을 수행할 때 컴퓨터 처리 능력을 **일정한 시간 단위로 분할**하여 사용
  - 일괄 처리 시스템에 비해 **짧은 응답 시간**을 가짐
  - **Interactive**한 방식: 사용자를 생각해서 **바로 결과**를 보여줌
  - 컴퓨터를 동시에 사용해도, 시간을 분할해서 **마치 혼자** 사용하는 것처럼 보여줌
  - e.g. UNIX, 보통의 사용자가 이용하는 시스템
- 실시간 시스템 (Realtime OS)
  - 정해진 시간 안에 어떠한 일이 **반드시 종료됨이 보장**되어야 하는 실시간 시스템을 위한 OS
  - **Deadline**이라는 개념이 존재하고, 반드시 만족해야 함. 어기면 아주 치명적인 결과를 초래함
  - e.g. 원자로/공장 제어, 미사일 제어, 반도체 장비, 로봇 제어
  - 경성 실시간 시스템 (Hard realtime system)
    - Deadline을 어기면 상당히 치명적
  - 연성 실시간 시스템 (Soft realtime system)
    - Deadline을 어기면 상대적으로 덜 치명적
    - e.g. 동영상 플레이어 재생 시 프레임이 찍히지 않으면 영상이 끊김

### 용어 정리

아래 용어들은 컴퓨터에서 여러 작업들을 동시에 수행하는 같은 의미이나, 조금씩 강조하는 바가 다름.

- Multi-tasking
- Multi-process
- Multi-programming
  - memory 측면을 강조한 것
  - memory 위에 동시에 여러 프로그램들이 올라가 있다는 의미
- Time sharing
  - CPU 측면을 강조한 것
  - 주로 CPU의 시간을 분할하여 쪼개 쓴다는 의미

아래 용어는 아예 다른 의미

- Multi-processor
  - 하나의 컴퓨터에 CPU(Processor)가 여러 개 붙어 있음을 의미
  - 다만, 일반적인 환경은 아니고 고성능 컴퓨팅, 클라우드 컴퓨팅 분야에서 다룸

<br/>

## 운영체제의 예시

### UNIX

- 역사
  - 원래는 하드웨어적인 것도 관리가 필요해 assembly 언어로 개발이 매우 어려웠음
  - 이후 C언어의 등장으로 개발 및 수정이 수월하고 사람이 이해하기 편해짐
- 특징
  - 코드의 대부분을 C언어로 작성
  - C언어는 여러 아키텍처에서 호환이 되므로 **높은 이식성**을 가짐
  - 서버를 위한 운영체제 → 따라서 여러 프로그램, 여러 사용자 관리 필요
  - 최소한의 Kernel 구조
  - 오픈 소스
  - 프로그램 개발과 확장이 용이
  - 버전이 다양함 (e.g. Linux의 경우, 오픈소스이므로 이것을 기반으로 발전중)

### DOS (Disk Operating System)

- 역사
  - MS사에서 1981년에 IBM-PC를 위해 개발
- 특징
  - 단일 사용자용 운영체제
  - 메모리 관리 능력에 한계가 있음 (주 기억장치: 640KB)

### MS Windows

- 특징
  - MS사의 다중작업용 GUI 기반 운영체제
  - Plug and Play: 컴퓨터에 하드웨어를 연결하면 별도의 사용자 조작이나 프로그램 설치 없이 바로 사용할 수 있음
  - 네트워크 환경 강화
  - DOS용 응용 프로그램과 호환성 제공
  - 풍부한 지원 소프트웨어

<br/>

## 운영체제의 구조

![OS_structure](/Images/OS_structure.png)

<br/>

## 컴퓨터 시스템의 구조

![computer_system_structure](/Images/computer_system_structure.png)

<br/>

## Mode bit

### Mode bit이란?

- 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 **운영체제에 피해가 가지 않도록 하기 위한 보호장치**

### Mode bit의 필요성

- CPU가 사용자 프로그램에 넘어가면 **운영체제는 제어할 수 없기 때문**
- CPU에서 기계어를 실행할 때 운영체제 or 사용자 프로그램이 실행하는지 **구분하는 존재**가 필요함
- CPU가 제어 가능한 위치가 아닐 때 (다른 일을 하고 있을 때) mode bit이라는 것을 두어서 권한 제어를 하는 것
- 즉, Mode bit은 **안전한 기계어만 실행**할 수 있게끔 하여 **시스템을 보호**하는 역할

### Mode bit의 종류

Mode bit을 통해 하드웨어적으로 두 가지 모드의 operation 지원

#### 0 (모니터 모드, 커널 모드, 시스템 모드)

- 운영체제가 CPU를 실행 중일 때 (OS 코드 수행 중) **무슨 일이든** 해도 되는 상황
- 단, 운영체제가 사용자 프로그램에게 넘겨줄 때는 1로 바꿔서 넘겨줌
- 특권 명령(위험한 명령어)는 0일 때만, 운영체제만 실행 가능

#### 1 (사용자 모드)

- 사용자 프로그램 수행
- 사용자 프로그램이 CPU로 **안전한 기계어만** 실행할 수 있도록 함

### Mode bit의 원리

- 보안을 해칠 수 있는 중요한 명령어는 **모니터 모드(0)에서만 수행 가능한 특권 명령. 즉, 위험한 명령어**으로 규정
- 반대로, 사용자 프로그램에 CPU 사용 중, 특권 명령임에도 사용자 모드(1)인 상황이라면, 권한에도 없는 기계어를 실행한다고 확인 후, 자동으로 CPU가 운영체제에게 넘어감 (→ Interrupt와 Exception이라고 함)
- Exception 상황 발생 시, 다시 모니터 모드(0)로 전환되어 자동으로 CPU가 운영체제에게 넘어감
- 사용자 프로그램에게 CPU를 넘기기 전까지는 모니터 모드(0)로 실행되어야 하고, 사용자 프로그램이 실행되는 동안 Mode bit은 1로 설정됨

### Mode bit 전환 방식

![mode_bit](/Images/mode_bit.png)

#### Exception

- 권한이 없는 기계어 실행 시, **Mode bit이 0으로 바뀌면서** CPU 사용권이 운영체제에 넘어감
- 이는 권한이 없는 작업을 실행하려고 할 때 발생하는 Exception을 뜻함

#### Interrupt의 개요

- Interrupt line에서 매 순간 기계어를 읽다가, 다음 기계어를 읽기에 앞서, line에서 I/O 장치들이 준 Interrupt가 들어온건 없는지 체크
- 만약 Interrupt가 들어왔다면 CPU는 자동으로 운영체제에게 넘어감 → Mode bit이 0으로 바뀜 → 운영체제가 CPU를 잡아서 그 Interrupt에 대응함
- 이는 CPU Interrupt를 의미 (from Disk controller, I/O controller, etc.)

<br/>

## Registers

![registers](/Images/registers.png)

### Registers란?

- 연산의 input과 output을 저장하기 위한 빠르고 작은 크기의 것으로, CPU에 붙어 있음

### Registers의 종류

#### PC (Program Counter) Registers

- 다음번에 실행할 기계어의 memory의 주소를 가지고 있음 → 주소를 가리키고 있다는 뜻
- CPU는 PC Registers가 가리키고 있는 memory 주소의 기계어를 실행
- 실행이 끝나면 그 다음 위치의 기계어를 실행
- CPU Interrupt가 발생하면 PC Registers가 OS의 메모리 주소를 바라보고, 권한이 OS에 넘어간다. (즉, mode bit은 0이 된다)

<br/>

## Timer

### Timer란?

- 프로그램으로부터 CPU 사용권을 빼앗아 오는 것

### Timer의 역할

- 일정 시간이 지나면 Interrupt 를 발생 → **CPU의 사용권을 뺏어오는 역할**
  - CPU 사용권을 뺏어오는 작업은 OS가 혼자 할 수 없음
  - 정해진 시간이 흐른 뒤 **OS에게 제어권이 넘어가도록 Interrupt를 발생**
  - **CPU의 독점을 막기 위한 부가적인 hardware**가 필요 → 바로 Timer
- Time sharing을 구현하기 위해 널리 이용되며, 시간 계산 목적으로도 사용됨

### Timer의 동작 방식

- Timer는 매 clock tick 마다 1씩 감소
- Timer 값이 0이 되면 Timer Interrupt가 발생
- 운영체제가 사용자 프로그램에게 넘길 때 그냥 넘기는 것이 아닌, Timer에 시간을 세팅하고 넘겨줌
- 따라서 무한루프를 돌면서 CPU를 계속 쓰고 싶더라도 Timer가 CPU에게 interrupt를 걸기 때문에 CPU의 제어권이 운영체제에게 넘어옴 → 운영체제가 다른 프로그램에게 CPU 사용권을 넘겨주는 방식

<br/>

## Interrupt의 추가 설명

![interrupt_line](/Images/interrupt_line.png)

### Interrupt란?

- Interrupt 당한 시점의 Registers와 PC Registers를 save한 후, CPU의 제어를 Interrupt 처리 루틴에 넘기는 것
- 즉, CPU에 붙어 있는 Interrupt line이 세팅되어 다음 기계어를 실행하기 전에, CPU 제어권을 자동으로 운영체제에게 넘어가게 하는 것

### Interrupt의 두 가지 의미

- Interrupt (Hardware Interrupt)
  - 하드웨어가 발생시킨 Interrupt로, 더 일반적인 의미의 Interrupt
  - e.g. Timer, Disc Controller 등
- Trap (Software Interrupt)
  - 개별 프로그램이 운영체제에게 CPU를 넘기기 위해 소프트웨어가 발생시키는 Interrupt
  - e.g. Exception, System Call

### Interrupt 관련 용어

- Interrupt Vector
  - 해당 Interrupt 처리 루틴 주소를 가지고 있음
  - 운영체제의 어떤 코드를 실행해야 하는지 Interrupt 종류별로 실행할 코드의 위치를 담고 있음
  - **일종의 주소에 대한 포인터**라고 할 수 있음
- Interrupt 처리 루틴 (=Interrupt Service Routine, Interrupt Handler)
  - 해당 Interrupt를 처리하는 Kernel 함수
  - 정해진 주소로 가면, 무슨 일을 해야 하는지 기계어로 정해져 있음

<br/>

## System Call

![system_call](/Images/system_call.png)

### System Call이란?

- 사용자 프로그램이 운영체제의 서비스를 받기 위해 Kernel 함수를 호출하는 것
- 만약 I/O를 하고 싶은데 운영체제만이 할 수 있는 상황이라면, 스스로 Interrupt를 거는 것 → System Call
- CPU를 운영체제에 넘기고자 할 때 직접 PC(Program Counter)를 넘길 수 없기 때문에, 프로그램이 자신의 기계어를 통해서 Interrupt line 세팅 → Interrupt 발생 (여기서는 Software Interrupt)

### System Call의 원리

- CPU가 I/O를 요청하는 명령어는 전부 **특권 명령어로 묶여 있음**
  - 즉, 사용자 프로그램이 직접 실행할 수 없음
  - mode bit이 사용자 모드(1)일 때는 특권 명령을 수행할 수 없기 때문
- 따라서 운영체제에게 대신 해 달라고 요청(System Call) 필요
  - 사용자 프로그램 위치에서 기계어 실행되다가 → 운영체제에게 CPU 사용권을 넘김
  - 이때, 기계어가 실행되는 점프 발생 (프로그램의 가상 메모리를 가로질러 점프)

<br/>

## Device Controller

### I/O Device Controller란?

- 해당 I/O 장치 유형을 관리하는 일종의 작은 CPU
- 제어 정보를 위해 control register, status register 를 가짐
- local buffer를 가짐 (일종의 data register)

### Device Controller의 원리

- I/O 는 실제 device와 local buffer 사이에서 일어남
- **Device Controller는 I/O 가 끝났을 경우 Interrupt 로 CPU에게 그 사실을 알림**
- Device Driver에서 수행되는 코드는 '펌웨어' 라고 함
- I/O 장치의 펌웨어라는 미리 코딩된 프로그램이 들어 있기에 동작이 됨

### Device Controller 용어 정리

- Device Driver (장치 구동기)
  - OS 코드 중 각 장치별 처리 루틴 → software
  - CPU가 device controller에게 부탁을 하는 기계어
  - 컴퓨터 내부에서 CPU가 수행하는 코드
- Device Controller (장치 제어기)
  - 각 장치를 제어하는 일종의 작은 CPU  → hardware
  - Device Controller가 수행하는 코드가 아닌, CPU가 수행하는 코드

<br/>

## OS에 CPU가 넘어가는 경우들

### (1) 진정한 의미의 Interrupt

- Hardware 장치들이 Interrupt를 걸어 CPU가 넘어가는 경우

### (2) System Call

- 사용자 프로그램 Software가 직접 Interrupt line을 세팅해서 CPU가 넘어가는 경우

### (3) Trap

- 프로그램 본인에 요구에 의해 CPU가 넘어가는 경우 (Interrupt 라고는 할 수 없음)
- 다만 넓은 의미 Interrupt 즉, Trap정도로 해석

### (4) Exception

- 권한이 없는 기계어를 실행하는 경우
- 스스로 Interrupt가 걸려서 CPU가 넘어가는 경우

<br/>

## CPU를 내려놓는 경우

사용자 프로그램이 CPU를 가지고 있다가 다른 프로그램이나 운영체제에게 넘어가거나, 내려놓는 경우

### (1) 타의에 의한

- CPU를 쓰고 싶은데 독점권을 빼앗기는 경우
- e.g. Timer Interrupt

### (2) 자의에 의한

- 더이상 CPU를 사용하고자 하지 않는 경우
- e.g. 오래 걸리는 I/O 작업 시 (CPU를 가지고 있어도 I/O가 끝날 때까지 기다려야 하기 때문)

<br/>

## 현대의 운영체제는 **Interrupt**에 의해 구동

- 운영체제는 아무 일도 안하고, Interrupt가 들어올 때만 일한다는 의미
- 운영체제도 하나의 프로그램이기에, 막강한 권력을 가지고 CPU를 빼앗을 수 있다는게 아니라, **Interrupt가 들어오는 한 해 사용**할 수 있음
- 운영체제의 역할이 필요할 때는 CPU 독점을 막기 위해 **항상 Interrupt에 의해 넘어가도록 함**

<br/>

## 동기식 입출력과 비동기식 입출력

![synchronous_asynchronous](/Images/synchronous_asynchronous.png)

### (1) 동기식 입출력 (synchronous I/O)

- I/O 에서 일어나는 작업과 CPU에서 일어나는 작업이 시간적으로 잘 맞아야 하는 경우
- CPU가 I/O 요청 후, **입출력 작업이 완료된 후에야** 제어가 사용자 프로그램에 넘어감
- 즉, Disc에게 I/O 요청 후, I/O가 끝나면 컨트롤러가 Interrupt → **결과를 보고 서로 조율하며 그 다음 step을 밟아 나감**
- 비동기식보다 **더 일반적**인 입출력 방식임

#### 동기식 입출력 구현 방법 1

- I/O가 끝날때까지 CPU를 낭비시킴
- 매시점 **하나의 I/O만** 일어날 수 있음

#### 동기식 입출력 구현 방법 2

- I/O가 완료될 때까지 **해당 프로그램에게서 CPU를 빼앗음**
- 마냥 기다리고 있으면 CPU가 낭비되기 때문
- I/O 처리를 기다리는 줄에 그 프로그램을 줄 세움
- 기다리는 동안에는 다른 프로그램에게 CPU를 넘겨줌

### (2) 비동기식 입출력 (asynchronous I/O)

- I/O 에서 일어나는 작업과 CPU에서 일어나는 작업이 맞지 않아도 되는 경우
- I/O가 시작된 후, 입출력 작업이 끝나기를 기다리지 않고 제어가 사용자 프로그램에 즉시 넘어감
- 즉, CPU가 Disc에 요청하면, **요청과 결과와는 무관하게 요청받은 다음 일을 알아서 함**

> 동기식, 비동기식 모두 I/O의 완료는 Interrupt로 알려줌

<br/>

## DMA (Direct Memory Access)

![dma_controller](/Images/dma_controller.png)

![dma_picture](/Images/dma_picture.png)

### DMA의 필요성

- Interrupt가 너무 자주 걸리면 → overhead
- 따라서 Interrupt의 빈도를 줄이기 위해 사용됨

### DMA란?

- Device Controller가 CPU의 개입 없이 직접 Memory와 통신할 수 있도록 하는 메커니즘
- 간혹 빠른 속도를 가진 I/O 장치를 Memory에 가까운 속도로 처리하기 위해 사용됨
- 즉, DMA는 CPU의 개입 없이 메모리와 장치 사이에서 데이터 블록을 직접 copy 하여, Interrupt의 빈도를 줄이고, CPU의 효율성을 향상시킴

### DMA의 작동 원리

- Memory에 접근할 수 있는 장치는 CPU 뿐이기에, 원래는 CPU가 매번 Device Controller의 Interrupt에 의해 움직임 → 때때로 비효율적
- Device Controller가 DMA를 통해 데이터 블록을 Memory에 전송할 수 있고, 데이터 블록 전송이 완료되면 DMA Controller는 CPU에게 Interrrupt를 발생시킴
- 정리하자면, Memory에 직접 접근할 수 있는 DMA이라는 메커니즘을 하나 더 두어서 → Device에서 읽은 내용을 CPU Interrupt 없이 Device Controller가 **직접 메모리에 올려두는 작업**을 하고 → 작업이 끝나면 CPU에게 Interrupt를 걸어서 **한번에** 알려준다.

### DMA 사용 시 장점

- 위와 같이 한다면 Interrupt가 덜 빈번하게 발생하여 CPU가 효율적인 동작을 할 수 있음

<br/>

## 서로 다른 입출력 기계어

![special_instruction](/Images/special_instruction.png)

### I/O를 기계어를 통해 수행하는 두 가지 방법

#### (1) I/O만 전담하는 기계어를 두는 방법

- CPU에서 기계어 실행 시, 요청하는 기계어가 따로 있음
- Memory에 접근하는 기계어 따로 있고, I/O 수행하는 기계어가 따로 있음
- (위 그림에서 좌측) Device Addresses + Memory Addresses

#### (2) 메모리 접근 기계어가 I/O에 접근까지

- 메모리 주소를 실제 메모리 뿐만 아니라, I/O 장치에도 Primary Memory에 주소를 연장하고, 메모리 접근하는 기계어를 통해 I/O에 접근
- (위 그림에서 우측) Memory Addresses만
