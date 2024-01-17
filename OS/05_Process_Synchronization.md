# 병행 제어

## 1. 컴퓨터의 연산

<p align="left" width="100%"><img width="800" alt="image" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/42000663-94c1-4bc0-b8b7-75f6e1840946">

- 컴퓨터에서 연산을 할 때는 항상 데이터를 읽어와서 연산 → 그 결과를 다시 어딘가에 저장하도록 되어 있음
- 메모리에 있는 데이터를 읽어와서 CPU에서 연산을 하면 다시 데이터를 가져다가 메모리에서 사용하는 원리
- 연산을 할 때는 그 자리에서 하는 게 아니라, 뭔가를 불러들여와서 진행하고, 마치면 밖에다가 다시 내보내는 원리

<br/>

## 2. Race Condition

<p align="left" width="100%"><img width="800" alt="image" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/1820b341-e4e6-4387-93a4-5d9432b601d8">

### 문제의 발생

- 보통 데이터를 한 군데가 아니라, 여러 군데에서 읽어갈 때 발생함
- 예를 들어, count 변수 값이 왼쪽에서는 증가, 오른쪽에서는 감소 시킨다고 했을 때 이 count 값은 원래 값이 저장되어 있어야 함.
- 그런데 이렇게 연산하는 주체가 두 개가 있으면, 한 쪽에서 연산하는 “도중에” 데이터를 읽어갔을 때 문제가 발생할 수도 있음
- 하나의 공유 데이터를 여러 개가 동시에 접근하려고 할 때의 상태 → Race Condition (경쟁 상태)

### 정말 생길까?

- CPU가 여러 개 있으면 Race Condition 발생 가능하나, 꼭 생기는건 아님
- 물론 CPU가 하나 있어도 Race Condition이 발생하지 않는다고 볼 수 없음

### 문제가 일어나는 과정

> CPU가 하나 있더라도 운영체제가 끼어드는 경우

- 예를 들어, A 라는 프로그램이 실행중인데 이때 프로세스는 본인이 직접 할 수 없는 일을 운영체제에게 대신 해달라고 요청 → system call
- 프로세스 A가 CPU를 잡고 일하다가 → 내가 할 수 없는 일이라 판단해 system call → 이때 A는 운영체제 안의 데이터 값을 바꾸고 있던 상황 → 그런데 자신의 CPU 할당 시간이 끝나버림
- CPU가 A에서 프로세스 B에게 넘어감 → B가 CPU를 잡고 일하고 있는데 → B도 본인이 할 수 없는 일을 하게 되어 운영체제에게 system call → 이번에는 B의 요청으로 또다시 운영체제의 코드가 실행됨
- 그런데 운영체제 데이터에서 A가 건드리던 데이터를 또 똑같이 건드리게 됨 →운영체제는 하나니까 A가 요청하든 B가 요청하든 동일한 데이터를 건드릴 수 있는 것
- 아까 A가 운영체제의 어떤 데이터를 읽어서 변경하려는 ‘찰나에’ CPU가 B에게 넘어감 → B의 요청으로 운영체제가 변수를 변경함 → 나중에 CPU가 A에게 다시 넘어오면 → 아까 system call 했을 때 ‘하다 말았던’ 일을 계속 해야하는데 이미 이 데이터를 읽어 와버림 → 분명 CPU가 한 개였고 프로세스들끼리 데이터를 공유하지도 않았는데 운영체제 안의 데이터를 건드리면서 문제가 발생

### 정리: OS에서 Race Condition가 발생하는 경우

1. kernel 수행 중 인터럽트 발생 시
2. 프로세스가 system call을 해 커널 모드로 수행중인데, context switch가 일어나는 경우
3. 멀티 프로세서에서 shared memory 내의 커널 데이터를 건드리는 경우

### OS에서의 Race Condition

<p align="left" width="100%"><img width="800" alt="image" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/a0f9ae90-96b3-483b-9bff-03a4ae465d4f">

- 프로그램 A가 user mode에서 본인의 코드를 실행할 때는 어차피 본인의 주소 공간의 데이터에 접근할테니 문제가 없음
- 그런데 system call을 통해 커널의 코드가 실행될 때는 커널의 데이터를 건드리는데, 커널의 데이터는 프로세스 입장에서는 일종의 **공유 데이터**라고 볼 수 있음
- 따라서 프로그램이 user mode, kernel mode 반복하면 user mode에서는 주소 공간을 공유하지 않기에 문제가 되지 않지만, kernel mode에서 kernel의 데이터를 건드리는 도중에 CPU를 빼앗기게 되면 Race Condition이 발생한다.

### 해결책

<p align="left" width="100%"><img width="800" alt="image" src="https://github.com/ella-yschoi/CS-Study/assets/123397411/58de6d59-3832-4795-941b-eb0bdd631d7c">

- kernel mode에서 ‘수행 중’일 때는 CPU를 preempt 하지 않음 (빼앗지 않음)
- kernel mode에서 user mode로 돌아갈 ‘때’ preempt 하기
