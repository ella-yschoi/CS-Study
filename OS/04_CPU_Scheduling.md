# CPU 스케줄링

## 1. CPU and I/O Bursts in Program Execution

<p align="center" width="100%"><img width="1010" alt="image" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/05bdab1a-786b-43da-b85d-d39ccee6916f">

<br/>

## 2. CPU-burst Time의 분포

> CPU로 기계어를 실행하는 단계

<p align="center" width="100%"><img width="1010" alt="image" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/87bb3ca5-ade5-4bf2-816a-cf4a55822b02">

### CPU 스케줄링의 필요성

- 여러 종류의 job(=process)이 섞여 있기 때문에 CPU 스케줄링이 필요
  - Interactive job에게 적절한 response 제공 요망
  - CPU와 I/O 장치 등 시스템 자원을 골고루 효율적으로 사용
- I/O bound job가 CPU를 빨리 얻을 수 있도록 함
  - CPU를 먼저 주어야 빨리빨리 CPU를 쓰고 나가기 때문
  - 사람과 Interaction하기 때문에 빠른 응답성 제공 필요
  - 쓰고나서 바로 I/O 작업이 이루어지는데, CPU를 못 주고 있으면 CPU도 못 얻을 뿐만 아니라 I/O 도 못할 것이기 때문

<br/>

## 3. Process의 특성 분류

### (1) CPU-bound process

- CPU를 길게 쓰고, I/O를 짧게 쓰는 작업
- 사람과 Interaction을 많이 하는 경우 (계산 위주의 job 등)
- few & very long CPU bursts

### (2) I/O-bound process

- CPU를 짧게 쓰고, I/O를 길게 쓰는 작업
- CPU를 잡고 계산하는 시간보다 I/O에 많은 시간이 필요한 경우 (과학적 연산 등)
- many & short CPU bursts

<br/>

## 4. CPU Scheduler & Dispatcher

> 소프트웨어이며, CPU 스케줄링을 하는 운영체제 안에 있는 코드

### (1) CPU 스케줄러

- Ready 상태의 프로세스 중에서 이번에 CPU를 어떤 프로세스에게 줄지 고르는 역할

### (2) Dispatcher

- CPU의 제어권을 CPU scheduler에 의해 선택된 프로세스에게 넘김
- 즉, 결정된 프로세스에게 실제로 CPU를 넘기는 역할
- 운영체제의 일부이며 이 과정을 context switch라고 함

### (3) CPU 스케줄링이 필요한 경우

프로세스에게 아래와 같은 상태 변화가 있는 경우

1. Running → Blocked (e.g. I/O 요청하는 시스템 콜)
2. Running → Ready (e.g. 할당시간만료로 timer interrupt)
3. Blocked → Ready (e.g. I/O 완료 후 인터럽트)
4. Terminate

- 1, 4 에서의 스케줄링은 nonpreemptive (= 강제로 빼앗지 않고 자진 반납)
- 그 외 다른 스케줄링은 preemptive (= 정책상 다른 것들도 번갈아 CPU 사용해야 하기에 강제로 빼앗음)

<br/>

## 5. Scheduling Criteria

- Performance Index (= Performance Measure, 성능 척도)
- 중국집 주방장에 비유하면 이해하기 쉬움

### (1) CPU Utilization (이용률)

- keep the CPU as busy as possible
- CPU를 놀게 하지 않고 이용률은 높을수록 좋음

### (2) Throughput (처리량)

- number of processes that complete their execution per time unit
- 처리량은 많을수록 좋음
- e.g. 중국집 셰프가 많은 요리를 처리하면 할수록 좋음

### (3) Turnaround Time (소요 시간, 반환 시간)

- amount of time to execute a particular process
- CPU burst를 하고 나서, I/O burst를 하러 나가기까지 전체 시간
- Ready Queue에서 CPU를 기다린 시간 + 실제로 CPU를 사용한 시간
- e.g. 밥먹은 시간 + 대기시간 다 합친 시간

