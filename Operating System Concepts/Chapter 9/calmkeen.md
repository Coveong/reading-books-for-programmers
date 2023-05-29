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
