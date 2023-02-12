# Chapter 5. CPU Scheduling

### CPU스케줄링이란

멀티프로그래밍 연산 시스템의 기초이다.

**multiprogramming이란?**

- 모든시간에 프로세싱하는 것
- CPU를 최고까지 이용하기 위한 것

![image](https://user-images.githubusercontent.com/78361650/218301747-d06f920b-a3b6-4ff8-8b39-8e543ca71b75.png)

cpu scheduler

- 메모리에 로드되어 있는 프로그래스 중에 어떤 프로그레스에 cpu를 할당해줄지 확인해주는 것
    - 준비상태에 있는 것들중에서 어떤 프로세스에 할당할수 있는지 정하는 것
- 어떻게 다음 프로세스를 선택하느 것인가?
    - FIFO Queue
    - Priority QUeue
    

### Preemptive vs Non preemptive ( 선점형 vs 비선점형 )

**Non-preemptive** scheduling

- 프로세스가 릴리즈 할때까지 종료 하거나 바뀔때까지 내버려둠

**Preemptive** scheduling

- 스케줄링에 의해 강제로 바꿀수 있음

 Decision Making for CPU- scheduling

1. 실행상태에서 대기 상태로 가는 경우
2. 실행상태에서 준비상태로
3. 대기 상태에 준비상태로 가는경우
4. 프로세스가 끝나는 경우

### **Dispatcher**

cpu 코어에 대한 컨트롤 권한을 프로세스에게 주는 것

디스페치는 빨라야만 한다.

 
![image](https://user-images.githubusercontent.com/78361650/218301762-264f00c4-74c4-429c-96e7-1399caf1df7b.png)


### Scheduling Criteria

CPU utilization: cpu동작이 바쁜 상태를 유지시킨다.

Throughput : 단위시간내 프로세스작업을 완성시키는 것

Turnaround time :  프로세스가 실행에서 종료까지의 시간을 최소화

Waiting time : 프로세스의 대기 시간을 최소화 시키는 것

response time: UI같은 부분에서 응답 시간을 최소화

### SCheduling Problemm soultion

- FCFS : First-Come, First-Served
- SJF : shortest job first(SRTF: shortest remaing time first)
- RR: Round-robin
- Priority-based
- BLQ: multi level quque
- MLFQ: multi level feddback quque

**FSFS scheduling**

먼저 요청한  CPU부터 처리해주는 스케줄링 알고리즘

단점 : 미니멀하지 않고, 프로세스 대기시간이 너무 뒤죽박죽이다.(같은 건데 빠를수도 느릴수도 있음- 비선점형이기때문에)

**SJF scheduling**

오는 순서에 무관하게 가장 적은것부터 처리한다.
