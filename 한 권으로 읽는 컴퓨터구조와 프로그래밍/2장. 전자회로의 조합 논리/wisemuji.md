# 전자회로의 조합 논리

비트에 대해 동작하는 장치를 포함해 모든 물리적인 장치를 '하드웨어라고 부른다'. 이 장에서는 '조합 논리'를 구현하는 하드웨어에 대해 살펴볼 것이다.

## 아날로그와 디지털의 차이

- 아날로그: 연속적인 것
- 디지털: 이산적인 것

### 아날로그 세계에서 디지털 만들기

카메라 센서나 필름의 `전이 함수`:

![image](https://user-images.githubusercontent.com/32327475/170872270-35bc755f-6cb3-499c-917c-83634410c322.png)

x축은 들어오는 빛의 양(입력)을 뜻하며 y축은 (센서나 필름에) 기록되는 밝기나 센서가 기록하는 빛(출력)을 의미한다.

- 빛이 곡선의 상단부에 많이 닿으면 필름이나 센서에 기록되는 밝기 값이 서로 모이면서 이미지 노출이 과해짐
- 반대로 하단에 많이 닿으면 이미지 노출이 부족해짐

카메라의 경우 목표는 노출을 조절해서 빛이 직선부에 많이 닿게 만드는 것이다.

아날로그는 가능한 한 선형 영역을 크게 만들기 위해 노력하는 것이고 디지털은 직선부를 가능하면 작게 만드는 것이라고 생각할 수 있다.

## 10진 숫자 대신 비트를 사용하는 이유

가장 큰 이유는 전이 함수를 각기 다른 10가지 문턱값으로 구분할 수 있는 간단한 방법이 없기 때문

# 간단한 전기 이론 가이드

## 전기는 수도 배관과 유사하다

- 한 밸브의 출력이 다른 밸브의 입력에 연결될 경우 직렬 연결 - AND 연산
- 두 밸브의 입력을 한 관에 함께 연결하고 구 밸브의 출력을 다른 관에 함께 연결하면 병렬 연결 - OR 연산
- 물이 파이프를 흘러서 전달되는데 시간이 걸리듯 전기가 칩 내부에서 전파되는데도 시간이 걸림 - 전파 지연
- 수압 - 전압
- 측정 단위 - 볼트
- 전기 흐름의 양 - 전류
- 측정 단위 - 암페어

# 비트를 처리하기 위한 하드웨어

## 릴레이

- 릴레이: 스위치를 움직이기 위해 전자석을 사용하는 장치
- 평상시 열린 릴레이: 코일에 전력이 들어가지 않아서 스위치가 열려 있는 릴레이
- 평상시 닫힌 릴레이: 전력이 들어가지 않을 때 스위치가 닫혀 있는 릴레이

릴레이를 사용하면 NOT 함수를 구현하는 인버터, 코일을 두개만 사용하는 단극십투 스태퍼 릴레이 등 스위치로는 불가능한 일을 할 수 있다.

릴레이는 느리고 전기를 많이 소모하며 먼지가 스위치 접점에 있으면 제대로 작동하지 않는다는 큰 문제가 있다.

## 진공관

물체를 충분히 가열하면 전자가 튀어나오는 열전자 방출이라는 현상 기반으로 만들어짐

## 트랜지스터

최근에는 트랜지스터가 왕이다. 진공관과 비슷하지만 반도체라는 특별한 물질을 사용함.

- 쌍극 접합 트랜지스터
- 필드 효과 트랜지스터

트랜지스터를 사용하면 더 작고 빠르고 신뢰할 수 있으며 전력도 적게 소모하는 논리 회로를 만들수 있게 되었다.

## 집적 회로

트랜지스터의 단점인 AND 함수같은 간단한 회로를 만들 때 부품이 너무 많이 필요하다는 점을 개선한 집적 회로가 있다. (생긴 모양 때문에 칩이라고 불렸음)

복잡한 시스템을 트랜지스터 하나를 만드는 정도의 비용으로 만들 수 있다.

# 논리 게이트

게이트를 사용하면 하드웨어 설계자가 밑바닥에서부터 모든 회로를 설계할 필요 없이 IC를 선으로 연결해 복잡한 회로를 쉽게 만들 수 있다.

## 몇 가지 게이트 조합

- 가산기
- 디코더
- 디멀티플렉서
- 실렉터
