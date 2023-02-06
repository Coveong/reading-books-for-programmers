
# Chapter 4. Thread & Concurrency

싱글 스레드 컨트롤

멀티 스레드 컨트롤

Tread is..

- ligthweight process.
- CPU이용에 기본적인 부분이다
- ProgramCount, thread id, register set , stack로 구성되어 있다.

![image](https://user-images.githubusercontent.com/78361650/216978353-cc2f3cc1-2350-48f3-a818-244f83d0e9c9.png)

### 쓰레드 서버 아키텍처

![image](https://user-images.githubusercontent.com/78361650/216978414-91979625-5873-4528-88d9-92560caaa9cd.png)

멀티쓰레딩의 장점

- Responsivenss
    - ui처리시 블락킹없이 처리가 가능
- Resource SHaring
    - 메모리 공유, 메시지 패싱보다 리소스 처리가 쉽다.
- Economy
    - 프로세싱 가격이 조금더 훌륭하다.
    - 컨텍스 스위칭에서도 이점을 보인다.
- Scalability (확장성)
    - 아키텍처 구조 프로세싱에도 이점을 보인다
    

Java에서 스레드를만드는 세가지 방법

- Thread 클래스 상속

```jsx
Class A extend Thread {}
```

- unnable인터페이스 구현

```jsx
Class B implements Runnable {}
```

- lamda 사용(java V 1.8 after)

```jsx
public class C {
	public static final vodi main(String[] args){
		Runnable task = () -> {
			try { runnable thread.sleep()} catch (interruptedException ie){interrupted}
		Thread thread = new Thread(task)
thread.start();
```

멀티 쓰레딩은 Multicore system이다.

코어를 효율적으로 사용해 동시성을 개선

스레드에 대한고려

- single-Core : 시간에따라서 일터리빙( 사이사이에 끼어넣는다는 느낌)
- Multiple-Core : 평행하게 스레드를 작동시킬 수 있다.

챌린지  in Multicore Systems

- identifying tasks : 영역을 별도의 작업으로 나눌수 있다.
- Balacne: 업무의 동등성을 보장하다
- Data splitting : 데이타는 코어의 분할 동작을 나눌수 있다.
- Data dependcy: 데이터의 동기화를 이루어야 한다.
- Testing and debugging : 싱글스레드보다 더 어렵다