### (4) Waiting Time (대기 시간)

- amount of time a process has been waiting in the ready queue
- 처음으로 기다리는 시간이 아닌, CPU를 쓰러 와서 기다린 전체 시간의 합
- e.g. 중국집에서 웨이팅한 시간.. 단무지 → 짜장면 → 탕수육 → 디저트 각각 나오는 시간의 합

### (5) Response Time (응답 시간)

- amount of time it takes from when a request was submitted until the first response is produced, not output (for time-sharing environment)
- CPU를 쓰러 들어와 최초로 CPU를 얻기까지 걸린 시간
- 주의할 것은 프로세스가 처음으로 시작해서 처음 CPU를 얻은 시간이 아님. 즉, CPU를 쓰러 들어오는 여러 번의 burst 중, CPU burst를 들어와 최초로 CPU를 얻기까지 걸린 시간이라 보면 됨
- e.g. 중국집에서 최초로 단무지를 받기까지 걸리는 시간

<br/>

## 6. Scheduling Algorithms

### (1) FCFS(First-Come First-Served)

> Much better than previous case

#### 예시

<p align="center" width="100%"><img width="1010" alt="fcfs-example-1" src="https://github.com/ella-yschoi/TIL/assets/123397411/cd52e0fb-f323-45cf-8a78-635b4db71df9">

<p align="center" width="100%"><img width="1010" alt="fcfs-example-2" src="https://github.com/ella-yschoi/TIL/assets/123397411/b4fe68b4-6460-4dd5-84fa-b491070a41ff">

#### 특징

- 두 케이스 다 선착순으로 처리했으니 별 차이가 없는 것 같지만, 평균을 내보면 굉장히 많이 줄어들었음
- 왜? 앞에 CPU 사용 시간이 긴 프로세스가 들어와버리면 전체 프로세스의 기다리는 시간이 길어지기 때문 → 호위 효과

#### Convoy effect (호위 효과)

- short process behind long process
- 오래 걸리는 프로세스가 먼저 도착해 CPU를 오래 쓰는 탓에, 짧게 걸리는 프로세스가 오래 걸리게 되는 것

### (2) SJF (Shortest-Job-First)

#### 예시

<p align="center" width="100%"><img width="1010" alt="sjf-example-1" src="https://github.com/ella-yschoi/TIL/assets/123397411/6a7dcca0-f6b0-4a46-9aec-6a55ffe366d2">

<p align="center" width="100%"><img width="1010" alt="sjf-example-2" src="https://github.com/ella-yschoi/TIL/assets/123397411/b37088c8-2026-4c8f-bf20-aad0f96d5a81">

#### 특징

- 각 프로세스의 다음번 CPU burst time을 가지고 스케줄링에 활용
- CPU burst time이 가장 짧은 프로세스를 제일 먼저 스케줄링
- 대기 시간 측면에서 보자면 가장 최적의 방법. 이보다 평균 대기시간을 더 짧게 할 수는 없음

#### SJF is optional

- 평균 대기시간이 짧고 minimum average waiting time을 보장함
- Preemptive한 SJF == SRTF(Shortest-Remaining-Time-First)라고도 부름
- 대기 시간 측면에서 최적인 방법

#### High Priority를 가진 process에게 CPU 할당

- SJF는 일종의 Priority Scheduling
- Priority = predicated next CPU burst time

#### Two schemes

- Nonpreemptive
  - 일단 CPU를 잡으면 이번 CPU burst가 **완료될 때까지 CPU를 선점당하지 않음**
  - 갑자기 더 짧은 프로세스가 등장하더라도 이미 CPU를 주었으면 해당 프로세스가 다 쓸 때까지 빼앗지 않음

- Preemptive
  - 현재 수행중인 프로세스의 남은 burst time보다 **더 짧은 CPU burst time을 가지는 새로운 프로젝트가 도착하면 CPU를 빼앗김**
  - Nonpreemptive 보다 더 최적화된 방법

### (3) SJF의 단점

