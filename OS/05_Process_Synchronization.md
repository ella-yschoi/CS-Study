# 병행 제어

## 1. 컴퓨터의 연산

<p align="left" width="100%"><img width="1000" alt="컴퓨터의 연산" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/42000663-94c1-4bc0-b8b7-75f6e1840946">

- 컴퓨터에서 연산을 할 때는 항상 데이터를 읽어와서 연산 → 그 결과를 다시 어딘가에 저장하도록 되어 있음
- 메모리에 있는 데이터를 읽어와서 CPU에서 연산을 하면 다시 데이터를 가져다가 메모리에서 사용하는 원리
- 연산을 할 때는 그 자리에서 하는 게 아니라, 뭔가를 불러들여와서 진행하고, 마치면 밖에다가 다시 내보내는 원리

<br/>

## 2. Race Condition

<p align="left" width="100%"><img width="1000" alt="Race Condition이란" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/1820b341-e4e6-4387-93a4-5d9432b601d8">

### (1) 문제의 발생

- 보통 데이터를 한 군데가 아니라, 여러 군데에서 읽어갈 때 발생함
- 예를 들어, count 변수 값이 왼쪽에서는 증가, 오른쪽에서는 감소 시킨다고 했을 때 이 count 값은 원래 값이 저장되어 있어야 함.
- 그런데 이렇게 연산하는 주체가 두 개가 있으면, 한 쪽에서 연산하는 “도중에” 데이터를 읽어갔을 때 문제가 발생할 수도 있음
- 하나의 공유 데이터를 여러 개가 동시에 접근하려고 할 때의 상태 → Race Condition (경쟁 상태)

### (2) 정말 생길까?

- CPU가 여러 개 있으면 Race Condition 발생 가능하나, 꼭 생기는건 아님
- 물론 CPU가 하나 있어도 Race Condition이 발생하지 않는다고 볼 수 없음

### (3) 문제가 일어나는 과정

> CPU가 하나 있더라도 운영체제가 끼어드는 경우

- 예를 들어, A 라는 프로그램이 실행중인데 이때 프로세스는 본인이 직접 할 수 없는 일을 운영체제에게 대신 해달라고 요청 → system call
- 프로세스 A가 CPU를 잡고 일하다가 → 내가 할 수 없는 일이라 판단해 system call → 이때 A는 운영체제 안의 데이터 값을 바꾸고 있던 상황 → 그런데 자신의 CPU 할당 시간이 끝나버림
- CPU가 A에서 프로세스 B에게 넘어감 → B가 CPU를 잡고 일하고 있는데 → B도 본인이 할 수 없는 일을 하게 되어 운영체제에게 system call → 이번에는 B의 요청으로 또다시 운영체제의 코드가 실행됨
- 그런데 운영체제 데이터에서 A가 건드리던 데이터를 또 똑같이 건드리게 됨 →운영체제는 하나니까 A가 요청하든 B가 요청하든 동일한 데이터를 건드릴 수 있는 것
- 아까 A가 운영체제의 어떤 데이터를 읽어서 변경하려는 ‘찰나에’ CPU가 B에게 넘어감 → B의 요청으로 운영체제가 변수를 변경함 → 나중에 CPU가 A에게 다시 넘어오면 → 아까 system call 했을 때 ‘하다 말았던’ 일을 계속 해야하는데 이미 이 데이터를 읽어 와버림 → 분명 CPU가 한 개였고 프로세스들끼리 데이터를 공유하지도 않았는데 운영체제 안의 데이터를 건드리면서 문제가 발생

### (4) 정리: OS에서 Race Condition가 발생하는 경우

1. kernel 수행 중 인터럽트 발생 시
2. 프로세스가 system call을 해 커널 모드로 수행중인데, context switch가 일어나는 경우
3. 멀티 프로세서에서 shared memory 내의 커널 데이터를 건드리는 경우

<br/>

## 3. OS에서의 Race Condition

### (1) 문제의 발생

<p align="left" width="100%"><img width="1000" alt="Race Condition" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/a0f9ae90-96b3-483b-9bff-03a4ae465d4f">

