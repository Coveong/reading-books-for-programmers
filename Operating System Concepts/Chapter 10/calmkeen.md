# Chapter 10. Virtual memory

## Virtual memory

피지컬메모리보다 프로그램이커도 실행할수있게 만들어주는 메모리

메인메모리를 가상의 메모리라고 생각하면 **로지컬 메모리**와 **피지컬 메모리**를 분리할수 있다.

### virtual Address space

- 논리적 or 가상의 메모리 구조를 가진다.
- logical address는 0에서 시작해서  max까지.
- 

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/a86d0aeb-75c6-4ca5-9647-f66d4766a306)

### Virtual Memory

- 두개이상의 메모리에서 프로세스를 공유할때 파일과 메모리를 공유를 가능하게 한다.

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/e9a75167-2155-415e-9300-8b4bef034c66)

## Demand Paging

프로세스가 실행중일때.

- 메모리나 세컨드 메모리에 있을경우에서 표시할경우
- valid - 페이지가 정상적으로 로드되어있따.
- invalid - 페이지를 벗어나서 생성되어 있거나 세컨드 스토리지에 있을 수 있다.

### Page Fault가 일어날시 방법

1. 내부에 있는 페이지테이블을 확인한다.(참조된 메모리가 valid인지 invalid인지 확인)
2. valid일경우 pass, valid 이지만 page fault의 경우 페이지로 접속한다.
3. Freeframe(특정 영역)을찾는다.
4. 보조 스토리지 작업을 예약한다.
5. 스토리지 읽기가 성공하면 내부 테이블을 수정하고 페이지가 메모리에 인식되도록 한다.
6. 다시 시작 명령

### Pure Demand Paging

- 요청하지 않을 경우 절대로 메모리에 작업을 가져오지 않는 것
- 그때그때 페이지 폴트가 일어날 경우 가져온다.

## Locality of Reference(참조 국부성)

어떤  프로세스는 같은 페이지나 동작을 엑세스 해서 가져오지 않는다.

하나의 페이지가  여러 가지를 가르킨다면 오류가 생길수 있다.

하지만 분석결과, 그럴 가능성이 낮다.

프로그램은 국부성을 띄기 때문에 결과적의로 deamand paging이 좋은 성능을 낼수 있다.

### Instruction Restart

- demand paging에대한 중요한 요구사항
- 페이지 fault가 일어나면 중단된 프로세스(register.condition cdoe, instructino counter ,etc)등이 저장된다.
- 그러므로 프로세스를 재시작하고 같은 장소에 같은 상황을 만들어 낸다.
- 패치시에 실패하면 다시 패치하고, 운영에서 문제가 되면 다시 운영패치를 한다.

### Page fault

Eat = (1-p) x ma + p x (pagfault)

## Copy-on-Write

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/13571834-4a7a-4437-b97a-7c9b87561215)

프리프레임이 없을경우에 어떻게 될까?

적정 위치로 프로세스가 실행이 못하는 경우가 생길수 있다.

이런경우에 쓰이는 방식이 있는데

## 페이지 교체 알고리즘

1. 프리 프레임을 찾는다.
- 프리프레임이 없는 경우에  victim을 정하게 페이지 알고리즘이 발생한다.
2. 페이지 아웃시킨다.
3. 페이지가 비었다고 알려준다.
4. 페이지 인 시킨다.

- 여기서 Victim을 선정하는 기준을 어떻게 선정하는것인가?
- Victim을 얼마만큼의 공간을 줄것인가?

선정기준

- 페이지 폴트의 기준을 낮추는 경우를 기준으로 삼는다.(최소)
- 페이지 프레임이 많으면 페이지 폴트가 적어진다.

방식은 FIFO(first in first out)을 사용하면 된다.

## 페이지 교체 알고리즘의 방식

### Belady’s Anomaly

- 프레임을 많이 주었는데 페이지 폴트가 줄어들지 않는 현상

최적의 페이지 교체는 페이지 폴트가 일어날수 있는 상황에서만 일어나게 줄이는 것

### OPT or MIN

- 앞으로 사용 안할것 같은 페이지를 버린다 라고 정의하는 것

### LRU

가장 사용되지 않은 기간이 오래된것

- Counter / Stack을 이용할수 있다.
- S/W로는 구현하기 힘들기 때문에 하드웨어의 서포트가 필요하다.

## Thrashing

- 프로세스 페이지가 바쁘게 인앤 아웃을 하는 상황
- 이런 상황이 지속되면 CPU사용량이 작업을 하는 중임에도 떨어진다.
- Page-falut가 매우 높게 난타난다.
