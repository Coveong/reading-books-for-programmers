# Background

Process - 메모리에 프로그램이 로드되어있는 상태

이번강의 주제 - 메모리를 어떻게 프로세스가 나누어 사용하는가.?

레지스터의 쌍 

- base register
- limmit register

메모리의 기본은  base regsiter보다는 크고 limit보다는 작아야 허가 된다.


![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/802e6ccd-3b6d-4947-bfbc-5888e1346345)

주소 연결 (address Binding) 과정

- 프로그램마다 주소를 연결하는 방법은 다 다르다.
- 어느 값도 단순히 선언된 이름이 호출되기 전에는 변수에 담아 컴파일러가 정한 무작위 메모리 위치에 담긴 더미 값이다. (
- 프로세스의 주소는  os의 커널이 정해주는 것이다. → 그때마다 다를수 있다는 것.
- 자원의 주소는  symblic이 되고 컴파일러가 이를 바인딩한다.
- 이과젱에서 symbolic 주소는 재배치 된다.
- 그리고 링커가 symbolic주소를 링킹해주고, 로더가 물리 주소로 바인딩해준다.

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/4d66d7fe-f1a2-48d7-bef9-aa08ce6157ee)

### Logical address vs physical address

- logical address - cpu에서 접근하고 싶어하는 주소
- physical address : 메모리 레지스터에 로드해주는 주소

위 두가지를 연결해주기 위해

- logical address space: 프로그램을 통한 연결
- physical address space - 물리적 주소를 표현하는 공간

을 분리 구분해야 한다.

## MMU( Memory management Unit)

이를 통해서 cpu와  physical memory를 연결해준다.

mmu에는  relocation register라는 것이 있는데 이는 base register의 역할을 한다.

dynamic Loading

- 메모리에서 한가지 프로세스를 작동시킬때 , 프로세스가 포함된 메모리의 파트를 모두 다 실행시켜야 한느것인가. 라는 경우에 필요한 요소이다.
- 메모리 주소를 효율적으로 모든 루틴을 필요할때 호출하게 하는 것을 뜻한다.

Dynamic Linking and shared libraries

- DLLs: dynamically linked libararies
- 프로그램이 실행될때 연결되어 사용되는 라이브러리 시스템
- staic Linking - 로더가 바이너리 코드를 링킹해서 불러온다.
- dynamic linking - 링킹을 실행시까지 연기를 해 링커가 실행중에 로드한다.
- shared library - .dll, .so파일등의 윈도우나 리눅스에서 있는 파일들을 지칭합니다.

### Contiguous Memory Allocation(연속 메모리 할당)

메모리 효율을 위해 사용

메모리에 어느 사용 부분을 통째로  할당해 넘기는 것

어떤 색션에 통째로 프로세스가 올라가기 때문에 싱글 섹션의 메모리를 가집니다.

- 프로텍션 - relocation register + limit register

### Memory allocation

메모리 할당에서의 사이징 문제를 해결하는 방법

- First-Fit: 링크드 리스트에서 처음부터 탐색하며 할당 가능한 부분에 넣는것
- Best-Fit: 가장 작은 홀부터  힙으로 확인후에 넣는 것
- Worst-Fit: 가장 큰 부분에 넣는 것

### Fragmentation

external fragmentation(외부 단편화)

- 프로세스가 너무 커서 작은 메모리블럭 단위 부분에 넣을수 없는 것

internal fragmentation(내부 단편화)

- 메모리에서 프로세스가 돌아 갈 경우 그 나누어진 작은 파티션 공간에 부분에서 할당해주고 남은 짜투리 파트

## Paging

- 정의 - 메모 관리를 프로세스 물리 주소 공간을  인접하지 않게 쪼개는 것.
- 외부 단편화를 피하게 한다.
- 압축 관련 요구사항을 피하게 한다.
- 오퍼이팅 시스템과 컴퓨터 하드웨어를 협력하게 한다.

기본적인 방법

- 물리 메모리를  고정 사이즈로 나눈다. → frame
- 논리 메모리를 같은 크기로 나눈다. → pages
- 전체 분리된 저장소로 부터 물리 주소를 완전하게 분리시킨다.

### Page number

![image](https://github.com/Coveong/reading-books-for-programmers/assets/78361650/6724450f-d47a-4db9-afb7-68dc7621cc29)

- Cpu에 의해서 메모리 정리가 되는 과정
1. 페이지번호  p를 추출해서 인덱스를 사용한다.
2. 페이지 번호에허 해당 프레임 번호f를 추출
3. 페이지 번호 p와 프레임 넘버 f를 교체한다.

### 페이지 사이즈(frame size) 정하는 법

- 하드웨어 사이즈를 정의 한다.
    - 페이지 사이즈는 무조건 적으로 2의 배수여야한다.4KB  나 1GB로 사이로 추천

하드웨어 서포트

- cpu 스케줄러가 프로그래스를 실행하면 페이지 테이블이 포인터가 하드웨어로 제어하기 힘들기때문에  PTBR(page table bsae register)를 사용한다.

### BTBR(page table bsae register)

- 테이블의 일정 파트를 가져가서 사용하겠다는 의미
- 메모리 접근 시간은 느리지만 context switch(업무 전환)은 빠르다.
- 단점 - 메모리를 두번 접근해야 한다.(page table 과 메인 메모리)

### 효과적인 메모리 접근 시간

TLB hit - 찾으려 하는 정보가 TLB 안에 있을때 

TLB miss - 찾고자 하는 정보가 TLB에 없을때 ( 메인을 접근한뒤 다시 오게됨)

hit ratio - 페이지에 접근하고 이동하는 시간

### 메모리 페이징 보호

- 프레임별로 비트를 추가해서 유효성검사를 실행한다.
- 유효한 경우 - 물리 프로세스에 추가되어있기 떄문에 legal
- 아닐경우 는 논리 프로세스 주소에 추가된거기 때문에 illegal

### 공유 페이지

페이징 처리된 부분을 멀티 프로그래밍 환경에서 공용화 하는 것

만약 코드가 실행중에 변경될일이 없는 경우(ex. libc)

### 페이지 테이블의 구조설계

커진 물리 주소공간을 관리하는 3가지 방법

- Hierarchical Paging - 페이지 테이블이 관리하기 힘들정도로 커지면 페이지테이블을 한번 더 쪼개서 관리한다.
- hashed Page Table - 해쉬테이블의 개념을 물리주소에 입혀서 만든다.
- Inverted Page Table - pid 를 통해 어떤 페이지테이블을 가지고 있는지 확인해서 관리한다.

## Swapping

- 피지컬 프로세스보다 큰 경우가 존재하지 않을까라는 경우에서 파생되었다.
- 메모리가 필요한 부분만 사용하고 나머지 부분은 메모리 밖에 있다가 스왑형식으로 접근시킨다.
- 전체 프로세스를 스왑하면 부담이 너무 크기때문에 페이지개념을 입혀서 페이지 단위로 스왑시킨다.
    - page out - 메모리 밖으로 페이징된 부분을 보내는것
    - page in : 페이지를  가상메모리에서 실제 메모리로 다시 가져오는 것