#### starvation 발생

- SJF는 너무 효율성만 생각하다보니, 짧은 프로세스에게 우선권을 주기 때문
- queue가 계속 쌓이면 long job은 영원히 CPU를 얻지 못할 수도 있음

#### 다음 CPU Burst Time의 예측

- 누가 짧게 쓰고 누가 길게 쓰는지 알려주지 않음
- CPU queue에 들어올 때 미리 알 수 없기 때문
- 즉, 프로그램이라는 게 순차적으로 진행되기보단 if문 등 그때그때 상황이 다르기 때문
- 그렇다면 다음번 CPU burst time을 어떻게 알 수 있을까?
- CPU burst를 알 수 없다면 과거의 CPU burst time을 기반으로 추정은 가능

<br/>

## 7. Priority Scheduling

> A priority number (integer) is associated with each process

### 특징

- highest priority(smallest integer)를 가진 프로세스에게 CPU 할당
- preemptive: 우선순위가 더 높으면 빼앗아서 줌
- nonpreemptive: 빼앗지 않고 기다림
- SJF는 일종의 priority scheduling

### 문제점

- Starvation: low priority processes may never execute

### 해결책

- Aging: as time progresses increase the priority of the process
- 경로우대 같은 개념

<br/>

## 8. Round Robin

### 특징

- 각 프로세스는 동일한 크기의 할당 시간(time quantum)을 가짐
- 할당 시간이 지나면 프로세스는 선점(preempted) 당하고, ready queue의 제일 뒤에 가서 다시 줄을 선다
- n개의 프로세스가 ready queue에 있고, 할당 시간이 q time unit인 경우, 각 프로세스는 최대 q time unit 단위로 CPU 시간의 1/n을 얻는다.
- 즉, 어떠한 프로세스도 (n-1) q time unit 이상 기다리지 않는다.

### Performance

- 할당 시간이 너무 길면 → FCFS
- 할당 시간이 너무 짧으면 → context switch가 너무 빈번하게 일어남

<br/>

## 9. Multilevel Queue

> Ready queue를 여러 개로 분할

<p align="center" width="100%"><img width="1010" alt="image" src="https://github.com/ella-yschoi/TIL/assets/123397411/f801abd8-bfdd-4455-9d49-54465a7d7778">

### (1) Ready queue를 여러 개로 분할

- foreground: interactive
- background: batch - no human interaction

### (2) 각 queue는 독립적인 스케줄링 알고리즘을 가짐

- foreground: Round Robin
- background: FCFS (긴 프로세스 하나가 context switch 없이 쭉 실행)
- Queue에 대한 스케줄링이 필요 (각 queue 마다 wait을 주는 것)

### (3) Fixed priority scheduling

- serve all from foreground then from background
- starvation의 가능성이 있음

### (4) Time slice

- 각 queue에 CPU time을 적절한 비율로 할당
- e.g. 80% to foreground in RR, 20% to background in FCFS
- foreground에 우선순위를 주는데 무조건 주는건 아니고, 각 큐마다 시간의 weight를 주는 것

<br/>

## 10. Multilevel Feedback Queue

<p align="center" width="100%"><img width="1010" alt="image" src="https://github.com/ella-yschoi/TIL/assets/123397411/93ec9f6a-fee8-430c-977f-ac8bd1bf4eb0">

### (1) 특징

- 줄 간의 이동이 가능
- 한번 정해진 queue는 바뀌지 않음
- Process가 다른 queue로 이동 가능
- 상위 queue가 우선순위가 높다
- 상위 queue가 비어야 하위 queue가 채워짐
- aging을 이와 같은 방식으로 구현 가능

### (2) Multilevel Feedback Queue Scheduler를 정의하는 파라미터들

- Queue의 수
- 각 queue의 scheduling algorithm
- 프로세스를 상위 queue로 보내는 기준
- 프로세스를 하위 queue로 보내는 기준
- 프로세스가 CPU 서비스를 받으려 할 때 들어갈 queue를 결정하는 기준

