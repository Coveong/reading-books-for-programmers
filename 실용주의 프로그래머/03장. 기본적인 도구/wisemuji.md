# 기본적인 도구

개발자들의 도구

## 14. 일반 텍스트의 힘

> 실용주의 프로그래머 Tip 20: 지식을 일반 텍스트로 저장하라.

### 일반 텍스트로 저장했을 때의 단점

* 압축된 이진 포맷을 사용하는 것보다 많은 공간 차지
* 해석하고 처리하는 데에 더 많은 계산이 필요할 수 있음

### 일반 텍스트로 저장했을 때의 장점

* 구식이 되는 것에 대한 보험
* 호환성
* 더 쉬운 테스트

## 15. 조개 놀이

> 실용주의 프로그래머 Tip 21: 명령어 셸의 힘을 사용하라

명령어 셸에 익숙해지면 생산성이 급상승할 수 있다.

## 16. 파워 에디팅

> 실용주의 프로그래머 Tip 22: 하나의 에디터를 잘 사용하라

에디터의 기능:
* 설정변경 가능
* 확장 가능
* 프로그램 가능

## 17. 소스코드 관리

소스코드 관리 시스템은 관리하는 파일들을 중앙 저장소에 유지할 수 있다. 아카이브를 위한 훌륭한 선택이다.

> 실용주의 프로그래머 Tip 23: 언제나 소스코드 관리 시스템을 사용하라

## 18. 디버깅

> 실용주의 프로그래머 Tip 24: 비난 대신 문제를 해결하라

> 실용주의 프로그래머 Tip 25: 디버깅을 할 때 당황하지 마라

> 실용주의 프로그래머 Tip 26: 'select'는 망가지지 않았다

> 실용주의 프로그래머 Tip 27: 가정하지 마라. 증명하라.

### 디버깅 체크 리스트

* 보고된 문제가 내재하는 버그의 직접적 결과인가 아니면 단순히 증상일 뿐인가?
* 버그가 정말로 컴파일러에 있나? OS에 있나? 아니면 여러분 코드에 있나?
* 이 문제를 동료에게 상세히 설명한다면 어떻게 말하겠는가?
* 의심이 가는 코드가 단위 테스트를 통과한다면 테스트는 충분히 완전한 것인가? 이 데이터로 단위 테스트를 돌리면 무슨 일이 생기는가?
* 이 버그를 야기한 조건이 시스템의 다른 곳에도 존재하는가?

## 19. 텍스트 처리

> 실용주의 프로그래머 Tip 28: 텍스트 처리 언어를 하나 익혀라

텍스트 처리 언어를 잘만 사용한다면 예리하고 섬세하게 조작할 수 있다.

## 20. 코드 생성기

> 실용주의 프로그래머 Tip 29: 코드를 작성하는 코드를 작성하라

### 수동적 코드 생성기

용도:
* 타이핑을 줄여줌
* 새 소스 파일 생성
* 프로그래밍 언어간 일회용 변환을 수행하기
* 런타임에 계산하기엔 비용이 많이 드는 참조 테이블과 여타 자원을 생성하기

### 능동적 코드 생성기

어떤 지식을 단 하나의 형태로만 만들어놓고 애플리케이션이 필요로 하는 온갖 형식으로 변환할 수 있다.
