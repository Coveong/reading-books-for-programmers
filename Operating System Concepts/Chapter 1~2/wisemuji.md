- https://www.inflearn.com/course/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B3%B5%EB%A3%A1%EC%B1%85-%EC%A0%84%EA%B3%B5%EA%B0%95%EC%9D%98#curriculum
- 주니온 박사님 강의 중 섹션 1에 대한 정리

# 운영체제란?

### 정보란?

- 불확실한 것을 측정하여 양적(quantitative)으로 정의한 것.
<img width="453" alt="image" src="https://user-images.githubusercontent.com/32327475/210575439-f3dffea7-64a8-4664-a920-d493d193adb7.png">

### 컴퓨터는 정보를 어떻게 처리할까
- 정보의 최소 단위: bit
- 정보의 처리: 정보의 상태 변환 (0에서 1로, 1에서 0으로)
- 논리 게이트와 논리 회로
- 정보의 저장과 전송: 플립-플롭, 데이터박스

### 컴퓨터의 속성
- 범용성
  - NOT,AND,OR 게이트(또는 NAND 게이트)만으로 모든 계산을 할 수 있다.
  - General purpose computer: 범용 컴퓨터
- 계산 가능성
  - Turing-computable: 튜링 머신으로 계산 가능한 것
  - Halting Problem: 튜링 머신으로 풀 수 없는 문제

## 프로그램이란?

- 컴퓨터의 하드웨어에게 명령을 내리는 명령어의 집합

### 운영체제도 프로그램일까?

- 운영체제는 컴퓨터에서 항상 실행중인 프로그램
- 애플리케이션 프로그램에 시스템 서비스를 제공해줌
- 프로세스, 리소스, 유저 인터페이스 등을 관리한다

## 운영체제의 정의

- 컴퓨터 시스템을 operate하는 소프트웨어
- 하드웨어를 제어하고 사용자와 어플리케이션에 서비스를 제공함
<img width="897" alt="image" src="https://user-images.githubusercontent.com/32327475/210578172-535a993d-2807-4911-bd1d-5f6a3cc6d760.png">

# Chapter 1-2. Introduction & O/S Structures

<img width="571" alt="image" src="https://user-images.githubusercontent.com/32327475/210579049-0ba1d9e6-211c-4a07-acea-6eb19ffe2300.png">

## 전통적인 컴퓨터 구조
- 하나 이상의 CPU
- 연결된 디바이스 컨트롤러
- 부트스트랩 프로그램
  - 컴퓨터가 켜졌을 때 가장 먼저 시작되는 프로그램
  - 메모리에 운영체제를 로딩함
- Interrupts
  - 쉽게 말해 이벤트 (I/O 디바이스가 CPU에게 -> 키보드를 눌렀어!)
  - 하드웨어가 언제든 트리거할 수 있음

### 폰 노이만 아키텍쳐
- 전형적인 instuction-execution cycle
  1. 메모리에서 명령어 집합을 가져와서 (fetch) instruction register에 저장함
  2. 실행함 (execution)
- 저장 시스템(비 휘발성 저장 장치)
<img width="866" alt="image" src="https://user-images.githubusercontent.com/32327475/210580962-b7cf1870-704a-4a4c-b447-f2dae8e141b4.png">
- I/O 시스템
  - OS code의 상당수가 입출력을 관리하는데에 쓰임

### 컴퓨터 시스템 컴포넌트 정의
- CPU - 명령을 실행하는 하드웨어
- 프로세서 - 하나 이상의 CPU를 포함하는 물리적 칩
- 코어 - CPU의 후면 연산 장치
- 멀티코어 - 동일한 CPU의 여러 컴퓨팅 코어를 포함함
- 멀티프로세서 - 여러 프로세서를 포함함

### SMP
- 가장 일반적인 멀티프로세서 시스템
- 여러 개의 CPU가 각각의 레지스터와 캐시를 가지고 있음
- 멀티 코어 디자인: 여러 개의 CPU 칩을 들고 있는게 아니라, 하나의 CPU 칩 안에 여러 개의 코어를 가지고 있음

## 멀티프로그래밍
- 하나 이상의 프로그램을 동시에 메모리에 올려놓고 실행하는 것.
- 메모리에 여러 개의 프로세스를 동시에 올려놓고 CPU의 사용 효율을 높임

### 멀티테스킹(=멀티프로세싱)
- 멀티프로그래밍의 **논리적** 확장
  - CPU가 작업을 빠르게 전환하여 사용자가 실행 중인 각 작업과 상호 작용할 수 있다. (concurrency) 
- CPU 스케줄링:
  - 여러 개의 프로세스를 동시에 실행할 준비가 된 경우 시스템은 어떤 프로세스를 다음에 실행할지 선택해야 한다

## 운영체제의 두 모드
- 유저 모드
  - 위험한 명령을 1차적으로 막아줌
- 커널 모드

## 가상화 기술
- VMM: Virtual Machine Manager
  - 하나의 CPU에서 여러 프로세스를 실행시키듯이
  - 하나의 하드웨어에서 동시에 여러 OS를 실행시키는 것

## 다양한 컴퓨팅 환경의 운영 체제
- 모바일 컴퓨팅
- 클라이언트-서버 컴퓨팅
- 피어 투 피어(P2P) 컴퓨팅
  - 블록체인 기술에 영향을 줌
- 클라우드 컴퓨팅
- 실시간 임베디드 시스템

## 운영체제 오버뷰
<img width="817" alt="image" src="https://user-images.githubusercontent.com/32327475/210584721-9bb9968d-6d7c-4e57-be96-1d4e317e3948.png">