<br/>

## 11. Example of Multilevel Feedback Queue

### (1) Three queues

- Q0 - time quantum 8 milliseconds
- Q1 - time quantum 16 milliseconds
- Q2 - FCFS

### (2) Scheduling

- new job이 queue Q0로 들어감
- 8ms 동안 다 끝내지 못했으면 queue Q1으로 내려감
- Q1에 줄서서 기다렸다가 CPU를 잡아서 16ms 동안 수행됨
- 16ms 안에 끝내지 못한 경우 queue Q2로 쫓겨남

<br/>

## 12. Multiple-Processor Scheduling

> CPU가 여러 개인 경우, 스케줄링은 더욱 복잡해짐

### (1) Homogeneous processor인 경우

- Queue에 한 줄로 세워서 각 프로세서가 알아서 꺼내가게 할 수 있음
- 반드시 특정 프로세서에서 수행되어야 하는 프로세스가 있는 경우에는 문제가 더 복잡해짐

### (2) Load Sharing

- 일부 프로세서에 job이 몰리지 않도록 부하를 적절히 공유하는 메커니즘 필요
- 별개의 queue를 두는 방법 vs. 공동의 queue를 사용하는 방법

### (3) Symmetric Multiprocessing (SMP)

- 각 프로세서가 각자 알아서 스케줄링 결정

### (4) Asymmetric Multiprocessing

- 하나의 프로세서가 시스템 데이터의 접근과 공유를 책임지고 나머지 프로세서는 거기에 따름

<br/>

## 13. 특수한 경우의 CPU 스케줄링

### (1) Load Balancing

- 기본적으로 CPU가 여러 개 있는 경우는 Load Balancing
- 여러 CPU가 밸런스 있게 골고루 일하게 하는 것이 중요
- 그렇지 않으면 전체 성능이 떨어짐

### (2) SMP (Symmetric Multiprocessing)

- 하나의 대장 CPU가 있는게 아니라 여러 CPU들이 대등함
- 따라서 각 프로세서가 각자 알아서 스케줄링을 결정

### (3) ASMP (Asymmetric Multiprocessing)

- SMP의 비효율적인 측면을 보완하는 방법
- 한 개의 CPU가 대장 CPU가 되어 데이터의 접근과 공유를 책임지고, 나머지 CPU는 거기에 따름

<br/>

## 14. Real-Time Scheduling

### (1) Hard real-time systems

- 정해진 시간 안에 반드시 끝내도록 스케줄링 해야 함
- 이를 위해 각 프로세스들의 CPU 도착 시간을 미리 알고 스케줄링해 시스템이 따라가도록 하기도 함 → Offline Scheduling
- 각 작업들의 load가 얼마인지 고려해서 CPU를 남겨두어야 함

### (2) Soft real-time computing

- 일반 프로세스에 비해 높은 priority를 갖도록 해야 함
- 데드라인을 보장해주면 좋지만 비용이 비싸서 일반 프로세스와 섞어 사용
- e.g. 동영상이 끊기지 않도록 동영상 재생 프로세스가 먼저 CPU를 얻을 수 있도록 priority를 높임

<br/>

## 15. Thread Scheduling

### (1) Thread

- 프로세스 내에서 실행되는 가장 작은 실행 단위
- 각 스레드는 동일한 하나의 프로세스 안에서 실행 중인 다른 프로세스와 메모리 및 자원을 공유
- 이러한 구조는 프로세스 간의 통신을 용이하게 하고, 자원의 효율적 사용을 가능하게 함

### (2) Local Scheduling

- 운영체제는 스레드의 존재를 모르고, 사용자 프로세스 본인이 내부에 스레드를 여러 개 두기에 운영체제는 어느 프로그램에 CPU를 주겠다는 의사결정을 하지 못함
- 대신, 사용자 수준의 스레드 라이브러리(User level thread)가 스레드 간의 스케줄링을 담당
- 이 스레드는 사용자 수준에서 관리되기에 커널의 개입 없이 빠르게 스위치할 수 있지만, 한 스레드가 block 되면 전체 프로세스가 block 될 수 있음

