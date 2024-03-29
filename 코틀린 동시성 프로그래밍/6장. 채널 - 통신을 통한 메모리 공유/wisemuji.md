# 채널 - 통신을 통한 메모리 공유

채널은 동시 코드 간의 안전한 통신을 허용하는 도구다. 
그들은 메시지를 보내 동시성 코드가 통신할 수 있도록 한다. 

채널은 실행 중인 스레드에 상관없이 서로 다른 코루틴 간에 메시지를 안전하게 보내고 받을 수 있는 파이프라인으로 간주할 수 있다.

## 채널 유형과 배압

채널의 send() 기능을 일시 중지하는데에 사용할 수 있다. 데이터를 실제로 수신하는 사람이 있을 때까지 요소를 전송하는 코드를 일시 중지하는 것이 좋다. 

이 개념을 '배압'이라고 하며, 수신기가 실제로 처리할 수 있는 것보다 더 많은 요소로 채널이 범람하는 것을 방지하는 데 도움이 된다.

이 배압을 구성하기 위해 채널에 대한 버퍼를 정의할 수 있다. 채널의 요소가 버퍼 크기에 도달하면 채널을 통해 데이터를 전송하는 코루틴이 일시 중단된다. 요소가 채널에서 제거되면 송신기가 재개된다.

## 언버퍼드 채널

현재 버퍼링되지 않은 채널의 구현은 랑데부 채널뿐이다. 

채널에서 send()을 호출하면 수신자가 채널에서 receive()할 때까지 일시 중단된다. 
```
val channel = RendezvousChannel<Int>()
```

다음 코드와도 동일하다.
```
val rendezvousChannel = Channel<Int>(0)
```

## 버퍼드 채널

두 번째 유형의 채널은 버퍼가 있는 채널이다. 

앞서 언급한 바와 같이, 이 유형의 채널은 채널에 있는 요소의 양이 버퍼 크기와 같을 때마다 송신자의 실행을 일시 중단한다. 

### LinkedListChannel
중단 없이 무제한으로 요소를 보낼 수 있는 채널을 원한다면 LinkedListChannel이 필요하다. 이 유형의 채널은 sender를 일시 중단하지 않는다. 
```
val channel = LinkedListChannel<Int>()
```

다음 코드와도 동일하다.
```
val channel = Channel<Int>(Channel.UNLIMITED)
```

### ArrayChannel

포함된 요소의 양이 버퍼 크기에 도달하면 sender를 일시 중단한다. Int보다 낮은 양의 값을 전송하여 생성할 수 있다.
```
val channel = Channel<Int>(50)
```
생성자를 직접 호출하여 만들 수도 있다:
```
val ArrayChannel = ArrayChannel<Int>(50)
```
이 유형의 채널은 버퍼가 가득 차면 송신기를 일시 중단한 다음 하나 이상의 항목이 검색되면 다시 시작한다.

### ConflatedChannel

이 유형의 채널에서는 배출된 원소가 손실되어도 괜찮다.

이 유형의 채널은 한 요소의 버퍼만 가지고 있으며, 새 요소가 전송될 때마다 이전 요소가 손실된다. sender가 절대 정지되지 않고 retrived 되지 않은 요소가 재지정된다

생성자를 호출하여 인스턴스화할 수 있다.
```
val channel = ConfulatedChannel<Int>()
```
파라미터로 채널() 함수를 호출하여 호출할 수도 있다.
```
val channel = Channel<Int>(Channel.CONFLATED)
```

# 요약

* 채널은 스레드(thread)에 상관없이 코루틴 간에 안전하게 메시지를 보낼 수 있는 커뮤니케이션 도구다.
* 버퍼링되지 않은 채널: 이것들은 각 요소에 대해 receive()가 호출될 때까지 send()를 일시 중단하는 채널이다.
* 세 가지 유형의 채널: 마지막으로 전송된 요소만 유지하는 ConflatedChannel, 사용 가능한 메모리에 무제한 또는 최소한 많은 요소를 저장할 수 있으므로 send()시 절대 중단되지 않는 LinkedListChannel, send() 시 일시 중단되는 ArrayChannel. 
