- https://www.inflearn.com/course/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B3%B5%EB%A3%A1%EC%B1%85-%EC%A0%84%EA%B3%B5%EA%B0%95%EC%9D%98#curriculum
- 주니온 박사님 강의 중 Chapter 3에 대한 정리

# Chapter 3. Processes (Part 1)

## 프로세스란?
- 정의: 실행 중인 프로그램
- 하나의 프로세스가 테스크를 수행하는데에 필요한 리소스들
  - CPU 시간
  - 메모리
  - 파일
  - I/O 디바이스

### 프로세스의 메모리 레이아웃
<img width="311" alt="image" src="https://user-images.githubusercontent.com/32327475/215326394-7635340a-9233-4f68-a0b7-1632540735f0.png">

- 텍스트 섹션:
  - 실행 코드
- 데이터 섹션:
  - 전역 변수
- 힙 섹션:
  - 프로그램 실행 시간 동안 동적으로 할당되는 메모리
- 스택 섹션:
  - 함수를 호출할 때의 임시 데이터 저장소
  - 예를 들어 함수 매개 변수, 반환 주소 및 로컬 변수

### 프로세스의 상태(State) 종류
- New: 프로세스가 생성되고 있다.
- Running: 명령이 실행되고 있다.
- Waiting: 이벤트가 발생하기를 기다리는 중이다.(I/O 완료 또는 신호 수신)
- Ready: 프로세스가 프로세서에 할당되기를 기다리고 있다.
- Terminated: 프로세스 실행이 완료되었다.

### PCB(Process Control Block)
각 프로세스는 PCB에 의해 운영 체제에 나타난다.

PCB에는 특정 프로세스와 관련된 많은 정보가 포함되어 있다:
- 프로세스 상태(State)
- 프로그램 카운터
- CPU 레지스터
- CPU 스케줄링 정보
- 메모리 관리 정보 
- 계정 정보
- I/O 상태 정보

## 스레드
- 프로세스는 단일 실행 스레드를 수행하는 프로그램이다.
  - 단일 제어 스레드를 통해 프로세스를 수행할 수 있다.
  - 한 번에 하나의 작업만 수행한다.
- 최신 운영 체제는 프로세스가 여러 실행 스레드를 갖도록 허용하도록 프로세스 개념을 확장했다(멀티프로세스).
  - 따라서 한 번에 두 개 이상의 작업을 수행할 수 있다.
- 스레드는 경량화된(lightweight) 프로세스다. (Chapter 4에서 더 잘 다룸)

## 멀티프로그래밍

- 멀티프로그래밍의 목표는 CPU 활용률을 극대화하기 위해 일부 프로세스를 항상 실행하는 것이다.
- 시분할의 목적은 프로세스 간에 CPU 코어를 자주 전환하여 사용자 입장에서 동시에 실행되는 것처럼 보이게 하는 것이다.

### ready queue와 wait queue
<img width="724" alt="image" src="https://user-images.githubusercontent.com/32327475/215327234-7c2f3e17-a88f-4fa2-8ad3-82fc9d90e4aa.png">

참고: Queuing 다이어그램 이해하기

### Context Switch
- 인터럽트가 발생하면 시스템은 실행 중인 프로세스의 현재 컨텍스트를 저장하므로 재개해야 할 때 해당 컨텍스트를 복원할 수 있다.
- CPU 코어를 다른 프로세스로 전환하여 현재 프로세스의 상태 저장 및 다른 프로세스의 상태 복원을 수행한다.
<img width="614" alt="image" src="https://user-images.githubusercontent.com/32327475/215327400-e5c9603a-27db-4f8f-a96b-6d5292409b97.png">

## 프로세스의 실행
- 실행의 두 가지 가능성
  - 부모는 자식과 동시에(concurrently) 실행됨
  - 부모는 자식이 종료될때까지 기다림
- 주소(address-space)의 두 가지 가능성
  - 자식은 부모와 중복됨
  - 자식에 새 프로그램이 로드됨
<img width="959" alt="image" src="https://user-images.githubusercontent.com/32327475/215327657-4a79af3e-5fbc-4d72-bd7d-a3ce7c3249e9.png">

### 좀비와 Orphan
- 좀비 프로세스: 종료되었지만 부모가 아직 대기를 호출하지 않은 프로세스
- Orphan 프로세스: wait()을 호출하지 않고 종료된 부모가 있는 프로세스