### (3) Global Scheduling

- 글로벌 스케줄링은 커널 수준에서 이루어지며, 여기서는 운영체제가 스레드의 존재를 알 수 있으며, 커널의 스케줄러가 어떤 스레드를 실행할지 결정
- 이는 각 스레드가 별도의 커널 객체로 관리되기에 가능
- 즉, 커널의 단기 스케줄러가 어떤 thread를 스케줄링할지 결정
- 이 방식은 멀티 프로세서 시스템에서 효율적이지만, 사용자 수준 스레드보다 context switch에 더 많은 비용이 들 수 있음

### (4) Global Scheduling이 멀티 프로세서 시스템에서 효율적인 이유

> 멀티 프로세서 시스템에서는 여러 CPU 코어가 동시에 작동하며, 이 환경에서 글로벌 스케줄링은 아래의 이점을 제공

#### a. 자원 분배의 유연성

글로벌 스케줄링에서는 커널이 모든 스레드를 인식하고 관리한다. 이는 시스템 전체의 스레드를 다양한 프로세서 코어에 할당하는 유연성을 제공한다. 따라서, 작업 부하를 여러 코어에 균등하게 분산시켜 시스템의 전반적인 성능을 극대화할 수 있다.

#### b. 효율적인 로드 밸런싱

멀티 프로세서 시스템에서는 각 프로세서의 부하를 모니터링하고, 스레드를 다른 프로세서로 이동시켜 부하를 균등하게 분산시킬 수 있다. 이는 로드 밸런싱을 통해 시스템의 효율성을 높이는 데 중요한 역할을 한다.

#### c. 병렬 처리의 최적화

멀티 프로세서 시스템에서는 여러 스레드가 동시에 실행될 수 있다. 글로벌 스케줄링을 통해 이러한 병렬 처리를 최적화할 수 있으며, 이는 특히 병렬 계산이 많은 작업에서 중요하다.

#### d. 데드락 및 기아 상태 관리

멀티 프로세서 환경에서 데드락(Deadlock) 및 기아(Starvation) 상태는 프로세스 및 스레드 관리의 복잡성을 증가시킨다. 글로벌 스케줄링은 이러한 문제를 관리하고 해결하는 데 필요한 전반적인 관점을 제공한다.

#### e. 동기화와 일관성 유지

멀티 프로세서 환경에서 데이터와 자원의 동기화는 중요한 과제이다. 글로벌 스케줄링을 통해 데이터의 일관성을 유지하고, 자원 접근 시 발생할 수 있는 충돌을 효과적으로 관리할 수 있다.

<br/>

## 16. Algorithm Evaluation

> 어떤 CPU 스케줄링 알고리즘이 좋은지 평가하는 척도

### (1) Queueing models

- 확률분포로 주어지는 arrival rate와 service rate 등을 통해 각종 performance index 값을 계산
- 서버가 요청을 처리하는데에 요청이 queue에 쌓여있으면 서버가 처리, 못하면 queue에 남아있음
- CPU를 쓰고자 하는 요청들이 queue에 도착하는데, 얼마나 빠른 빈도로 도착하는지 → 이 도착률이 확률 분포로 주어짐
- CPU의 성능 or 역량을 ‘처리율’ 이라고 함
- 복잡한 수식을 계산하고 나면 Throughput(처리량), Waiting Time(대기 시간)이 도출됨

### (2) Implementaion and Measurement

- 실제 시스템에 알고리즘을 구현하며 실제 작업(workload)에 대해 성능을 측정하며 비교

### (3) Simulation

- 알고리즘을 모의 프로그램으로 작성 후 trace를 입력으로 하여 결과 비교
- trace란, Input이 되는 data를 말하며 실제 시스템에서 돌아가는 프로그램의 패턴과 유사하거나 대변할 수 있을 정도로 신빙성이 있어야 함
