# 03. 프로세스 관리

> Reference: [이화여자대학교 운영체제 강의 - 반효경](http://www.kocw.net/home/cview.do?cid=4b9cd4c7178db077)

<br/>

## 프로그램의 실행 (Memory load)

![memory_load](/Images/CS03_memory_load.png)

### 원리

- 프로그램은 실행 파일 형태로 저장되어 있다가
- 실행하면 해당 프로그램이 memory에 올라가서 프로세스가 됨
- 이때, 운영체제 Kernel은 이미 memory에 올라가 있음
- 프로그램이 실행될 때 해당 프로그램만의 자신만의 독자적인 Address Space가 → Virtual memory
- 만약 당장 필요한 부분은 물리적인 memory에 올라감
- memory에 올라가 있지 않은 부분은 Swap area로 들어감
- 이 중 일부는 파일 시스템의 파일 형태로 존재

### Address Translation

- Virtual Memory Address (논리적 주소)와 Physical Memory Address (물리적 주소) 간 변환이 필요함

### Virtual Memory

#### stack

- 함수 안에 있는 지역 변수 데이터가 위치
- 함수 호출 및 return과 관련된 정보에 차곡차곡 쌓임

#### data

- 전역 변수 데이터가 위치
- 프로그램이 시작될 때부터 종료될 때까지 있는 값도 위치

#### code

- 실행 파일에 있던 code가 올라오는 것
- CPU에서 수행할 기계어가 위치 → 컴파일된 기계어 코드

### Kernel Address Space

- Kernel도 하나의 프로그램이기 때문에 함수 구조로 되어 있음
- 따라서 동일하게 stack, data, code로 구성되어 있음

<br/>

## Kernel Address 공간의 내용

![kernel_address](/Images/CS03_kernel_address.png)

### 구조

- 하나의 프로그램으로서 함수 구조로 되어 있음
- 프로세스 각각의 Adress Space에는 Code, Data, Stack으로 구성됨

### 운영체제의 Code

> 주로 운영체제가 하는 일이 Kernel Code 안에 들어가 있다고 보면 됨

- System call, Interrupt 처리 코드
- Interrupt가 들어왔을 때 무엇을 처리해야 하는지 들어가 있음
- 효율적인 자원 관리를 위한 코드
- 편리한 서비스 제공을 위한 코드

### 운영체제의 Data

- 현재 실행중인 하드웨어와 모든 프로세스들을 관리하기 위한 자료구조
- 이것을 PCB(프로세스 Control Block) 라고 함

### 운영체제의 Stack

- 지금 누가 실행중인지를 알기 위해 프로세스의 kernel stack은 각각 저장됨
- 각 프로세스마다 별도의 kernel을 두고 있음
- 즉, Kernel이 호출되기 전에 어떤 프로세스에서 실행된 것인지에 따라 구분되어 저장되기 때문
- Kernel의 스택이기 때문에 Kernel 함수와 관련됨

<br/>

## 사용자 프로그램이 사용하는 함수

![function](/Images/CS03_function.png)

### 사용자 정의 함수

> 해당 프로세스 address space 내에 위치

- 내가 직접 만든 함수, 자신의 프로그램에서 정의한 함수
- program counter 값만 바꾸어 다른 위치의 기계어를 실행하면 됨
- 자신의 프로그램 안에 포함되어 있음

### 라이브러리 함수

> 해당 프로세스 address space 내에 위치

- 자신의 프로그램에서 정의하지 않고, 가져다 쓴 함수
- 내가 만든 함수는 아니고 프로그램 안에 집어 넣는 것
- 사용자 정의 함수와 마찬가지로, 자신의 프로그램 안에 포함되어 있음
- 이때는 내 프로그램 안에서 program counter 값만 바꾸어서 다른 위치의 기계어를 실행하는 원리

### 커널 함수

> kernel address space 내에 위치

- 운영체제 프로그램의 함수
- 프로그램 안에 들어가는게 아니라 커널 안에 들어가는 함수
- 실행되다가 디스크에서 파일을 읽어온다든지 하면 I/O는 내가 직접 할 수 있는게 아니기 때문에 사용자 정의/라이브러리 함수가 아닌 커널 함수 실행(system call)해야 함
- virtual memory의 주소 공간을 가로질러 다른 program 의 영역으로 바뀌는 것이기에  CPU 제어권을 OS에 넘긴 후 실행함

<br/>

## 프로그램의 실행

![program_execution](/Images/CS03_program_execution.png)

### User Defined Call & Library Function Call

- user mode (mode bit: 1)
- 내 주소 공간에 있는 code가 user mode로 실행됨

### System Call

- kernel mode (mode bit: 0)
- CPU 제어권이 운영체제에게 넘어가므로 Kernel mode에서 운영체제의 주소공간에 있는 코드가 실행됨

<br/>

## 프로세스의 개념

![프로세스](/Images/CS03_프로세스.png)

### 프로세스란?

- 실행중인 프로그램을 뜻함
- “*프로세스 is a program in execution”*

### 프로세스의 문맥 (context)

#### 문맥이란

- 프로세스의 현재 시점의 상태를 나타내는 것
- 시간에 따라 바뀌는 개념

#### CPU 수행 상태를 나타내는 hardware context

> CPU에서 어디까지 수행했는가?

- program counter 값 → 현재 어디를 실행하고 있는가
- 각종 Register 값 → Register에 어떤 값을 넣고 있었는가

#### 프로세스의 주소 공간

> 자신의 메모리 공간에 무엇을 가지고 있는가?

- code, data, stack

### 프로세스 관련 Kernel 자료 구조

#### PCB(프로세스 Control Block)

- CPU, memory 등을 얼마나 썼는지에 대한 정보를 갖고 있는 자료구조
- PCB를 봐야 context를 알 수 있기 때문에 프로세스를 관리하기 위한 자료구조라 할 수 있음

#### Kernel Stack

- 프로세스마다 다른 kernel stack을 가짐

<br/>

## 프로세스의 상태

> 프로세스는 상태(state)가 변경되며 수행된다

![프로세스_state](/Images/CS03_프로세스_state.png)

### Running

- CPU를 잡고 instruction을 수행중인 상태
- CPU가 하나밖에 없기 때문에 CPU에서 기계어를 실행하는 프로세스는 매순간 하나 → Running 상태에 있다고 함

### Ready

- CPU를 기다리는 상태(메모리 등 다른 조건을 모두 만족하고)

### Blocked(wait, sleep)

- 오래 걸리는 작업 때문에 CPU를 주어도 당장 instruction을 수행할 수 없는 상태
- 프로세스 자신이 요청한 event(e.g. I/O)가 즉시 만족되지 않아, 이를 기다리는 상태
- Inturrupt 되면 진행중이던 event가 완료 되었다는 의미이므로 Ready 상태로 바꿔줌
- e.g. Disc에서 file을 읽어와야 하는 경우

### New

- 프로세스가 생성’중’인 상태

### Terminated

- 수행(execution)이 끝난 상태

### 예시

- 키보드 입력을 기다리는 프로그램에서는 blocked 상태 → 키보드 입력 들어옴 → ready 상태로 바꾸고 → 키보드 입력된 내용을 메모리에 copy 해서 CPU 얻을 수 있게 해줌(CPU 제어권이 운영체제에게 넘어감)

<br/>

## 프로세스의 상태도

![프로세스_state_picture](/Images/CS03_프로세스_state_picture.png)

### new

- 생성 **중**인 상태
- 시작 전이면 프로세스가 아님

### ready

- CPU만 할당되면 바로 실행이 가능한 상태 즉, 온전히 프로세스가 된 상태
- CPU가 필요한 부분은 memory에 올라가 있어야 함

### running

- CPU scheduler가 dispatch(CPU를 넘겨주면)하면 running 상태가 됨
- running 상태가 끝나는 경우
  - Timer interrupt: 할당 시간 만료로 ready queue의 제일 뒤로 가 줄을 서야하는 상황
  - I/O or event wait: 오래 걸리는 작업 때문에 blocked 상태로 들어가는 경우
  - Exit: 종료되는 경우

### waiting(blocked)

- 오래 걸리는 작업이 끝나면 (보통 interrupt가 걸림) 해당 프로세스의 상태를 ready로 바꿔서 다시 CPU가 해당 프로세스를 실행할 준비를 시켜둠

### terminated

- 종료 **중**인 상태
- 이미 종료되었다면 프로세스가 아님

<br/>

## PCB

![pcb](/Images/CS03_pcb.png)

### 정의

- 운영체제가 각 프로세스를 관리하기 위해 두는 자료구조
- 프로세스 당 유지하는 정보

### 구성 요소

- 운영체제가 관리상 사용하는 정보
  - 프로세스 State, 프로세스 ID
  - Scheduling Information, Priority (가장 높은 프로세스에 CPU가 할당됨)
- CPU 수행 관련 hardware 값
  - Context Switching에 대비하여 저장해 놓는 값
  - Program Counter, Registers
- Memory 관련
  - code, data, stack의 위치 정보
- File 관련
  - Open file descriptors …

<br/>

## 문맥 교환 (Context Switching)

![context_switching](/Images/CS03_context_switching.png)

### 정의

- CPU를 한 프로세스에서 다른 프로세스로 넘겨주는 과정

### 필요한 이유

- CPU를 그냥 빼앗아 가는게 아니라, 바로 그 다음 지점을 실행할 수 있도록 현재 프로세스 문맥을 저장해야 함
- 즉, 만약 CPU가 프로세스 A에서 프로세스 B로 넘어가게 하려면, 현재 기계어의 어디 실행중인지 등 정보를 CPU 뺏기 전에 프로세스 A의 PCB에 저장 후, CPU를 넘겨줌

### 동작 과정

> CPU가 다른 프로세스에게 넘어갈 때 운영체제는 아래의 동작을 수행함

- CPU를 내어주는 프로세스의 상태를 해당 프로세스 PCB에 저장
- CPU를 새롭게 얻는 프로세스의 상태를 PCB에서 읽어옴

<br/>

## Context Switch 구분하기

![context_switch_difference](/Images/CS03_context_switch_difference.png)

### context switch인 것

- 사용자 프로세스 A ↔ 사용자 프로세스 B

### context switch가 아닌것

- 사용자 프로세스 A → 운영체제 → (다시) 사용자 프로세스 A

### context switch 여부와 overhead 관계

- user mode에서 kernel mode로 가는건 context switch보다 상대적으로 overhead가 적은 작업
- context switch 없이 CPU 수행 정보 등 context의 일부를 PCB에 save 해야 하지만
- context switch 하는 경우, 그 부담이 훨씬 큼 (캐시 메모리 flush 할 때 overhead가 매우 큼)

<br/>

## 프로세스를 스케줄링 하기 위한 Queue

> 프로세스들은 각 queue를 오가며 수행됨

### job queue

- 현재 시스템 내에 있는 모든 프로세스의 집합

### ready queue

- 그 중 CPU를 당장 얻어야 하는 집합
- 현재 메모리 내에 있으면서, CPU를 잡아 실행되기를 기다리는 프로세스의 집합

### device queue

- 오래 걸리는 작업
- I/O device의 처리를 기다리는 프로세스의 집합

<br/>

## 실제 자료구조 형태

![data_structure](/Images/CS03_data_structure.png)

<br/>

## 프로세스 스케줄링 Queue의 모습

![프로세스_scheduling_queue](/Images/CS03_프로세스_scheduling_queue.png)

- 프로세스가 처음에 실행되면 ready queue
- 본인 차례가 되면 CPU를 얻고 쓰다가 언젠가는 종료
- 쓰다가 오래 기다리는 작업을 요청하면 → I/O queue 가서 줄서고
- 끝나면 다시 ready queue에서 CPU 얻을 권한 생기고
- CPU 계속 쓰고 싶은데 timer interrupt 끝나면
- 다시 ready queue로 감

<br/>

## Scheduler

> 운영체제 안에 들어있는 code의 일부

### Long-Term Scheduler (장기 스케줄러 or Job scheduler)

> 운영체제 내에서 memory scheduling을 하는 역할

#### 역할

- 시작 프로세스 중 어떤 것들을 ready queue로 보낼지 결정
- 프로세스에 memory (및 각종 자원)을 ‘주는’ 문제. 즉, 프로세스가 실행될 때 memory에 올라오도록 하는 역할을 담당
- Degree of Multi-programming(memory에 올라가 있는 program의 수)을 제어

#### 특징

- 일반적인 운영체제 Time Sharing System 에서는 보통 장기 스케줄러가 없음 (무조건 곧바로 ready 상태로 들어감)
- 장기 스케줄러가 없는 대신, 메모리 관리는 어떻게 하나? → 중기 스케줄러로 관리

### Medium-Term Scheduler (중기 스케줄러 or Swapper)

> Time sharing system은 장기 스케줄러를 두지 않고 중기 스케줄러를 둠

#### 역할

- 시작된 프로그램은 다 메모리에 들어오면 → 경합하므로 시스템 성능이 떨어지므로, 여유 공간 마련을 위해 프로세스를 통째로 memory에서 disk로 쫓아냄
- 프로세스에게서 memory 를 ‘뺏는’ 문제 (일단 다 주고, memory가 부족해서 전체 시스템 성능이 떨어지면 memory에서 쫓아냄)
- Degree of Multi-programming 을 제어

### Short-Term Scheduler (단기 스케줄러 or CPU scheduler)

> CPU scheduling을 하는 역할

#### 역할

- 어떤 프로세스를 다음번에 running 시킬지 결정
- 프로세스에 CPU를 '주는' 문제

#### 특징

- 굉장히 자주 호출되므로 충분히 빨라야 함 (millisecond 단위)

<br/>

## 프로세스의 상태

### Suspended (Stopped)

#### 특징

- 중기 스케줄러가 추가되면서 생기는 상태
- 중기 스케줄러 때문에 메모리에서 쫓겨난 상태
- 프로세스는 통째로 disk에 swap out 됨
- 뿐만 아니라 그 외의 외부적인 이유로 프로세스의 수행이 정지된 상태

#### e.g. 외부적인 이유

- 사용자가 프로그램을 일시 정지시킨 경우 (by break key, Linux 환경에서 Ctrl+Z)
- system이 여러 이유로 프로세스를 잠시 중단시킨 경우 (memory에 너무 많은 프로세스가 올라와 경합이 심한 경우) 중기 스케줄러가 여유 공간 마련을 위해 프로세스를 통째로 memory에서 disk로 쫓아냄

### Blocked와 Suspended의 차이

#### 공통점

- CPU를 얻을 수 없는 상태

#### 주요 차이점

- Blocked 상태
  - 프로세스가 여전히 메모리에 남아 있고
  - 이벤트를 기다리며 CPU를 얻을 수 있는 상태
- Suspended 상태
  - 프로세스가 일시적으로 메모리에서 제거되고
  - 외부에서 활성화될 때까지 아무 작업도 수행하지 않는 상태

#### Blocked

- 프로세스가 I/O 작업 또는 다른 이벤트(e.g. 외부 자원 요청)를 기다리는 동안 CPU를 얻을 수 없는 상태
- 프로세스가 스스로 해당 이벤트가 발생할 때까지 (스스로) 기다리는 상태
- 이벤트가 발생하면 프로세스는 준비 상태(Ready)로 전환되어 CPU를 다시 얻을 수 있게 됨

#### Suspended

- 프로세스가 일시적으로 중지되거나 정지된 상태
- 일을 아예 못하는 정지된 상태
- 정지 상태는 일반적으로 외부에서 프로세스를 다시 활성화하거나 재개(resume)해야만 함
- 주로 메모리에서 프로세스의 상태를 제거하고 나중에 필요할 때 다시 메모리로 로드할 때 사용

<br/>

## 프로세스 상태도

> Suspended를 포함한 상태도

![프로세스_status_active_inactive](/Images/CS03_프로세스_status_active_inactive.png)

### Active / Inactive

#### Active

- Running
- Ready
- Blocked

#### Inactive

- Suspended Blocked
- Suspended Ready

### Swap Out / Swap In

> 운영 체제에서 메모리 관리를 위해 사용되며, 메모리 부족 상황에서 프로세스들을 관리하는 데 중요한 역할을 함

#### Swap out

- 프로세스나 프로그램이 현재 메모리(RAM)에 적재되어 있을 때
- 시스템이 메모리 공간을 확보하기 위해 해당 프로세스나 프로그램을 메모리에서 "통째로 쫓겨나게" 하는 작업을 의미
- 이때, 해당 프로세스의 데이터와 코드가 디스크의 Swap File 또는 Swap Area에 저장됨

#### Swap in

- Swap out된 프로세스나 프로그램이 다시 실행되어야 할 때
- 시스템이 스왑 파일 또는 스왑 영역에 저장된 해당 프로세스의 데이터와 코드를 메모리로 다시 "올리는 작업"을 의미
- 이로써 해당 프로세스는 메모리에서 다시 실행 가능한 상태가 됨

### Suspended(Blocked) / Suspended(Ready)

#### 공통점

- 메모리를 통째로 빼앗겨서 없는 상태
- 메모리를 통째로 빼앗겼으니 CPU 작업은 할 수 없지만, I/O 작업은 일부 진행 가능
- 외부에서 다시 메모리를 할당해야 active 상태가 됨

#### Suspended(Blocked)

- Suspended 상태로 진입하기 전에 I/O 작업을 하던 프로세스
- I/O 작업이 일시 중단되고 "Suspended Blocked" 상태로 진입하며
- I/O 작업이 완료되면 "Suspended Ready" 상태로 돌아갈 수 있음

#### Suspended(Ready)

- 완료된 상태이며 I/O 작업을 하고 있지 않다는 의미

### Running (User mode / Kernel mode)

![프로세스_status_running](/Images/CS03_프로세스_status_running.png)

#### 중요한 개념

- 여기서 말하는 상태는 운영체제가 사용자 프로그램을 관리하기 위해 존재하는 상태지, 운영체제 본인의 상태를 의미하는 것은 아님
- Running 상태는 현재 CPU를 사용하고 있는 프로세스 또는 운영 체제 Kernel mode 코드를 나타냄
- Interrupt나 System Call로 인해 프로세스가 일시 중단되는 경우, CPU는 다른 작업을 수행

#### User mode에서 Running 상태인 경우

- 프로세스 본인의 코드가 실행 중인 상태
- CPU를 사용하여 해당 프로세스의 명령을 수행함

#### Kernel mode에서 Running 상태인 경우

- 운영 체제의 커널 코드가 실행 중인 상태
- 주로 System Call이나 Interrupt 처리와 같은 운영 체제의 핵심 기능을 수행할 때 발생

#### User mode에서 프로세스가 System Call을 호출한 경우

- 해당 System Call이 kernel mode에서 실행됨
- 해당 program은 CPU를 빼앗긴 것이 아니므로 CPU를 사용중인 것으로 간주되지만
- Kernel mode에서 실행되는 System Call은 프로세스의 요청을 처리하고 다시 User mode로 돌아감

#### Interrupt가 들어오는 경우

- Interrupt는 프로세스가 실행 중일 때 다른 이벤트(예: I/O 완료, 하드웨어 이벤트)가 발생했을 때 발생
- 이 경우, 현재 실행 중인 프로세스가 Blocked 상태로 전환되며, 이후 운영 체제는 해당 Interrupt를 처리하기 위해 Kernel mode에서 실행됨
- 이렇게 되면 현재 실행 중인 프로세스는 일시 중단되었지만, CPU는 여전히 사용 중이며 다른 이유로 Kernel mode에서 running 상태라고 간주함

<br/>

## Thread

> A thread (or lightweight 프로세스) is a basic unit of CPU utilization : CPU 수행 단위

### 설명

- 프로세스에서 CPU 수행에 실행에 필요한 부분만 별도로 가지고 있는 것이 Thread
- 프로세스는 공유하되, thread는 작업에 따라 각자 가짐

### Thread가 동료 thread와 공유하는 부분 (=task)

> 전통적인 개념의 heavyweight 프로세스는 하나의 thread를 가지고 있는 task로 볼 수 있음

- code section
- data section
- OS resources

### Thread의 구성

<p align="center" width="100%"><img width="1010" alt="thread-composition" src="https://github.com/ella-yschoi/TIL/assets/123397411/438efd77-c002-4392-840a-7b4439fd468c">

- program counter
- register set
- stack space
  - 주소 공간에서는 thread가 함수 호출과 관련된 stack만을 가짐
  - 그 외에는 프로세스 내에서 다른 thread들과 공유

### Thread의 효율성

<p align="center" width="100%"><img width="1010" alt="thread-efficiency" src="https://github.com/ella-yschoi/TIL/assets/123397411/f68d530d-c37f-4d00-91b7-6beb3ff499a9">

- 프로세스에서 스레드로 CPU 수행 관련 부분만 별도로 가지고 있고, 그 외는 단일 프로세스로 관리하는 것이 효율적임.
- 즉, 스레드를 사용함으로써 프로세스 간 통신 및 데이터 공유가 더 효율적으로 이루어질 수 있다는 것을 의미함. 여러 스레드가 같은 프로세스 내에서 작동하며, 프로세스의 메모리 및 자원을 공유하므로 커뮤니케이션 비용이 낮아짐.
- context switch는 overhead가 큰 편이지만, thread에서 thread로 넘어가는 것은 효율적임
- 웹 브라우저를 여러 개 띄우면 여러 프로세스가 뜨니 비효율적이므로 스레드를 만드는게 효율적
- 다만, 크롬 브라우저는 보안 상의 이슈가 있어 여러 개의 프로세스를 띄우며 (Multi-프로세스 Architecture) 브라우저마다 구현 방식은 다를 수 있음
  - [Chrome for Developers - Inside look at modern web browser (part 1)](https://developer.chrome.com/blog/inside-browser-part1/)
  - [[위 블로그 한국어 번역] 크롬 브라우저는 어떻게 작동 할까? - 01](https://onlydev.tistory.com/80)
  - [The Chromium Projects - Multi-프로세스 Architecture](https://www.chromium.org/developers/design-documents/multi-프로세스-architecture/)

### Thread의 장점

<p align="center" width="100%"><img width="1010" alt="thred-pros" src="https://github.com/ella-yschoi/TIL/assets/123397411/67207b0b-1b79-48cc-81ad-421bd23cf6c1">

#### Responsiveness (빠른 응답성)

- 하나의 스레드가 네트워크를 통해 읽어오는 동안, 또다른 스레드가 화면에 보여주면 사용자에게 빠른 응답 제공 가능

#### Resource Sharing(자원 공유)

- 동일 프로세스 내에 있는 스레드들끼리는 CPU 수행 정보를 제외한 대부분의 것들을 공유할 수 있기에 효율적인 자원 공유가 가능
- 스레드는 같은 프로세스 내에서 실행되므로 프로세스의 code, data, resource를 공유할 수 있음.
- 이는 자원 공유를 간편하게 만들며, 데이터를 복사하는 등의 overhead를 줄여줌.
- 즉, 여러 개의 thread는 프로세스의 binary code, data, resource를 공유 가능

#### Economy(경제적)

- 똑같은 일을 프로세스 여러 개 or 프로세스 안에 스레드를 여러 개 두는 것의 차이
- creating & CPU switching thread (rather than a 프로세스)
- solaris(Oracle가 개발한 다중 스레드 모델)의 경우, 위 두 가지 overhead가 각각 30배, 5배

#### Utilization of Multi프로세스or Architectures(병렬성)

- Multi프로세스or(CPU가 여러 개 있는 상황)에서는 병렬성 추구 가능
- 예를 들어 만약, 큰 행렬을 곱셈을 한다 했을 때, CPU가 하나 있으면 한 열씩 순차적으로 해야 하는 상황. CPU가 여러 개 있다면 행렬의 곱셈을 여러 스레드로 나눠서 연산 후, 서로 다른 CPU에 배치하며 연산을 병렬적으로 작업을 수행함 → 효율적

### 다중 thread 구성의 장점

- 다중 스레드로 구성된 태스크 구조에서는 하나의 서버 스레드가 blocked(waiting) 상태인 동안에도, 동일한 태스크 내의 다른 스레드가 실행(running)되어 빠른 처리를 할 수 있음
- 동일한 일을 수행하는 다중 스레드가 협력하여 높은 처리율(throughput)과 성능 향상을 얻을 수 있음. 따라서 스레드 사용 시, 병렬성을 높일 수 있음
- 예를 들어 만약, HTML 문서를 통해 웹 브라우저에서 당장 보여줄 수 있는 것만이라도 보여주어야 하는 상황인데, 이미지 url을 받아오는 등 오래 걸리는 작업으로 인해 시간이 오래 걸린다면, 하나의 스레드가 읽어오는 동안 또 다른 스레드가 당장 표현할 수 있는 텍스트라도 빠르게 보여줄 수 있음

### Thread 구현 방법

<p align="center" width="100%"><img width="1010" alt="thread-supported" src="https://github.com/ella-yschoi/TIL/assets/123397411/ec3b56b7-9c1f-4d3b-92ed-e95878431c18">

#### Kernel Threads

> supported by kernel

- 운영체제가 이미 스레드의 존재를 알게 구현하는 것
- 운영체제 커널이 직접 지원해주는 스레드를 사용
- 운영체제가 서로 다른 스레드 간의 CPU 전환을 할 수 있음 (마치 프로세스간의 CPU 스케줄링 처럼)

#### User Threads

> supported by library

- 사용자 프로그램 단에서 스레드를 관리
- 운영체제가 이미 스레드의 존재를 모르게 구현하는 것
  - 운영체제가 볼 때는 그저 스레드가 없는 프로세스처럼 보임
- 운영체제가 thread 간의 CPU를 넘기는 작업을 할 수 없음
  - 프로세스 내부에서 운영체제에 비동기식 I/O를 요청해서 바로 다시 받아 다른 thread로 CPU를 넘기는 등 사용자 프로그램단에서 관리하는 것을 의미

<br/>

## 프로세스 관리

> 프로세스가 어떻게 만들어지고 종료되는가

### 프로세스의 생성

#### 생성 원리

- 부모 프로세스(parent 프로세스)가 자식 프로세스(children 프로세스)를 만들면 복제 생성 (프로세스가 또다른 프로세스를 생성)
- 부모와 똑같은 나이를 가진 프로세스를 생성
- 즉, 본인이 직접 만드는 것이 아니라, 운영체제에게 system call 요청을 통해 생성 (fork system call)
- 프로세스의 트리(계층 구조) 형성

#### 프로세스는 자원을 필요로 함

- 운영체제로부터 받음
- 부모와 공유 (사실 부모와 자식 프로세스는 별개의 프로세스이므로 자원을 할당받기 위해 경쟁)

#### 자원의 공유

- 부모와 자식이 모든 자원을 공유하는 모델
- 일부를 공유하는 모델
- 전혀 공유하지 않는 모델

#### 수행 (Execution)

- 부모와 자식은 공존하며 수행되는 모델
- 자식이 종료(terminate)될 때까지 부모가 기다리는 모델 (blocked 상태)

#### 주소 공간 (Address Space)

- 자식은 부모의 공간을 복사 (binary and OS data)
- 자식은 그 공간에 새로운 프로그램을 올림
- 즉, 부모의 것을 그대로 복사하여 코드도 복사되지만, 데이터와 스택도 복사됨 → 전역 변수 등도 그대로 가져가고, 스택도 카피한다는 것은 현재 수행 위치에서부터 자식이 실행된다는 의미

#### 여러 프로그램들을 돌린다?

- 일단 복제(fork system call)해놓고, 거기에 다른 프로그램을 덮어 씌워서(exec system call) 새로운 프로그램을 돌린다는 의미

#### Unix(유닉스)의 예

- fork() system call이 새로운 프로세스를 생성
  - 부모를 그대로 복사 (OS data except PID + binary)
  - 주소 공간 할당
- fork 다음에 이어지는 exec() system call을 통해 새로운 프로그램을 메모리에 올림

### 프로세스 종료

- 프로세스가 마지막 명령을 수행한 후, 운영체제에게 이를 알려줌 (exit)
  - exit system call → 운영체제가 프로세스를 종료하면서 모든 자원을 반납하고 종료
  - 자식이 부모에게 output data를 보냄 (via wait)
  - 프로세스의 각종 자원들이 운영체제에게 반납됨
- 부모 프로세스가 자식의 수행을 강제 종료시키는 경우 (abort)
  - 자식이 할당 자원의 한계치를 넘어설 때
  - 자식에게 할당된 태스크가 더이상 필요하지 않을 때
  - 부모가 종료(exit)하는 경우
    - 운영체제는 부모 프로세스가 종료하는 경우, 부모가 죽기 전에 자식을 먼저 죽임
    - 단계적 종료 (계층 구조의 제일 아래 자손부터 순차적으로 죽임)
- 보통은 자식이 먼저 종료 → 부모가 뒷 일을 함 (exit)
  - 자식이 부모에게 output data를 보냄
  - 프로세스의 각종 자원들이 운영체제에게 반납됨
- 브라우저 창을 끄면 실행되던 것들이 전부 종료되는 것

### fork() system call

- 프로세스는 fork() system call에 의해 생성됨
  - 운영체제에게 자식을 만들어 달라는 함수
  - caller를 복제해 새로운 address space를 생성

### Parent 프로세스와 Child 프로세스 구분

<p align="center" width="100%"><img width="1010" alt="프로세스-end" src="https://github.com/ella-yschoi/TIL/assets/123397411/ecc4b973-846b-4411-b202-723daf57324f">

- 부모: PID 값이 0보다 큰 return value로 받게 됨
- 자식: PID 값을 0으로 return value로 받게 됨

### exec() system call

<p align="center" width="100%"><img width="1010" alt="exec-systemcall-1" src="https://github.com/ella-yschoi/TIL/assets/123397411/1dabcb46-8ca2-4b55-a017-0af035443df1">

- hello 출력 후, 새로운 프로그램으로 덮어 씌움

<p align="center" width="100%"><img width="1010" alt="exec-systemcall-2" src="https://github.com/ella-yschoi/TIL/assets/123397411/de7761d1-06e4-4bab-a7bb-0ca99b3127c0">

- 자식 프로세스를 생성하여 다른 프로그램을 돌리고 싶을 때 (자식에게 새로운 프로그램을 덮어 씌울 때)
  - 부모는 그냥 덮어 씌우는게 아니라 fork() 실행해 복제 후
  - 부모 자신은 원래 실행하던 프로그램 이어서 실행하고
  - 자식에게는 exec() 실행해 기존 프로그램 말고 다른 새로운 프로그램을 돌리게 함

### wait() system call

<p align="center" width="100%"><img width="1010" alt="wait-systemcall" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/20e424e8-8b68-49ab-9836-b1042bf670d3">

- 프로세스 A가 wait() system call을 호출하면
  - 커널은 자식 프로세스가 종료될 때까지 프로세스 A를 sleep시킨다 → blocked 상태
  - 자식 프로세스가 종료되면 부모가 CPU를 얻고, 커널은 프로세스 A를 깨운다 → ready 상태
- Linux에서는 command를 입력할 때, 기본적으로 입력받을 수 있는 한 줄이 하나의 프로세스 (Shell)
  - command 하나가 종료되어야 또다른 command를 입력 가능
  - command 하나가 실행시키면 자식 프로세스의 형태로 실행되는 것
- 정리
  - 부모 프로세스는 자식 프로세스가 종료될 때 까지 blocked 상태로 기다리다가
  - 자식이 종료되면 다시 깨어나서 일을 할 수 있음

### exit() system call

#### 자발적 종료

- 마지막 statement 수행 후, exit() 시스템 콜을 통해 종료
- 프로그램에 명시적으로 적어주지 않아도 main 함수가 리턴되는 위치에 컴파일러가 넣어줌

#### 비자발적 종료

- 부모 프로세스가 자식 프로세스를 강제 종료시킴
  - 자식 프로세스가 한계치를 넘어서는 자원 요청
  - 자식에게 할당된 태스크가 더이상 필요하지 않음
- 키보드로 kill, break를 입력한 경우
- 부모가 종료하는 경우
  - 부모 프로세스가 종료하기 전에 자식들이 먼저 종료됨

<br/>

## 프로세스간 협력

### 독립적 프로세스(Independent 프로세스)

- 프로세스는 각자의 주소 공간을 가지고 수행되므로 원칙적으로는 하나의 프로세스는 다른 프로세스의 수행에 영향을 미치지 못함
- 프로세스는 항상 독자적으로 작동됨

### 협력 프로세스(Cooperating 프로세스)

- 프로세스 협력 메커니즘을 통해 하나의 프로세스가 다른 프로세스의 수행에 영향을 미칠 수 있음
- 경우에 따라서는 프로세스간 협력이 필요함
- 다른 프로세스의 메모리를 볼 수 없고, 상대 프로세스와 무언가를 주고받지는 못하기에, 아래와 같이 IPC에 따라 정보를 주고받으며 협력함

<br/>

## 프로세스 간 협력 메커니즘 (IPC: Inter프로세스 Communication)

<p align="center" width="100%"><img width="1010" alt="IPC" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/f9fa2ddb-8fcd-4c6d-909a-d8430741ff7f">

### (1) Message Passing (메시지 전달 방법)

커널을 통해 메시지를 전달하며, 운영체제 커널에 메시지를 전달해서 다른 프로세스가 커널로부터 전달을 받는 방법 사용

#### Message System

프로세스 사이에 공유 변수(shared variable)를 일체 사용하지 않고 통신하는 시스템

#### Direct Communication

통신하려는 프로세스의 이름을 명시적으로 표시하며, 메시지를 누구한테 보낼지를 명시하여 양자간에 합의된 명시적 메시지 전달 방식

<p align="center" width="100%"><img width="1010" alt="Direct-Communication" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/7c8374c7-dc57-46ef-863a-e5002d71ccc2">

#### Indirect Communication

mailbox (or port)를 통해 메시지를 간접 전달하며, 타겟을 명시하지 않고 전달하면 협력하는 프로세스 중 하나가 꺼내가도록 하는 방식

<p align="center" width="100%"><img width="1010" alt="Indirect-Communication" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/d1c4201a-1410-4d5f-963f-e3079cfacd80">

### (2) Shared Memory (주소 공간을 공유하는 방법)

- 서로 다른 프로세스 간에도 일부 주소 공간을 공유하게 하는 shraed memory 메커니즘이 있음
- 서로 공유 운영체제에게 system call을 통해 부탁해 메모리 일부를 공유하겠다고 함(메모리를 공유한다는 것은 서로 협력이 가능하다는 뜻)
- 단, 서로 신뢰할 수 있을 때만 공유해야 함
- 참고) Thread
  - 스레드는 사실상 하나의 프로세스이므로 프로세스간 협력으로 보기에는 어렵지만, 동일한 프로세스를 구성하는 스레드들간에는 주소 공간을 공유하므로 협력이 가능
