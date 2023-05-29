- [주니온 박사님 강의](https://www.inflearn.com/course/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B3%B5%EB%A3%A1%EC%B1%85-%EC%A0%84%EA%B3%B5%EA%B0%95%EC%9D%98#curriculum) 중 Chapter 7에 대한 정리

# Chapter 7. Synchronization Examples

## 대규모 동시성 제어 문제의 예: 
- Bounded-Buffer 문제
  - (생산자-소비자)Producer-Consumer 문제
- Readers-Writers 문제
- 철학자들의 저녁식사(Dining-Philosophers) 문제

## Bounded-Buffer 문제

각각 하나의 항목을 보유할 수 있는 n개의 버퍼로 구성된 풀에 대한 생산자-소비자 문제를 상기해보자. 
- 생산자는 소비자를 위해 전체 버퍼를 생산한다.
- 소비자는 생산자를 위해 빈 버퍼를 생성한다.

## Readers-Writers 문제

동시에 실행되는 프로세스가 공유 데이터에 대한 Reader 혹은 Writer인 경우에는 어떻게 될까?(예: 여러 개의 동시 프로세스 간에 공유되는 데이터베이스)
- Reader는 데이터베이스만 읽기를 원할 수 있지만 Writer는 데이터베이스를 업데이트(즉, 읽고 쓰기)할 수 있다.
- 두 명 이상의 Reader가 공유 데이터에 동시에 액세스하는 경우 괜찮지만
- 그러나 Writer와 다른 프로세스(Readers 또는 Writers)가 동시에 데이터베이스에 액세스하는 경우 혼란이 발생할 수 있다.

### Reader-Writer Locks
- Readers-Writers 문제는 Reader-Writer Locks을 제공하여 해결할 수 있다.
- 단, 여러 프로세스가 읽기 모드에서 Lock을 획득할 수 있지만 쓰기를 위한 Lock을 획득할 수 있는 프로세스는 하나뿐이다.

## Dining Philosophers(식사하는 철학자) 문제

<img width="254" alt="image" src="https://user-images.githubusercontent.com/32327475/235439854-f3f557cd-1ec2-49e1-9b37-200abac1c044.png">

다섯 개의 젓가락을 공유하며 생각하고 먹는 데 평생을 보내는 다섯 명의 철학자들이 있다고 가정하자.
- 때때로, 한 철학자는 배가 고파서 그녀와 가장 가까운 두 개의 젓가락을 집으려고 한다.
- 배고픈 철학자가 두 젓가락을 동시에 들고 있을 때, 그녀는 젓가락을 놓지 않고 먹는다.

**여러 프로세스에 여러 리소스를 할당하여 교착 상태를 방지하고 굶주림을 방지해야 한다**

### 세마포어 솔루션:
한 가지 간단한 해결책은 각 젓가락을 세마포어로 표현하는 것이다.
- 철학자는 𝑎𝑖𝑡()로 젓가락을 집는다.
- 그녀는 𝑠𝑔𝑛𝑙()로 젓가락을 놔준다.

## 교착 상태와 Starvation(기아) 문제:
- 다섯 명의 철학자들이 동시에 배가 고파서 각각 왼쪽 젓가락을 잡고, 오른쪽 젓가락을 잡으려고 한다고 가정하자. 교착 상태가 될 수 있다.
- 교착 상태를 제거하는 문제 해결 방법은 완전히 기아 문제를 해결해주지 않는다.

## 모니터 솔루션
- 두 젓가락 모두 이용한 가능한 상태일 때 젓가락을 집도록 한다.
- 상호 배제가 보장되고 교착 상태가 발생하지 않지만 기아는 여전히 가능하다.