- 프로그램 A가 user mode에서 본인의 코드를 실행할 때는 어차피 본인의 주소 공간의 데이터에 접근할테니 문제가 없음
- 그런데 system call을 통해 커널의 코드가 실행될 때는 커널의 데이터를 건드리는데, 커널의 데이터는 프로세스 입장에서는 일종의 **공유 데이터**라고 볼 수 있음
- 따라서 프로그램이 user mode, kernel mode 반복하면 user mode에서는 주소 공간을 공유하지 않기에 문제가 되지 않지만, kernel mode에서 kernel의 데이터를 건드리는 도중에 CPU를 빼앗기게 되면 Race Condition이 발생한다.

### (2) 해결책

<p align="left" width="100%"><img width="1000" alt="Race Condition 해결책" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/58de6d59-3832-4795-941b-eb0bdd631d7c">

- kernel mode에서 ‘수행 중’일 때는 CPU를 preempt 하지 않음 (빼앗지 않음)
- kernel mode에서 user mode로 돌아갈 ‘때’ preempt 하기

### (3) OS에서의 Race Condition (interrupt handler vs. kernel)

<p align="left" width="100%"><img width="1000" alt="OS에서의 Race Condition (interrupt handler vs. kernel)" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/b09ed6ff-37d5-42a5-821b-105ac1ea6e36">

- interrupt가 들어오면 현재의 문맥을 처리하고 → 그 다음에 interrupt 처리 루틴으로 돌아가서 그 작업을 먼저 하고 → 다시 돌아오기
- 결국 변수를 건드리기 전에 (real time system이 아닌 이상) interrupt를 disable 시키고 → 작업이 다 끝나면 enable 시키면서 해결하면 됨
- 결국 프로세스들 간에는 주소 공간을 공유하지 않는데 왜 race condition이 생기나? 운영체제 안에 들어가서 작업할 때 이러한 상황이 생긴다.

### (4) OS에서의 Race Condition (multi-processor)

<p align="left" width="100%"><img width="1000" alt="OS에서의 Race Condition (multi-processor)" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/51aff8e3-47f0-482c-acd1-457f788f0d8b">

#### a. 방법 1

- 운영체제 전체를 lock 걸어서 혼자만 독점
- 즉, 한번에 하나의 CPU만이 커널로 들어갈 수 있게 하는 방법
- 다른 것은 못 쓰게 하고 커널 전체가 lock으로 묶이는 것

#### b. 방법 2

- 운영 체제 안의 데이터별로 lock을 거는 것
- 해당 데이터를 건드리지만 않는다면 동시에 수행 가능
- 즉, 커널 내부에 있는 각 공유 데이터에 접근할 때마다 해당 데이터에 대한 lock/unlock을 하는 방법

<br/>

## 4. Process Synchronization 문제

> 공유 데이터(shared data)의 동시 접근(concurrent access)은 데이터의 불일치 문제(inconsistency)를 발생시킬 수 있다.

### (1) Race condition

- 여러 프로세스들이 동시에 공유 데이터를 접근하는 상황
- 데이터의 최종 연산 결과는 마지막에 그 데이터를 다룬 프로세스에 따라 달라짐
- 예를 들어 process A: count ++와 process B: count - -가 있다고 치자. process A에서 B를 거치면 (하나 더하고 하나 뺐으면) 원래대로 0이 되어야 하는데 왜 데이터가 불일치되는가? 에 대한 문제인 것.
- 따라서 race condition을 막기 위해서는 concurrent process는 동기화되어야 한다.

### (2) 해결책

- 일관성(consistency) 유지를 위해서는 협력 프로세스(cooperating process) 간의 실행 순서(orderly execution)를 정해주는 메커니즘 필요
- 데이터를 온전히 변경한 후 → 데이터를 넘기는 것 (A → B or B → A 이런 식으로)
- 협력 시 실행 순서를 정해주어서 ‘도중에’ CPU를 빼앗기는 문제를 막기 위함

<br/>

## 5. Example of a Race Condition

<p align="left" width="100%"><img width="1000" alt="Example of a Race Condition" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/0de20292-283c-4ea5-b701-dc9fee819b8e">

