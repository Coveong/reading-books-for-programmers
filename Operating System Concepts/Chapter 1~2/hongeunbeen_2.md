# 02. 운영체제의 개념과 구조

## 컴퓨터 시스템

![Untitled](https://user-images.githubusercontent.com/37995685/217673222-fc01174c-d7d3-4a61-a60f-2a886cc6cf41.png)


- hardware
- OS
- application programs
- user

컴퓨터에서 항상 운영중인 프로그램 = OS

→ 가장 대표적인게 kernel

## Kernel

OS의 핵심..!!

system programs와 appliation programs에 대한 인터페이스를 제공해줌

## Classic Computer System

![Untitled 1](https://user-images.githubusercontent.com/37995685/217673234-b96b525d-0f68-4553-8aa1-0057c8d9dddf.png)

- 한 개 또는 여러개의 CPU
- 여러개의 디바이스와 연결할 수 있는 common bus

→ CPU는 버스를 통해서 RAM, disk, adapter 등등과 연결되어 있음

위와 같은 시스템은 OS가 제어해 줘야 함

## bootstrap program

컴퓨터의 전원을 키면 CPU가 처리해야 할 명령어들

파워 온 되자마자 처음으로 동작해야 할 프로그램

HDD에 있는 OS를 메모리에 로드하는 역할을 하는 프로그램을 실행함

운영체제를 메모리에 로딩~

## Interrupts

![Untitled 2](https://user-images.githubusercontent.com/37995685/217673254-69ff516a-f699-4f10-856f-b47f296ae4de.png)


키보드같은 I/O 디바이스에서 입력값을 CPU에 전달할 때 interrupts라는 방법으로 알려줌

- CPU와 프로세스(주변 장치 등등)가 통신하는 방법 중 하나 interrupts
- I/O 디바이스에서 idle하고 있다가 CPU가 받아서 처리하는 것

hardwre가 언제라도 버스를 통해 인터럽트를 트리거 시킬 수 있고 CPU에 전달 할 수 있음

## storage systems

비휘발성 저장 장치는 용량과 접근 속도에 따라서 여러개의 계층으로 존재함

![Untitled 3](https://user-images.githubusercontent.com/37995685/217673265-659bb84f-ac0f-4c1f-b1ec-bf8caf257073.png)


- registers
    - CPU안에 있는 공간으로 제일 빠름
- cache
    - ram보다 빠르지만 비싸기 때문에 용량을 적게 사용함
- main memory
    - RAM
- solid-state disk
    - SSD로 메모리 형태의 HDD를 수행함
- hard disk
    - 자기장을 이용한 HDD
- optical disk
- magnetic tapes
    - backup 용도로 사용함 (은행.. )

## I/O Structure

OS 코드를 구현할 때 거의 대부분이 managing I/O임… (커널 개발은 안정화 되어 있음)

![Untitled 4](https://user-images.githubusercontent.com/37995685/217673289-eb5ec760-94cb-45b6-9826-27d857c87c78.png)

- thread of execution
    - 실행되는 과정으로 CPU가 이를 가지고 있음
    - 캐시 메모리를 통해서 RAM에 access하고..
- device
    - I/O 디바이스가 interrupts를 하고 데이터 주고 받음
- DMA (Directed Momory Access)
    - interrupts 구조 증 하나
    - 디바이스끼리 연결되는 것으로 network와 LCD가 직접 연결하는 그런 것들…

## Computer System Components

- CPU
    - 명령을 처리해줌
- Processor (실행중인 프로그램)
- Core
- Multi

## Symmetric multiprocessing (SMP)

![Untitled 5](https://user-images.githubusercontent.com/37995685/217673307-ee920ead-aa04-4861-8415-68340091b62a.png)

메모리는 한 개지만 연결되어 있는 CPU가 여러개로 각각의 registers와 cache를 가짐

## Multi-core design

![Untitled 6](https://user-images.githubusercontent.com/37995685/217673320-f2bbdc5e-4a56-4f72-b304-5aa7a79de413.png)

CPU 자체를 여러개 두는 것은 비용이 많이 발생하기 때문에 한개의 CPU(같은 프로세스 칩)안에 CPU core만 따로 회로를 구성함

위와 같은 CPU들이 여러개가 붙으면 multi processor

## Multiprogramming

![Untitled 7](https://user-images.githubusercontent.com/37995685/217673331-b303ad32-4f9e-439c-bae9-7801565cf978.png)

예전에는 메모리에 한 개의 프로그램만 로딩해서 사용 후 버리고 로딩하고 함..

여러개의 프로그램을 메모리에 올리고 동시에 실행 → multiprogramming

메모리에 여러개의 processes가 동시에 올라가서 CPU 사용 효율을 높임

⇒ 그저 메모리에 한 번에 올린다의 개념

## Multitasking (=multiprocessing, = cocurrency?)

멀티 프로그래밍 → CPU가 시간을 쪼개서 사용함 (여러개의 프로세스를 자주 바꿔줌)

⇒ 이를 CPU가 가져와서 tiem chnage하면서 실행하는 것

⇒ CPU가 가져와서 실행한다의 개념

## CPU scheduling

CPU가 실행할 processes을 선택하는 방법

목표: CPU 효율을 최대로 사용할 수 있는가

## Opertaions mode

![Untitled 8](https://user-images.githubusercontent.com/37995685/217673347-1bdfbebe-a7ae-4966-bbd2-5a51ad215e6b.png)

- user mode
- kernel mode
1. presess를 실행하다 system call(OS에게 요청)발생 
2. 이때 kernel mode로 변경해서 시스템 콜을 처리 후 다시 되돌려줌

→ kernel mode가 아니면 직접적으로 HW를 제어할 수 없게 만들기 위해 mode가 나눠짐!

## Virtualization

가상화 기술로 HW에서 OS를 여러개 돌리는 기술로 HW와 OS사이에 VMM을 위치시킴

- VMM (Wirtual Machie Manager)
    - VMware, WSL

![Untitled 9](https://user-images.githubusercontent.com/37995685/217673357-959c6ae7-05ec-4833-9287-b2e91adab510.png)

CPU 스케줄링 하듯이 VMM에서도 OS 스케줄링..! (context swtiching)

## Variety of Computing Environments

- Traditional Computing
- Mobile Computing
    - AOS, IOS
- Client-Server Computing
    - Web Server - Web Browser
- Peer-to-Peer Computing
    - 음악 파일 공유.. (bittonent?)
    - 파일 조각들을 나눠서 중앙 서버 없이도 운영되게..!
    - bitcoin (clock chain)
- Cloud Computing
    - AWS, Azure, GCP
- Real-Time Embedded Systemes
    - 화성 탐사 등 Vxworks 라즈베리..

## OS Services

![Untitled 10](https://user-images.githubusercontent.com/37995685/217673380-ba805a9f-ec0a-4eeb-9b68-ac22b5cac3ad.png)

## System Calls

- CLI: command line interface
    - shells…
- GUI: graphical user interface
    - windows, aqua for macOS
- Touch-Screen Interface

→ 사용자가 인터페이스 하는 방법들임

프로그램이 OS에 인터페이스하기 위해선 System calls을 사용함

![Untitled 11](https://user-images.githubusercontent.com/37995685/217673390-361f8650-ebf0-454d-a9d0-1fcc2d801bdc.png)


OS가 제공해주는 서비스들은 System Call을 통해 호출함 = API

- function read …

OS의 API = System calls

![Untitled 12](https://user-images.githubusercontent.com/37995685/217673398-ab901ef7-5569-47db-8a78-c726b7a69381.png)


System call interface를 통해서 사용자가 open해서 read, write 등등을 함

→ 매번 번거로움… ⇒ 대부분은 library를 제공함

- standard C library
    ![Untitled 13](https://user-images.githubusercontent.com/37995685/217673420-ba4eea89-71cd-4ee9-8675-a04cfc3f203a.png)

    
