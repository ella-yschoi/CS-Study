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