- 사용자 프로세서 P1 수행 중, timer interrupt가 발생해서 context switch가 일어나서 P2가 CPU를 잡으면?
- 이 작업을 하는 ‘도중에’ 쪼개져서 CPU가 다른 프로세스에게 넘어가게 되는 게 문제임
- 공유데이터를 건드리는 코드 → critical section

<br/>

## 6. The Critical-Section Problem

<p align="left" width="100%"><img width="1000" alt="The Critical-Section Problem" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/61854ede-f797-4ee6-89b0-bde9acafdf43">

- n개의 프로세스가 공유 데이터를 동시에 사용하기를 원하는 경우
- 각 프로세스의 code segment에는 공유 데이터를 접근하는 코드인 critical section이 존재 (공유 데이터가 critical section이 아니라, 공유데이터를 각각의 프로세스가 접근하는 코드)
- problem: 하나의 프로세스가 critical section에 있을 때 다른 모든 프로세스는 critical section에 들어갈 수 없어야 한다. 즉, critical section 들어가기 전에 lock을 걸어서 접근을 막고 lock을 풀어주는 형태

<br/>

## 7. Attempts to solve problem

### (1) Initial Attempts to solve problem

<p align="left" width="100%"><img width="1000" alt="Initial Attempts to solve problem" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/b8fe2787-ee3b-42bf-889c-f99b4b15402f">

critical section에 동시 접속 시 문제가 생기기에 들어갈 때의 코드(entry section)와 빠져갈 때 코드(exit section)를 추가해서 동시 접속을 막는 방법이 있음

### (2) 프로그램적 해결법의 충족 조건

> 가정 1. 모든 프로세스의 수행 속도는 0보다 크다.<br/>
> 가정 2. 프로세스들 간의 상대적인 수행 속도는 가정하지 않는다.

#### a. Mutual Exclusion (상호 배제)

- 동시에 들어가면 안된다는 조건
- 프로세스 Pi가 critical section 부분을 수행 중이면 다른 모든 프로세스들은 그들의 critical section에 들어가면 안 된다.

#### b. Progress (진행)

- 들어가고 싶은데 못 들어가는 조건
- 아무도 critical section에 있지 않은 상태에서 critical section에 들어가고자 하는 프로세스가 있다면 critical section에 들어가게 해주어야 한다.

#### c. Bounded Waiting (유한 대기)

- 기다리는 시간이 유한해야 하는 조건 (starvation 막아야)
- 프로세스가 critical section에 들어가려고 요청한 후부터 그 요청이 허용될 때까지 다른 프로세스들이 critical section에 들어가는 횟수에 한계가 있어야 한다.

### (3) 동시 접속을 막기 위한 알고리즘 1

<p align="left" width="100%"><img width="1000" alt="동시 접속을 막기 위한 알고리즘 1" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/0987a3a2-c218-4245-85e4-1c56bff58db6">

- 만약 지금이 내 차례가 아니라면 while 문에서 계속 기다리다가 → 만약 turn이 0이면(내 차례면) critical section에 들어감 → 그리고 다 쓰고 나갈 때 turn을 1로 바꾸어 준다.
- 즉, critical section에 들어가기 전에 이번이 내 차례인지 체크해서 → 내 차례가 아니면 기다리고, 맞으면 critical section에 들어가서 공유코드를 실행하고 → 끝나면 상대방 들어갈 수 있게 상대방 코드로 바꾸어 줌
- 제대로 돌아갈까? 일단 둘이 동시에 critical section에 들어가는 경우는 생기지 않을 것
- 하지만 엄격하게 critical section에 번갈아 들어가도록 하게 되어 있으므로 들어가고 싶어도 못 들어가게 되어 있음
- 즉, 동시에 들어가는 문제는 해결되지만, 나는 여러 번 들어가고 싶고 상대방은 들어가고 싶지 않다고 가정한다면 나는 여러 번 들어가지를 못하는 문제 발생 → 과잉 양보 문제

### (4) 동시 접속을 막기 위한 알고리즘 2

<p align="left" width="100%"><img width="1000" alt="동시 접속을 막기 위한 알고리즘 2" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/03ad06e8-9ddc-4a73-81f8-b5e87273f724">

