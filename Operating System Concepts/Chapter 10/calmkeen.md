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
