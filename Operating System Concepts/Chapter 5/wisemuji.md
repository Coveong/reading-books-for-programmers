- https://www.inflearn.com/course/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B3%B5%EB%A3%A1%EC%B1%85-%EC%A0%84%EA%B3%B5%EA%B0%95%EC%9D%98#curriculum
- 주니온 박사님 강의 중 Chapter 5에 대한 정리

# Chapter 5. CPU Scheduling

## CPU 스케줄링
- 멀티프로그래밍된 운영 체제의 기초 개념.
- 멀티프로그래밍의 목적은 일부 프로세스를 항상 실행하고 CPU 효율을 극대화한다.
- 메모리의 프로세스에서 프로세스를 선택하고 실행할 준비가 되면 CPU를 해당 프로세스에 할당한다.
  - 다음 프로세스를 선택할 때에는 Linked List, Binary Tree, FIFO Queue, Priority Queue 등의 전략이 선택될 수 있다.

### 선점 vs 비선점 스케쥴링
- 비선점 스케줄링
  - 프로세스는 CPU를 해제할 때까지 CPU를 유지하여 종료하거나 대기 상태로 전환한다.
- 선점형 스케줄링
  - 프로세스는 스케줄러에 의해 선점될 수 있다.

## 디스패쳐
CPU 코어를 제어하는 모듈
- CPU 스케줄러에서 선택한 프로세스로 이동한다.
- 역할
  - 프로세스 간에 컨텍스트 전환
  - 사용자 모드로 전환
  - 사용자 프로그램을 다시 시작하기 위해 적절한 위치로 이동
- 디스패처는 컨텍스트 스위칭이 발생할 때 항상 호출되므로 가능한 한 빨라야 한다.
  - 디스패쳐 레이턴시: 한 프로세스를 중지하고 다른 실행을 시작하는 시간

## 스케쥴링 기준
- CPU 사용률: CPU를 가능한 바쁘게 유지한다.
- 처리량: 시간 단위당 완료된 프로세스 수
- 처리 시간:
  - 프로세스를 실행하는 데 얼마나 걸리는지
  - 제출 시점부터 완료 시점까지.
- 대기 시간:
  - 프로세스가 대기열에서 대기하는 데 소요되는 시간
  - 대기열에서 대기한 기간의 합계
- 응답 시간:
  - 응답하기 시작하는 데 걸리는 시간

## 스케쥴링 문제 해결 방법
### FCFS: First-Come, First-Served
- 선점형
- 먼저 요청한 CPU에 먼저 배정. 가장 간단하다.
- CPU burst time에 따라 편차가 있음.
- 큰 작업이 앞에 막고 있으면 뒷 작업 모두 오래 걸린다. (똥차 효과)

### SJF: Shortest Job First (SRTF: Shortest Remaining Time First -> 선점형)
- 선점형이거나 비선점형이다.
- CPU가 가능하다면 burst가 가장 작은 작업 먼저 배정. 
- 다음 CPU burst를 정확히 알아내기 어렵기 때문에 예측해야 한다. (과거에 사용된 내역을 참고)

### RR: Round-Robin
- 시분할 선점형 FCFS

### Priority-based
- 선점형이거나 비선점형이다.
- 우선순위를 부여하여 우선순위가 높은 순서대로 선점.
- SJF는 Priority based의 하위에 해당됨.
- 우선순위가 낮은 작업이 무한정 기다리는 현상이 생길 수 있다.(starvation 문제)
  - aging 기법을 주어 해결할 수 있다.
- RR과 Priority 스케쥴링을 합치는 방법도 있다.
 
### MLQ: Multi-Level Queue
- 프로세스의 종류를 나누어 대기 큐를 따로 관리한다.

### MLFQ: Multi-Level Feedback Queue
- MLQ에 aging 기법을 준다.
- 실전 OS의 CPU 알고리즘