- ⛳ flag를 둔다는 알고리즘: 프로세스마다 각자 자신만의 깃발이 있기에 critical section에 들어가고 싶으면 자신의 깃발을 든다.
- 한 프로세스가 critical section에 들어가고 싶다는 의사를 표시: flag[i] = true; → 상대방의 깃발이 내려져 있는지 확인 → 들려져 있다면 안 들어감 → 내려져 있다면 critical section에 들어가기 → 다 썼다면 깃발을 내려서 상대방을 들어오게 하기: flag[i] = false;
- 일단 동시에 들어가는 문제는 해결이 되나, 모두 다 깃발을 들어 놓고 눈치만 보다가 들어가지 못하는 상황이 발생하기에 위에서 언급한 2. Progress (진행) 상황 발생

### (5) 동시 접속을 막기 위한 알고리즘 3

<p align="left" width="100%"><img width="1000" alt="동시 접속을 막기 위한 알고리즘 3" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/6c633233-3e53-4d15-b02a-4a17248f8c44">

- 앞의 2개 변수를 모두 사용함 (깃발 들기 & 깃발을 확인하는 절차)
- 추가로, 동시에 깃발을 들었다면 turn을 이용해서 니 차례냐 내 차례냐를 정하는 것
- critical section에 들어가기 전 의사표현 flag[i] = true;→ turn을 상대방 turn으로 만들어 둠: turn = j; → 그 다음에 상대방 깃발이 들려있는지 && turn이 상대방꺼인지 확인: while문; → 다 쓰고 나올 때는 상대방이 쓸 수 있게 깃발을 내려주고 감 flag[i] = false;
- 하지만 여전히 문제가 있음: busy waiting ( = spin lock) 비효율적임. 만약 내가 while문에 계속 걸려서 critical section에 들어가지 못하는 상황이라면 CPU와 메모리를 계속 쓰면서 기다리고 있는 상황

### (6) Synchronization Hardware

<p align="left" width="100%"><img width="1000" alt="Synchronization Hardware" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/e579ad52-894b-4605-b3d7-b544ff9d837c">

- 하드웨어적으로 원자적으로 즉, 중간에 CPU를 빼앗기거나 쪼개지지 않고 데이터를 바꾸고 저장하는 것을 반드시 ‘한꺼번에’ 실행하도록 함 → Test and Set이 지원되면 해결 가능 (1. Read + 2. TRUE 로 전환)
- a 라는 변수를 읽어가고 + 읽힌 a 라는 변수를 1로 만들고 ⇒ 이 과정을 atomic하게 한번에 만드는 과정이 지원 되면 위에서 언급한 복잡한 알고리즘들 필요 없이 가능

<br/>

## 8. Semaphores

### (1) Semaphore란?

