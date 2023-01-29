# 3. 프로세스의 이해

### 프로세스

- 정의: 실행 중인 프로그램
- 하나의 프로세스가 수행하는데 필요한 자원들
    - CPU 시간
    - 메모리
    - 파일
    - I/O 디바이스
    

### 메모리 레이아웃 세션

- text section
    - 명령어들이 있는 섹션
- data section
    - 전역변수들이 저장
- heap section
    - 메모리 얼로케이션을 했을때 저장되는 힙영역
- stack section
    - 함수가 호출되었을때 가지고 있는 영역
    
    os가 가지고 있는 프로세스의 종류
    
  ![test](https://user-images.githubusercontent.com/78361650/215335906-42cbfb30-cfc3-47b7-ad07-a14ae6935e89.jpg)

    
    new:  프로세스가 생성된 시작점
    
    running: 프로세스가 cpu를 점유하는 상태
    
    Waiting: 프로세스가 이벤트를 기다리는 상태 ( I/O 완료, 신호 수신)
    
    Ready: 준비가 다 된후 기다리는 상태
    

Terminated: 프로세스가 종료된 상태

### PCB(Process Control Block)

- 구조체 하나에 프로세스가 가져야할 정보를 다 저장한다는 뜻

PCB 에 연결되어있는 정보들

- process state
- program counter
- CPU registers
- CPU scheduling information
- Memory-management inforamation
- Accounting information
- I/O status information

### 쓰레드

프로세스를 가볍게 하기위한 개념

### 멀티 프로그래밍

cpu 사용율을 높이기 위해 사용

### time sharing

cpu 코어의 전환을 통해 동시에 도는것처럼 진행시키는 방식

### SCheduling Queues

프로세스가 시스템에 들어가서, ready queue 상태로 들어가고, 시간이 끝나면 큐의 방식대로 다시뒤로 가거나 ,  waiting으로 간다. 웨이팅으로 가면 i/o를 요청하다가 종료되면 큐 방식대로 맨 뒤로 간다

Queueing Diagram

![test1](https://user-images.githubusercontent.com/78361650/215335915-8effa151-f955-4e8f-a971-e6d73421cd3e.jpg)

### Context Switch

프로세스가 사용되고있는 상태

인터럽트가 일어났을때 문맥 상태를 저장해 두고 다시 cpu를 시작시켰을때 문맥을 실행시킨다.

하는일

- 현재 프로세스의 일을 저장한다.
- 새로 획득한 프로세스의 문맥 상태를 보관한다.

![test2](https://user-images.githubusercontent.com/78361650/215335921-3eb48329-270d-45c8-aabd-7cf8568f11b1.jpg)


### 간단한 프로세스 진행 실습

유닉스에서 새 프로세스는  fork() 라는 시스템 호출에 의해 생성됨

자식 프로세스는 부모 프로세스의 주소 공간을 그대로 복사해서 쓴다.

fork()호출 후 부모는 할 일을 하거나, 자식이 일하는 동안 잠시 기다린 후, 레디큐에에서 기다렸다가 자식 큐가 인터럽트가 걸리길 기다린다.

프로세스는 동시에 실행된다 어떻게?

독립적(independent)이고 협력적(cooperating)이게

### IPC(Inter Process Communication)

데이터를 교환하는 과정

2가지 모델을 사용한다.

- shared memory
- message passing

![test3](https://user-images.githubusercontent.com/78361650/215335930-5745a659-29e6-43de-b013-4297e5c9448d.jpg)

a - shared memory , b - message passing

버퍼 관련..이해부족으로 …직접 보길 권고..