> Semaphore 개념을 쉽게 설명하는 글: [뮤텍스(Mutex)와 세마포어(Semaphore)의 차이](https://worthpreading.tistory.com/90)

<p align="left" width="100%"><img width="1000" alt="Semaphore" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/87875110-ad0b-4598-8eb4-f25117e214e0">

- 일종의 추상 자료형이며 앞의 방식들을 추상화시킴
- 사용 목적: 자원은 공유자원이기 때문에 동시에 접근하면 문제를 생김. 이를 막기 위해 semaphore를 쓰면서 자원의 획득과 반납을 관리하는 것임
- 변수 값 S는 정수(jnteger)로 정의되며, atomic한 P 연산과 V 연산 두 가지로 표현됨

#### P 연산

- 자원을 획득하는 과정 (== lock을 거는 과정)
- 자원을 카운팅하는 변수 S가 0 이하의 값이면 (없으면) while문에서 기다리고, 여분이 있다면 - - 시켜서 자원을 쓰는 코드 수행

#### V 연산

- 자원을 반납하는 과정 (== lock을 푸는 과정)
- S ++로 자원을 내놓았다는 의미

### (2) Critical Section of n Processes

<p align="left" width="100%"><img width="1000" alt="Critical Section of n Processes" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/321a1661-4369-429e-ab7c-b8c8c334c764">

- critical section 들어가기 전에 P 연산 → 빠져 나올 때는 V 연산을 해서 다시 값을 복원시켜준다.
- 참고로 mutex란, ‘상호 배제’ 라는 뜻으로 동시 프로그래밍에서 공유 불가능한 자원의 동시 사용을 피하기 위해 사용되는 알고리즘이다. 임계 구역(critical section)으로 불리는 코드 영역에 의해 구현된다. ([출처: 위키백과](https://ko.wikipedia.org/wiki/%EC%83%81%ED%98%B8_%EB%B0%B0%EC%A0%9C))

### (3) Semaphore를 정의하는 방법

- P 연산을 본인이 계속 해봐야 0인 상태에서 while문을 쓸데없이 도는 문제 → busy waiting
- 만약 누군가가 critical section에 들어가서 이미 semaphore 변수를 체크하고 있으면 CPU를 아예 반납해서 blocked → 누군가가 semaphore V 연산을 해줄 때까지 CPU를 얻지 않는게 더 효율적임
- 따라서 busy waiting 방법 말고, `block & waiting` 방식으로 하면 더 효율적

### (4) Block & Wakeup Implementation

<p align="left" width="100%"><img width="1000" alt="Block & Wakeup Implementation" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/a7c9cec0-199a-451c-92c6-69ddd7e8c225">

- value가 하나 있고, semaphore를 줄 세워서 변수 L에 대해 프로세스들을 blocked : 줄을 세워 기다리게 함 (process wait queue)
- 누군가 semaphore를 쓰고 있고 V 연산을 통해 뱉어낸다면 → 줄 세워있는 프로세스들을 깨워주면 됨

### (5) Implementation

> Semaphore 연산이 다음과 같이 정의됨 (앞의 코드와는 다르게 현재 자원이 얼마나 남아있는지는 알기 어려운 코드임)

<p align="left" width="100%"><img width="1000" alt="Implementation" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/5782c15b-77da-4b76-97da-c4a5cea56938">

#### P 연산 시 일단 S value를 1을 뺀다

- 이미 semaphore 자원이 모두 다 사용 중이기에 0이라도 1을 뺀다 (심지어 음수일지라도)
- 이미 누군가가 쓰고 있어 음수면 → 여분이 없다 → Semaphore L (리스트)에 block 시켜서 잠들게 함

#### 언제 깨어나나?

- 누군가가 V 연산을 해서 자원을 내어 놓을 때 S value 1 증가 → 자원을 반납했더라도 꼭 양수가 된다는 보장은 없다

#### value에 따른 상황

- 만약 음수다? → Semaphore를 기다리면서 block 되어 잠들어 있는 상황
- 만약 양수다? → Semaphore가 남아돌아서 어느 누구도 쓰고 있지 않은 상황

### (6) Busy-waiting vs. Block/Wake up

- 일반적으로 Block/Wake up 방식이 더 좋음 (특히 여분이 없는데 쓸데없이 CPU를 쓰면서 기다리는 busy-wait 보다는 낫기 때문)
- critical section의 길이가 긴 경우 (여분이 없을 때)
  - Block/Wake up이 적당함
- critical section의 길이가 매우 짧은 경우 (critical section에 들어가는게 생각보다 엄청 빈번하지 않아 경쟁이 심하지 않다면)
  - 오히려 Block/Wake up 오버헤드가 busy-wait 오버헤드보다 더 커질 수 있음
  - 따라서 그냥 busy waiting 하는게 더 효율적일 수 도
- 여분이 생길 때는 Block/Wake up 방식이 더 낫지만, 어떻게 보면 오버헤드라고 볼 수 있음

### (7) Two Types of Semaphores

#### Counting Semaphore

- Counting: 자원이 여러 개 있을 때는 카운팅이 중요하기에 이렇게 부름
- 도메인이 0 이상인 임의의 정수 값
- 주로 resource counting에 사용

#### Binaray Semaphore

- Binaray: 값이 0 또는 1인 경우로 동시 접속을 못하게 mutual exclustion 를 위해 사용됨
- 락을 걸고 락을 푸는 용도 (양 쪽 다 쓸수 있다는 의미)
- 주로 mutual exclustion (lock/unlock)에 사용
